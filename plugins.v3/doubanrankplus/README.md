# 豆瓣榜单 Plus（适配版）

定时读取豆瓣 RSS 榜单，把里面的影视自动添加为 MoviePilot 订阅。

## 来源与修改说明

- 原始插件来自 [boeto/MoviePilot-Plugins](https://github.com/boeto/MoviePilot-Plugins)，原版本 `2.0.0`。
- 本仓库版本 `2.0.1`：适配 MoviePilot **3.2.6+** 新版接口。原版调用 `recognize_media(tmdbid=...)` / `recognize_media(doubanid=...)` 和 `subscribechain.add(tmdbid=...)`，新版主程序改为 `media_source` + `media_id` 成对传入，导致识别环节报 `unexpected keyword argument 'tmdbid'` 而失败。已全部改为新写法。

## 改动点

1. 导入 `MediaSource`。
2. `recognize_media(tmdbid=...)` → `recognize_media(media_source=MediaSource.TMDB, media_id=str(...))`。
3. `recognize_media(doubanid=...)` → `recognize_media(media_source=MediaSource.Douban, media_id=str(...))`。
4. `subscribechain.add(tmdbid=...)` → `subscribechain.add(media_source=MediaSource.TMDB, media_id=str(...))`。

## RSS 地址填写说明

自建 RSSHub 时，豆瓣用户想看清单的正确路由是：

```
https://<你的rsshub域名>/douban/people/<豆瓣用户名>/wish
```

注意必须带 `/douban` 前缀。常见错误是填成 `/people/<用户名>/wish`，会返回 308 跳转且无内容。
