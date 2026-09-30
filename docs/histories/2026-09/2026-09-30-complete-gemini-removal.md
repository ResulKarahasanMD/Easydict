## 2026-09-30 | 任务：补全 fork 中 Gemini 服务的移除

**Links:** f2a53895（删除 `GeminiService.swift`）、9822a57e（合并 upstream `dev`）

### 执行上下文

- **Agent Name:** `Claude Code`
- **Model:** `claude-opus-5-5`
- **Environment:** `macOS 26.6.2 / Xcode 27.0 (27A266a)`

### 用户请求

检测 fork `dev` 分支的构建错误并修复。

### 变更

- 从 `Easydict.xcodeproj/project.pbxproj` 移除 `GeminiService.swift` 的 PBXBuildFile、PBXFileReference、
  `Gemini` group 及其在父 group 和 Sources build phase 中的引用。
- 从 `QueryServiceFactory.serviceRegistrations` 移除 `.gemini` 注册项。

### 设计意图

f2a53895 只删除了源文件，工程引用和工厂注册仍在，导致 `xcodebuild build` 报
`Build input file cannot be found`，Swift 编译无法开始。本次按该提交的意图补全移除，
而不是恢复文件。服务列表由 `serviceRegistrations` 派生，`service(withTypeId:)` 找不到注册时
返回 `nil`，`services(fromTypes:)` 使用 `compactMap`，因此已保存的 `Gemini` 服务类型会被跳过。

`GoogleGenerativeAI` SPM 依赖、`EZServiceTypeGemini`、`APIKey.geminiAPIKey` 和本地化 key
保持不变，以减少与 upstream 合并时的冲突。

本地 Debug 签名通过被忽略的 `Easydict-debug.local.xcconfig`（`scripts/setup-team.sh` 生成）
配置，不属于仓库差异。

### 验证

- 修复前 `xcodebuild build`：失败（exit 65），唯一错误为缺失 `GeminiService.swift`。
- 在临时 APFS 副本中恢复 `GeminiService.swift` 后 `xcodebuild build`：成功，确认没有其他被掩盖的编译错误。
- 修复后 `xcodebuild build`：成功（exit 0，0 error；唯一 warning 为 AppIntents 元数据跳过）。
  产物 `codesign -v` 通过，`TeamIdentifier=3V5WCZ8D9T`。
- `plutil -lint Easydict.xcodeproj/project.pbxproj`：通过。
- `git diff --check`：通过。
- 未运行 `xcodebuild test`。

### 受影响文件

- `Easydict.xcodeproj/project.pbxproj`
- `Easydict/Swift/Service/Model/QueryServiceFactory.swift`
- `docs/histories/2026-09/2026-09-30-complete-gemini-removal.md`

### 后续事项

- 主 target Release 配置仍为 upstream 的 `Developer ID Application` / `45Z6V4YD5U`，
  9822a57e 合并时丢弃了 fork 的 Release 签名修改；本机没有该团队的证书，Release 构建会签名失败。
- 未使用的 `GoogleGenerativeAI` 依赖可在需要时移除。
