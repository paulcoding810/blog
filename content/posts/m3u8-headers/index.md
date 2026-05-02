---
date: "2026-05-02T21:43:48+07:00"
title: "M3U8 Headers"
summary: "Playing M3U8 streams with custom HTTP headers"
categories:
  - Code
tags:
  - streaming
  - vlc
  - mpv
  - m3u8
---

## VLC

```bash
/Applications/VLC.app/Contents/MacOS/VLC \
  --http-referrer "https://example.com/somepage" \
  --http-user-agent "Mozilla Firefox/150.0" \
  "https://example.com/movie.m3u8"
```

## mpv

```bash
mpv \
  --http-header-fields='User-Agent: Mozilla Firefox/150.0','Referer: https://example.com/somepage' \
  "https://example.com/movie.m3u8"
```
