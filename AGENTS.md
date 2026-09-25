# SenPublic - 公共资源子仓库

模块端通过 GitHub Raw 直链消费本仓库内容，**推送本仓库即完成内容更新，无需发版 App**。

## 文件清单

| 文件 | 职责 |
|------|------|
| `CHANGELOG.md` | 更新记录，`MainViewModel.fetchChangelog()` 拉取展示 |
| `sen_about.json` | 关于页动态内容：贡献者 + 开源库致谢，`data/AboutContentRepository` 拉取解析 |
| `README.md` / `banner.png` | 项目介绍与功能说明 |
| `.github/` | GitHub Issue 模板（bug 报告 / 功能建议） |

## sen_about.json 规范

```json
{
  "version": 1,
  "contributors": [
    { "name": "显示名", "github": "GitHub 用户名" }
  ],
  "libraries": [
    { "name": "库名", "author": "作者", "license": "协议", "url": "主页链接" }
  ],
  "references": [
    { "name": "库名", "author": "作者", "license": "协议", "url": "主页链接", "note": "参考说明" }
  ]
}
```

- `contributors` 项也支持纯字符串形式（GitHub 主页链接或裸用户名），自动提取头像。
- `libraries.url` 若为 GitHub 仓库/组织链接，关于页自动加载所有者头像（42dp 圆形）；非 GitHub 链接显示首字符占位。
- `references` 为可选的顶层字段，收录设计参考类条目（如 InstallerX-Revived / LSPosed / Xposed），项内可带 `note` 说明；展示对应仓库 `README.md` 的「设计参考」小节。客户端 `AboutContentRepository` 当前只解析 `contributors` / `libraries`，不读取 `references`。
- 解析端容错：字段缺失、JSON 非法均静默忽略并保留旧数据；`name` 缺失的 library 条目会被跳过。
- 客户端有 5 分钟刷新节流；内容只来自远端，不读写磁盘缓存、无内置默认名单，远端不可用时展示联网加载占位。

## 注意事项

- 修改 `sen_about.json` 后建议用 JSON 校验工具确认合法再推送。
- 本仓库为公开内容源，勿放置敏感信息。
