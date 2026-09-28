# raoqingjia.github.io

GitHub Pages 站点，用于对外提供 **Apple App Site Association (AASA)** 文件，
支撑 iOS Universal Links（配合 NFC 标签唤起 App）。

## 服务地址

```
https://raoqingjia.github.io/.well-known/apple-app-site-association
```

> 注意：仓库名必须为 `raoqingjia.github.io`（User Pages）才会挂在域名根路径下。
> 若仓库名不是它，地址会变成 `https://raoqingjia.github.io/<仓库名>/.well-known/...`，
> 带上一层子路径，Apple 会拒绝。

## 文件说明

| 文件 | 作用 |
| --- | --- |
| `.well-known/apple-app-site-association` | AASA 本体，**无扩展名**，UTF-8 无 BOM |
| `.nojekyll` | 关闭 Jekyll。不加它，以 `.` 开头的目录不会被发布，AASA 会 404 |
| `index.html` | 站点落地页（未安装 App 的访问者兜底） |
| `_headers` | 给 Netlify / Cloudflare Pages 用，强制 `Content-Type: application/json`；GitHub Pages 会忽略 |
| `.gitignore` | 排除 `.workbuddy/` 等本地文件，避免被公开发布 |

## 已知限制

GitHub Pages 对无扩展名文件返回 `Content-Type: application/octet-stream`，
而 Apple 要求 `application/json`，因此 **GitHub Pages 单独无法通过 Apple 校验**。

真机验证 Universal Links 前，需改由 Netlify / Cloudflare Pages 托管（`_headers` 生效），
或使用自有域名并在前面套一层可改响应头的 CDN。

## 校验

```bash
curl -I https://raoqingjia.github.io/.well-known/apple-app-site-association
```

要求：`200`（不能是 301/302）且 `Content-Type: application/json`。
