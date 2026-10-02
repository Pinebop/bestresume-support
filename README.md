# bestresume-support

小河狸（bestResume for iPhone）的静态技术支持站，用 GitHub Pages 托管。无构建步骤、无 JavaScript。

- **技术支持页：** https://pinebop.github.io/bestresume-support/ ——填入 App Store Connect 的「技术支持网址」
- **隐私政策：** https://pinebop.github.io/bestresume-support/privacy.html ——填入 App Store Connect 的「隐私政策网址」（App 信息页的独立字段）

## 文件

纯 HTML/CSS，直接改：

- `index.html` ——技术支持页（功能说明、常见问题、联系方式）
- `privacy.html` ——隐私政策
- `styles.css` ——共用样式（`prefers-color-scheme` 自适应浅色/深色，配色取自
  `app/templates/assets/brand-theme.json`）
- `assets/icon.png` ——App 图标（来自 `app/ios/App/Assets.xcassets/AppIcon.appiconset/icon-1024x1024.png`）

推送到 `main` 后会自动重新部署（通常一分钟内生效）。

## 口径必须与代码一致

隐私政策里的每一句都对应 App 的真实行为，改 App 的联网/存储行为时**必须回来同步本页**：

- 三处联网链路（经历扩写、意见反馈、订阅验签）的事实源是
  bestResume 仓库的 `docs/data-flow.md` 第 7–9 节；
- 契约（字段、PII 清洗、身份令牌与客户端环境字段）以 bestResumeServer 仓库的 `docs/api-contract.md` 为准；
- 不申请系统权限、照片走 PHPicker 的结论来自 `app/ios/App` 的 Info.plist 与选图实现。

还有两条容易写错、写前必须回头核的：

- **文本会被转交第三方大模型**。`config.release.toml` 的 `[llm]` 段默认 provider 是智谱
  （fallback DeepSeek），所以不能写「不经过任何第三方」。App 只连自建服务器，第三方在服务端。
- **请求文本会出站并经服务器转交第三方大模型**，政策里不能只说「本机数据」。
  `polish_record` 只存请求元信息与身份（`endpoint` / `provider` / `model` / 延迟 /
  `identity = app:<appTransactionID>`）；简历原文与生成结果的落盘只在排障时开启的
  `content.log`（仓库配置默认关闭，线上以 `.env` 为准），见 bestResumeServer
  `src/best_resume_server/db/tables.py` 与 `config/config.release.toml`。

**注意：** App 内「我的信息 → 隐私政策」（`app/ios/App/PrivacyPolicyView.swift`）已与
本页同步（2026-10-02，bestResume merge `1fadcd1`：去随机设备标识、与身份 v3 对齐）；
提审前再逐段核对两处一致。

## 支持渠道

本仓库的 Issues 是公开支持渠道；为保持 App Store 链接有效，**本仓库必须保持公开**。
App 内「意见反馈」是另一条直达开发者的渠道。

**支持邮箱 `kenshinlzh@163.com`** 是隐私相关事务（尤其是数据删除申请）的正式渠道，
隐私政策第 8、11 节均指向它；换邮箱时这两处、App 内 `PrivacyPolicyView.swift` 与
本仓库 README 要一起改。
