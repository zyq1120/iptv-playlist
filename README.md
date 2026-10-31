# iptv-playlist

Verified IPTV playlist, built 2026-10-31 from public sources and re-checked
with ffprobe (actually reading the video stream, not just the playlist).

- `iptv.m3u` — full list, 1690 entries, 938 channels. Per channel, lines are
  ordered: ffprobe-verified streams first (highest resolution first), then
  playlist-only streams, then IPv6. Same channel name is used for all its
  lines, so a player that groups by name shows them as one channel.
- `iptv-ffprobe-only.m3u` — 977 entries: only ffprobe-verified IPv4 + IPv6.
  Use this if you only want lines that were proven playable here.
- `aptv/line1.m3u` … `aptv/line5.m3u` — Aptv aggregated-config splits.
  Aptv does NOT merge duplicate names inside a single config (it shows 1/1),
  so: import each line file as an ordinary config, then create one
  Aggregated config selecting line1→line5 in order. Aptv then merges by
  channel name and shows multiple lines per channel. Put the 4K config
  (see below) first in the aggregated order if you want 4K as default.

## Subscription URLs (always latest)

```
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/iptv.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/iptv-ffprobe-only.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/aptv/line1.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/aptv/line2.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/aptv/line3.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/aptv/line4.m3u
https://raw.githubusercontent.com/zyq1120/iptv-playlist/main/aptv/line5.m3u
```

## 4K

True 4K (3840×2160, ffprobe-verified) lives in a separate repo, because it
needs its own daily signature maintenance:

https://github.com/zyq1120/iptv-4k
```
https://raw.githubusercontent.com/zyq1120/iptv-4k/main/4k.m3u
```

## Groups

`group-title` is `Country-Type`, e.g. 中国-新闻, 中国-少儿, 中国-体育,
香港-综合, 美国-新闻, etc. IPv6 lines were not directly stream-tested here
(no public IPv6 route in the test sandbox); they are kept from two
mutually-confirming sources (fanmingming / BigBigGrandG) and should be
tested on an IPv6 network.

## Maintenance

Daily scheduled job: re-probes the list with ffprobe, re-orders
verified-first, tries to replace dead channels from fresh upstream lists
(CCSH, Guovin, best-fan, iptv-org, etc.), and pushes only if something
changed. No broadcaster signature is regenerated from scratch; dead signed
URLs are re-sourced from upstream lists that refresh them.
