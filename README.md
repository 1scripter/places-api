# Places API

**Places API**, havalimanı / yer verisini yerel bir veri setinde tutarak Open-Meteo (hava) ve TimeAPI.io (yedek yerel saat) gibi web kaynaklarından aldığı bilgileri tek noktada birleştiren; IATA kodu, arama, rota mesafesi / tahmini uçuş süresi sunan bir JSON API projesidir.

| | |
|---|---|
| **Canlı API** | [places.alikara.com.tr](https://places.alikara.com.tr) |
| **Endpoint** | `https://places.alikara.com.tr/place.php` |
| **Canlı örnek** | [places.alikara.com.tr/ornek.html](https://places.alikara.com.tr/ornek.html) |
| **Geliştirici** | [Ali Kara](https://alikara.com.tr) · [GitHub](https://github.com/1scripter) · [bt.alikara@gmail.com](mailto:bt.alikara@gmail.com) |

> Bu repository **public API dokümantasyonudur** (sözleşme + örnekler). Kaynak kod burada yoktur.  
> Ticari kullanım, özel limit veya entegrasyon desteği için iletişime geçin.

---

## Ne sunuyor?

- **Tek destinasyon:** yerel saat + hava (sıcaklık, rüzgâr, nem, WMO kodu)  
- **Rota:** iki IATA arasında mesafe ve tahmini süre  
- **Arama / browse:** havalimanı autocomplete ve bölge listesi  

Örnek kodlar: `ist`, `cdg`, `jfk`, `bjv` …

### Veri kaynakları

| Veri | Kaynak |
|------|--------|
| Havalimanı / koordinat / saat dilimi | Yerel IATA veri seti |
| Anlık hava | Open-Meteo |
| Yedek yerel saat | TimeAPI.io |
| Bayrak | flagcdn.com |

---

## Hızlı deneme

```bash
# Sağlık
curl -s "https://places.alikara.com.tr/place.php?health=1" | jq .

# İstanbul
curl -s "https://places.alikara.com.tr/place.php?code=ist&lang=tr" | jq .

# İstanbul → Paris (rota)
curl -s "https://places.alikara.com.tr/place.php?from=ist&to=cdg&lang=tr" | jq '.data.flight'

# Arama
curl -s "https://places.alikara.com.tr/place.php?search=istanbul&lang=tr&limit=5" | jq .
```

```js
const res = await fetch(
  'https://places.alikara.com.tr/place.php?code=ist&lang=tr'
);
const { data } = await res.json();
console.log(data.place.name, data.place.localTime, data.place.weather.tempC);
```

CORS açıktır (`GET` / `OPTIONS`).

---

## Parametreler

| Parametre | Açıklama |
|-----------|----------|
| `code=` | Tek IATA (ör. `ist`) |
| `from=` + `to=` | Rota (2 destinasyon + `flight`) |
| `places=` | Virgülle kod listesi (`ist,cdg`) |
| `search=` | Autocomplete arama |
| `browse=1` | Liste / filtre (`q=`, `region=`, `limit=`) |
| `browse=1&stats=1` | Bölge sayıları |
| `list=1` | Tüm IATA kodları |
| `health=1` | Kaynak sağlık kontrolü |
| `lat=` + `lon=` + `tz=` | Özel koordinat |
| `lang=` | `tr` (varsayılan) veya `en` |

Şema: [`openapi.yaml`](openapi.yaml) · Örnek JSON: [`examples/`](examples/)

### İstek sınırı

IP başına **60 istek / 60 saniye**.  
`list=1`, `browse=1` ve `health=1` daha yüksek maliyetle sayılır. Limit aşılırsa `429` + `Retry-After` döner.

---

## Ticari kullanım / iletişim

Kaynak kod açık değildir. Demo ve deneme ücretsizdir; ürününüzde kalıcı / ticari kullanım, özel kota veya özel geliştirme için yazın.

**İletişim:** [bt.alikara@gmail.com](mailto:bt.alikara@gmail.com) · [alikara.com.tr](https://alikara.com.tr)

---

## English (short)

**Places API** aggregates airport/place data with live weather (Open-Meteo) and backup local time into a public JSON API (destination cards, route estimates, search).

This repository is **documentation only** (OpenAPI + examples). Source code is not published — contact for commercial use.

- **Live:** [places.alikara.com.tr](https://places.alikara.com.tr)  
- **Endpoint:** `https://places.alikara.com.tr/place.php`  
- **OpenAPI:** [`openapi.yaml`](openapi.yaml)

[Ali Kara](https://alikara.com.tr) — [bt.alikara@gmail.com](mailto:bt.alikara@gmail.com)

---

## Kullanım

- Deneme amaçlı kullanım ücretsizdir  
- Yanıtları cache’lemen önerilir  
- Limit aşımında `429 RATE_LIMITED` alırsın; `Retry-After` kadar bekle  
- Ticari kullanım için önce iletişime geç  
- Atıf: [places.alikara.com.tr](https://places.alikara.com.tr)

## Lisans

Dokümantasyon: MIT. Ürün, kaynak kod ve üçüncü taraf servisler kendi koşullarına tabidir.
