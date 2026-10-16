# TIPIVO Official Website

英文静态官网，使用 HTML5、CSS3 和 Vanilla JavaScript。无后端、数据库、外部字体、CDN 或运行时依赖。可部署到 GitHub Pages，支持 `/tipivo-website/` 子路径。

## 预览

直接打开 `index.html` 即可浏览完整页面，无需安装依赖。也可以在仓库根目录执行：

```sh
python3 -m http.server 8000
```

访问 `http://localhost:8000`。手机预览需让电脑与手机处于同一网络，访问电脑的局域网 IP 和端口。

## 内容维护

统一修改 `site.config.json`，然后使用 Node.js 18 或更高版本重新生成页面：

```sh
node scripts/build.mjs
```

生成结果已提交在工作目录中，GitHub Pages 无需运行 Node.js。请修改配置或 `scripts/build.mjs` 模板，避免直接修改生成的 HTML 后被覆盖。

- `supportEmail`：网站支持邮箱；空字符串时隐藏邮箱链接。
- `apps`：产品列表，新增产品只需新增一个配置对象和相关图片，再运行生成脚本。
- `slug`：小写字母、数字和连字符组成的稳定产品路径。
- `name`、`tagline`、`description`、`introduction`、`features`：实际确认的产品介绍。
- `icon`、`screenshots`：相对仓库根目录的本地图片路径；每张图片包含 `src` 和 `alt`。
- `appStoreUrl`：真实 App Store HTTPS 地址；为空时不显示下载按钮。
- `apps[].supportEmail`：产品专用邮箱；为空时转到网站 Support。
- `logo`：工作室 Logo 的本地图片路径，用于全站品牌区域和网站图标。
- 四份法律文档在 `legal-content/` 中独立维护，详见下文。
- `siteUrl`：正式网站根地址，必须以 `/` 结尾，用于 canonical、sitemap 和 robots。

当前产品名 PhotoSpace 和功能介绍取自用户提供的 App Store 宣传图，正式发布前请确认产品名称。三张图片标注为宣传图，未将其作为真实界面截图宣称。

## 法律文档

网站文档与 App 文档分开：

- 网站预留：`privacy.html`、`terms.html`
- 产品预留：`apps/<slug>/privacy.html`、`apps/<slug>/terms.html`

全站 Footer 的 **Privacy Policy**、**Terms of Use** 分别指向官网的 `privacy.html`、`terms.html`。PhotoSpace 详情页的法律链接指向 `apps/photospace/privacy.html`、`apps/photospace/terms.html`。

四份正文分别位于：

- `legal-content/privacy.json`：公司官网隐私政策。
- `legal-content/terms.json`：公司官网使用条款。
- `legal-content/apps--photospace--privacy.json`：PhotoSpace 隐私政策。
- `legal-content/apps--photospace--terms.json`：PhotoSpace 服务条款。

PhotoSpace 的隐私政策与使用条款已填入用户提供的英文正文，生效及更新日期为 October 16, 2026，已生成正文页面并加入 sitemap。联系方式中的 `[Developer Name]` 和 `[Support Email]` 仍需替换为实际信息。网站自身的两份文档仍为 `published: false`、`sections: []`，标记“未发布”并带有 `noindex, nofollow`，不进入 sitemap。

收到用户提供的正式正文后，将对应 JSON 的 `sections` 填为章节数组，每章包含 `heading`（标题，可为空）和 `paragraphs`（正文字符串数组）。按正文填写 `effectiveAt` 和 `updatedAt`（均可为空），审核后设置 `published: true`，运行 `node scripts/build.mjs`。生成器会在相同路径输出正文，移除未发布提示及 noindex，并加入 sitemap。正文作为纯文本转义输出；不要填 HTML。不得自行编写数据收集、SDK、服务器存储等行为。

新增 App 时会自动创建独立的空文档源文件和预留页面，已有正文不会被生成器覆盖。

## GitHub Pages 部署

1. 将本次文件提交并推送到 `a724632952/tipivo-website` 的发布分支。
2. 在 GitHub 仓库 **Settings → Pages** 中使用 **Deploy from a branch**，选择发布分支、`/ (root)`。
3. 等待 GitHub Pages 部署成功，访问 `https://a724632952.github.io/tipivo-website/`。

本次没有改动 Git 配置，没有自动提交或推送。无需付费服务；GitHub Pages 适用额度以内无额外服务器成本。

自定义域名：在 GitHub Pages 设置中绑定已拥有的域名并配置 DNS；将 `siteUrl` 改为正式 HTTPS 根地址，重新运行生成脚本并发布。资源和内部链接始终使用相对路径，无需重写 `/tipivo-website/`。

## 结构

```text
site.config.json       品牌、产品、支持和正式法律文档链接
scripts/build.mjs      静态页面和 SEO 文件生成器
assets/style.css       响应式样式、焦点状态和减少动态效果支持
assets/main.js         可访问的移动导航交互
assets/images/         本地图标及宣传图（已缩小适配网页）
index.html             首页和 Apps 列表
about.html             工作室介绍
support.html           邮件支持入口
apps/photospace/       独立产品页和法律文档预留路径
sitemap.xml            正式内容页面索引
robots.txt             搜索引擎抓取入口
```

无 JavaScript 时正文、产品介绍和移动端/桌面导航仍可读取。手机菜单使用 JavaScript，支持 Escape 关闭、焦点状态及 `aria-expanded`。动效仅用于交互过渡，并尊重 `prefers-reduced-motion`。

## 发布前待提供

- 确认 PhotoSpace 正式名称和功能文案。
- App Store 地址（如已上架）。
- 支持邮箱。
- 网站隐私政策、网站使用条款的正式正文。
- PhotoSpace 协议中的开发者名称与支持邮箱。
