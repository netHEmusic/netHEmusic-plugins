# netHEmusic 插件商店

netHEmusic 的插件市场清单就在这里。软件里打开 **插件 → 插件市场**，拿到的就是本仓库的
[`plugins.json`](plugins.json)。

---

## 这里的东西是自动生成的

`plugins.json` 由 [`.github/workflows/update.yml`](.github/workflows/update.yml) **每天自动生成**，
不要手动改（改了也会被下次覆盖）。

生成规则见 [`.github/market/build.mjs`](.github/market/build.mjs)：

1. 用 GitHub 搜索接口找出所有带 **`nethe-plugin`** 话题的公开仓库
2. 读每个仓库**根目录**的 `manifest.json`
3. 聚合成 `plugins.json`

> 为什么在 Actions 里聚合、而不是客户端现查：GitHub 搜索接口未登录只有 **10 次/分钟**，
> 客户端每开一次市场页就要发几十个请求，必然被限流。放 CI 里客户端只拉一个静态 JSON。

---

## 怎么把自己的插件上架

不用提 PR，也不用注册什么账号：

1. 把插件做成一个**公开仓库**
2. 仓库**根目录**放 `manifest.json`
3. 给仓库加 **`nethe-plugin`** 话题（仓库页右上角 ⚙ About → Topics）

第二天就自动出现在市场里。想立刻看到效果，可以在 Actions 页手动跑一次 **update**。

### 打包要求

下载地址**优先**用仓库最新 Release 里的 `.zip` 资源；没有 Release 就用 GitHub 自动生成的源码 zip。

不管哪种，zip 里必须满足下面之一，否则客户端解压后找不到 manifest：

- 根目录直接有 `manifest.json`
- 或者只有**一层**子目录，`manifest.json` 在那层目录里

### `manifest.json` 要写什么

完整字段说明见插件开发文档：
**[netHEmusic → plugins/README.md](https://github.com/netHEmusic/netHEmusic/blob/main/plugins/README.md)**

最小可用的长这样：

```json
{
  "manifest_version": 1,
  "id": "my-plugin",
  "name": "我的插件",
  "version": "1.0.0",
  "author": "你的名字",
  "description": "一句话说明",
  "permissions": ["ui", "events"],
  "injects": { "Main": [ { "file": "./main.js" } ] }
}
```

> `id` 不写就用仓库名。安装后插件会被放进
> `%APPDATA%\netHEmusic\plugins\<id>\`。

---

## 本组织下的仓库

| 仓库 | 说明 |
|---|---|
| [netHEmusic](https://github.com/netHEmusic/netHEmusic) | 主程序（WinUI3 + WebView2 桌面客户端） |
| **netHEmusic-plugins**（本仓库） | 插件商店：市场清单 + 收录规则 + 每日自动更新 |
| [netHEmusic-plugin-example-hello](https://github.com/netHEmusic/netHEmusic-plugin-example-hello) | 示例插件，也是上架范例 |

---

## 自己托管市场

不想用这个官方市场的话，也可以自己写一份清单自己托管，然后在
`config.ini` 里指过去：

```ini
[Plugins]
market_url = https://你的地址/plugins.json
```

清单格式：

```json
{
  "manifest_version": 1,
  "plugins": [
    {
      "id": "my-plugin",
      "name": "我的插件",
      "version": "1.0.0",
      "author": "你的名字",
      "description": "一句话说明",
      "permissions": ["ui", "events"],
      "download": "https://example.com/my-plugin.zip"
    }
  ]
}
```

| 字段 | 必填 | 说明 |
|---|---|---|
| `id` | 是 | 安装后的文件夹名，只能含字母数字下划线短横 |
| `name` / `version` / `author` / `description` | 否 | 展示用 |
| `permissions` | 否 | 展示用，安装前会展示给用户确认 |
| `download` | 是 | **https** 直链，指向一个 zip |

---

## 许可

MIT
