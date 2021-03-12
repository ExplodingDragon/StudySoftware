---
typora-root-url: images
---

# Nginx 配置

> Windows & MacOS & Linux 通用

## 配置反向代理

例如下面是 Nginx 作为反向代理缓存的配置片段，可以将 /data/cdn_cache 目录挂载为 tmpfs 。 

```nginx
proxy_temp_path  /data/cdn_cache/proxy_temp_dir;
proxy_cache_path /data/cdn_cache/proxy_cache_dirlevels=1:2 keys_zone=cache_one:50m inactive=1d;
```