# Scholar metrics image

供教师主页使用的 Google Scholar 指标图片。`scholar-metrics.svg` 初始内容为原网站提供的 **775 / 15 / 18**，图片明确标注 **2026-09-06 搜索摘要快照、可能滞后**。它不是本次实时读取，不含虚构的成功更新时间；首次成功刷新前不提供实时指标 JSON。

## 源码与检查

完整可审计的部署源码保存在 `scholar-source.zip`：两个 Node.js 脚本、离线测试及合成 fixtures、`docs/` 和本说明。压缩包不含 `.github` 或 `.git`；实际工作流独立位于 `.github/workflows/scholar-metrics.yml`。这种打包方式方便通过 GitHub 网页上传普通文件。

解压后，在解压目录使用 Node.js 22 或更高版本运行：

```text
node --test
```

测试用数值均为合成数据，不会写入公开指标文件。生产更新脚本仅请求固定的公开 Scholar 主页一次，核验个人 ID、累计列和三项指标；不使用代理、不处理验证码、不反复重试，也不需要 npm 包或 API key。

## 发布与刷新

仓库默认分支必须是 `main`；GitHub Pages 的 Source 设为 **GitHub Actions**。

- `main` 提交源包或工作流时：只发布现有卡片，不请求 Scholar。首次即可显示有来源日期的旧快照。
- 每周一北京时间 **09:17**：尝试读取 Scholar，成功才生成新的 JSON 和 SVG。GitHub 调度可能延迟。
- 手动运行时，`refresh` 默认勾选；取消勾选可只重新发布现有卡片。

工作流解压源码后，先用仓库根目录已有的 SVG/JSON 覆盖包内版本，防止更新源包时退回旧快照。只有真实刷新和图片生成均成功，才提交根目录的 `scholar-metrics.json`、`scholar-metrics.svg`，并部署图片。访问受限或验证码会使刷新失败，已有线上图片和成功日期保留；不能保证每周一定得到新值。

源码中的 `updated_at` 只记录成功读取时间，SVG 将其换算为北京时间日期。初始搜索摘要快照不冒充成功读取数据。

部署成功后的图片地址为：

https://dawn-ecnu.github.io/scholar-metrics/scholar-metrics.svg

教师主页使用普通 `<img>` 引用该外链，无需自定义脚本。应检查学校编辑器没有把外链转存为校内静态图片，并确认实际浏览器加载、手机可读性及国内访问情况。是否发布、是否成功同步，以 Actions 记录、图片中的日期和真实网址响应为准。
