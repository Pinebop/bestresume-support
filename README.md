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
- 契约（字段、PII 清洗、设备标识）以 bestResumeServer 仓库的 `docs/api-contract.md` 为准；
- 不申请系统权限、照片走 PHPicker 的结论来自 `app/ios/App` 的 Info.plist 与选图实现。

还有两条容易写错、写前必须回头核的：

- **文本会被转交第三方大模型**。`config.release.toml` 的 `[llm]` 段默认 provider 是智谱
  （fallback DeepSeek），所以不能写「不经过任何第三方」。App 只连自建服务器，第三方在服务端。
- **服务端会留存精修关键词与结果**。`PolishRecord` 表存 `keywords` / `result` / `device_id`
  （见 bestResumeServer `src/best_resume_server/db/tables.py`），政策里不能只说「本机数据」。

**注意：** App 内「我的信息 → 隐私政策」（`app/ios/App/PrivacyPolicyView.swift`）仍是旧文案
（写着「完全离线、不发起任何网络请求」），与本页不一致。提审前需要把它改成与本页相同的口径。

## 支持渠道

本仓库的 Issues 是公开支持渠道；为保持 App Store 链接有效，**本仓库必须保持公开**。
App 内「意见反馈」是另一条直达开发者的渠道。

**支持邮箱 `kenshinlzh@163.com`** 是隐私相关事务（尤其是数据删除申请）的正式渠道，
隐私政策第 8、11 节均指向它；换邮箱时这两处、App 内 `PrivacyPolicyView.swift` 与
本仓库 README 要一起改。
