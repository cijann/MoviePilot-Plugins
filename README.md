# MoviePilot-Plugins

> [!NOTE]
> 个人自用的 MoviePilot 第三方插件仓库，fork 自 [DDSRem-Dev/MoviePilot-Plugins](https://github.com/DDSRem-Dev/MoviePilot-Plugins)。
> 同时提供 V2 和 V3 两代插件索引，可按需作为插件市场源添加。

本仓库用于集中维护自己 MoviePilot 实例里用到的插件：一部分直接来自上游 DDSRem，一部分是上游插件的修复版，还有针对新版 MoviePilot 适配的插件。所有插件的原作者见下方列表，版权归原作者所有。

## 插件市场源地址

在 MoviePilot 的插件市场设置里添加以下地址即可使用：

```
https://github.com/cijann/MoviePilot-Plugins/
```

- V2 插件（MoviePilot 2.x）：索引 `package.v2.json`，源码在 `plugins.v2/`
- V3 插件（MoviePilot 3.2.6+）：索引 `package.v3.json`，源码在 `plugins.v3/`

## 插件列表

### V3 插件（MoviePilot 3.2.6+）

| 插件 | 版本 | 来源 | 说明 |
|---|---|---|---|
| [豆瓣榜单Plus](plugins.v3/doubanrankplus/) | 2.0.1 | 原作 [boeto](https://github.com/boeto/MoviePilot-Plugins)，本仓库适配 | 定时读取豆瓣 RSS，把想看清单和热门榜单里的影视添加为订阅；已适配 3.2.6 新版识别接口 |

### V2 插件（源自 DDSRem，原样保留）

以下插件均来自上游 [DDSRem-Dev/MoviePilot-Plugins](https://github.com/DDSRem-Dev/MoviePilot-Plugins)，作者 DDSRem，本仓库随 fork 保留。

#### 探索类

- **CCTV探索** `CCTVDiscover` — 让探索支持 CCTV 的数据浏览
- **咪咕视频探索** `MiGuDiscover` — 让探索支持咪咕视频的数据浏览
- **哔哩哔哩探索** `BilibiliDiscover` — 让探索支持哔哩哔哩的数据浏览
- **Bangumi每日放送探索** `BangumiDailyDiscover` — 让探索支持 Bangumi 每日放送的数据浏览
- **芒果TV探索** `MangGuoDiscover` — 让探索支持芒果TV的数据浏览
- **腾讯视频探索** `TencentVideoDiscover` — 让探索支持腾讯视频的数据浏览

#### 网盘与存储类

- **115网盘储存** `P115Disk` — 更快更强的 115 网盘存储模块
- **115网盘STRM助手** `P115StrmHelper` — 115 网盘 STRM 生成一条龙服务
- **123云盘储存** `P123Disk` — 使存储支持 123 云盘
- **123云盘STRM助手** `P123StrmHelper` — 123 云盘 STRM 生成一条龙服务
- **CloudDrive2储存** `CloudDriveDisk` — 使存储支持 CloudDrive2，grpc 原生 API 操作
- **Emby 302 反向代理** `EmbyReverseProxy` — 自动代理 HTTP 链接、跳转最终地址，支持外部播放器调用（本仓库有本地修复，见下）
- **MediaWarp** `MediaWarp` — EmbyServer/Jellyfin 中间件：优化 STRM 播放、自定义前端样式与访问客户端、嵌入脚本

#### 媒体管理类

- **神医媒体文件同步删除** `SaMediaSyncDel` — 通过神医插件通知同步删除历史记录、源文件和下载任务
- **ffprobe命名补充** `FFprobeNamingSupplement` — 整理重命名时调用 `ffprobe`，补全 `videoFormat`、`videoCodec`、`videoBit`、`audioCodec`、`fps`、`effect`，支持 STRM

#### 工具类

- **115订阅站点修复** `P115SubFixer` — 修复 115 网盘订阅追更插件导致的订阅站点被篡改问题，并自动卸载该插件

## 本仓库所做的更改

相对上游 fork 点，本仓库的本地改动：

1. **豆瓣榜单Plus 适配 MoviePilot 3.2.6**（2026-11-17，v2.0.1）
   - 原版 `2.0.0` 调用旧接口 `recognize_media(tmdbid=/doubanid=)` 和 `subscribechain.add(tmdbid=...)`，在 3.2.6 上会报 `unexpected keyword argument 'tmdbid'`，导致条目无法识别、订阅加不上。
   - 改为新版 `media_source` + `media_id` 成对传参；版本号升为 `2.0.1`。
   - 同时新增 V3 索引 `package.v3.json`，使本仓库可作为 V3 插件市场源使用（原仓库只有 V2 索引）。
   - 详情见 [插件 README](plugins.v3/doubanrankplus/README.md)。

2. **Emby 302 反向代理修复 ConnectTimeout**（2026-07-11）
   - 给 `http_client_follow` 和 `http_client_no_follow` 两个 `httpx.AsyncClient` 加上 `PROXY = http://192.168.1.95:7890`（mihomo 代理）。
   - 将 `Limits(max_keepalive_connections=20)` 改为 `Limits(max_connections=20)`。
   - 解决解析 alist 夸克直链时的 `ConnectTimeout`。

> [!IMPORTANT]
> 第 2 项里的代理地址是本机环境的私有地址（192.168.1.95），如在其他环境使用该插件，请按实际情况修改或移除。

提交记录遵循 [Conventional Commits](https://www.conventionalcommits.org/)，规范见 [AGENTS.md](AGENTS.md)。

## 使用说明

- 插件源码目录命名规则：`plugins.v2/<插件ID小写>/`、`plugins.v3/<插件ID小写>/`，入口文件为 `__init__.py`。
- 新增或修改插件后，需同步更新对应索引 `package.v2.json` / `package.v3.json` 的版本号。
- 插件来源以仓库绑定为准：从本仓库安装的插件会绑定 `github:cijann/moviepilot-plugins`，后续自动更新只从本仓库拉取，不会被上游覆盖。

## 感谢

感谢上游插件作者们的付出：

- [DDSRem-Dev/MoviePilot-Plugins](https://github.com/DDSRem-Dev/MoviePilot-Plugins)
- [boeto/MoviePilot-Plugins](https://github.com/boeto/MoviePilot-Plugins)

以及插件依赖的开源项目：

- [p115client](https://github.com/ChenyangGao/p115client)
- [p123client](https://github.com/ChenyangGao/p123client)
- [MediaWarp](https://github.com/Akimio521/MediaWarp)

## 许可证

此仓库内所有项目根据 GNU General Public License v3.0 许可证进行许可，详见 [`LICENSE`](LICENSE) 文件。插件版权归各自原作者所有。
