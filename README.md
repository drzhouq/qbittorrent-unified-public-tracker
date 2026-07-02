# Unified Public Tracker for qBittorrent

Consolidated public tracker search plugin for qBittorrent.

## Implementation Note: Search Query Encoding

A common pitfall in qBittorrent search plugins is **double URL-encoding**. 

qBittorrent's plugin runner passes the search string to the `search(self, what, cat)` method **already URL-encoded** (e.g., spaces are converted to `%20`). If the plugin calls `quote_plus()` on the already-encoded string, it double-encodes the percentage signs (`%20` becomes `%2520`), causing tracker APIs to return zero results for multi-word queries.

This plugin avoids that by decoding the incoming `what` parameter once at the start of the `search` method:

```python
from urllib.parse import unquote_plus

def search(self, what, cat="all"):
    # Decode once to handle qBittorrent's pre-encoding
    what = unquote_plus(what).strip()
    # ...
```

## Supported Trackers
- BitSearch
- ThePirateBay (via apibay)
- Rutor
- Nyaa
- SolidTorrents
- DMHY
- YTS
- EZTV
- AcademicTorrents
