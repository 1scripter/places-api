# curl

```bash
curl -s "https://places.alikara.com.tr/place.php?health=1" | jq .
curl -s "https://places.alikara.com.tr/place.php?code=ist&lang=tr" | jq .
curl -s "https://places.alikara.com.tr/place.php?from=ist&to=cdg&lang=tr" | jq '.data.flight'
curl -s "https://places.alikara.com.tr/place.php?search=bodrum&lang=tr&limit=5" | jq .
curl -s "https://places.alikara.com.tr/place.php?browse=1&region=TR&limit=10&lang=tr" | jq .
curl -s "https://places.alikara.com.tr/place.php?list=1" | jq '.count'
```
