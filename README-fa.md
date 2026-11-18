# nefisyemektarifleri-mcp 🇹🇷

mcp-name: io.github.mohammad-abbasi-sh/nefisyemektarifleri-mcp

سرور MCP غیررسمی و **فقط‌خواندنی** برای **«نفیس یمک تاریف‌لری»** (nefisyemektarifleri.com) — بزرگ‌ترین سایت دستور غذای ترکیه: جستجو در بیش از ۱۴٬۰۰۰ دستور غذا برای هر پرس‌وجو، جزئیات کامل دستور (مواد با مقدار، مراحل، نکته‌ها، زمان، تعداد نفرات، امتیاز، ارزش غذایی)، درخت کامل دسته‌بندی دستورها، فهرست دسته‌ها با مرتب‌سازی محبوب/پرطرفدار/جدیدترین، پرتکرارترین دستورها و منوی روزانه «امروز چی بپزم؟».

> بخشی از «کارخانه MCP ترکیه» (خط تولید به سبک سپهر). وابسته به Nefis Yemek Tarifleri نیست.

## برای ایجنت‌های هوش مصنوعی

به پیکربندی کلاینت MCP خود اضافه کنید (stdio):

```json
{
  "mcpServers": {
    "nefis": {
      "command": "uvx",
      "args": ["nefisyemektarifleri-mcp==0.1.0"]
    }
  }
}
```

یا به‌صورت محلی اجرا کنید: `python -m nefisyemektarifleri_mcp.server` (نیازمند `mcp<2`، `curl-cffi`، `requests`).

## ابزارها

| ابزار | توضیح |
|------|-------|
| `nefis_search_recipes` | جستجوی دستور غذا با نام غذا یا ماده — `query`، `limit` (۱ تا ۳۰)، `page` (۱ تا ۲۰) |
| `nefis_get_recipe` | دستور کامل — URL کامل یا اسلاگ ساده (مثلاً `mercimek-corbasi-tarifi`) |
| `nefis_list_categories` | درخت کامل دسته‌بندی (۲۴ دسته اصلی، ۷۴ زیردسته) |
| `nefis_category_recipes` | دستورهای یک دسته — اسلاگ/URL دسته، `limit`، `page`، `sort` (popular/trending/newest) |
| `nefis_trending` | پرتکرارترین دستورها («En Çok Yapılanlar»، ۲۴ دستور) |
| `nefis_daily_menu` | منوی روزانه «Bugün Ne Pişirsem؟» (۲۵ دستور) |
| `nefis_health` | بررسی در دسترس بودن سایت (ok / blocked / error) |

نمونه خروجی (`nefis_search_recipes` / «mercimek»):

```json
{"query": "mercimek", "total_results": 14694,
 "recipes": [{"title": "Mercimek Köftesi Nasıl Yapılır? (videolu)",
   "serves": "4-6", "prep_time": "50dk", "cook_time": "30dk",
   "author": "Elif Atalar", "saved_count": 385000, "comment_count": 2985}]}
```

نمونه خروجی (`nefis_get_recipe` / «mercimek-corbasi-tarifi»):

```json
{"title": "Mercimek Çorbası Tarifi", "rating": 4.8, "rating_votes": 388,
 "serves": "6-8", "prep_time": "15dk", "cook_time": "30dk",
 "ingredients": [{"group": "Malzemeler",
   "items": ["2 su bardağı kırmızı mercimek", "1 adet soğan", "..."]},
  {"group": "Sosu için:",
   "items": ["2 yemek kaşığı tereyağı", "..."]}],
 "steps": ["Kırmızı mercimek çorbası için sıvı yağı ...", "..."],
 "nutrition_per_serving": {"calories_kcal": 284, "carbs_g": 25,
   "protein_g": 13, "fat_g": 12.5, "fiber_g": 5.05}}
```

نکته‌ها:
- جستجو صفحه `/ara/` خود سایت است — Cloudflare درخواست بدون Referer را رد می‌کند؛ بنابراین هر درخواست، خود سایت را به‌عنوان Referer می‌فرستد. انتقال با تقلید TLS کروم و پشتیبان requests ساده انجام می‌شود.
- فهرست‌ها به سبک وردپرس صفحه‌بندی می‌شوند (`/page/2/`)؛ سایت پارامتر `?sayfa=N` را نادیده می‌گیرد (بی‌صدا صفحه ۱ را برمی‌گرداند)، پس سرور هرگز از آن استفاده نمی‌کند.
- زمان‌ها به‌صورت مخفف ترکی هستند (`15dk`)؛ تعداد نفرات ترکی است (`6-8 kişilik`)؛ شمار ذخیره/نظر از مخفف‌های ترکی تبدیل می‌شود (`385bin` ← 385000).
- داده: صفحه‌های عمومی nefisyemektarifleri.com — بدون کلید، بدون ورود، فقط‌خواندنی.

## توسعه

```bash
python3 -m venv .venv && .venv/bin/pip install "mcp<2" curl-cffi requests pytest
.venv/bin/python -m pytest tests/ -q
```

- `tests/test_recipes.py` — آفلاین، نمونه‌های واقعی ضبط‌شده را تجزیه می‌کند
- `tests/test_live.py` — ۹ درخواست زنده محتاطانه به nefisyemektarifleri.com

## مجوز

[PolyForm Noncommercial 1.0.0](https://polyformproject.org/licenses/noncommercial/1.0.0/)

---

Built by [Mohammad Abbasi](https://github.com/mohammad-abbasi-sh) — [Almanibama](https://almanibama.ir) · [mabbasi.ir](https://mabbasi.ir)
