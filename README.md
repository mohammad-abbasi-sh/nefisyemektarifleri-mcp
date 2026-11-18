# nefisyemektarifleri-mcp 🇹🇷

mcp-name: io.github.mohammad-abbasi-sh/nefisyemektarifleri-mcp

Unofficial **read-only** MCP server for **Nefis Yemek Tarifleri** (nefisyemektarifleri.com) — Türkiye's biggest recipe site: search 14,000+ recipes per query, full recipe details (ingredients with amounts, steps, tips, times, servings, ratings, nutrition), the full recipe-category tree, category listings with popular/trending/newest sorts, the most-made recipes and the daily "Bugün Ne Pişirsem?" menu.

> Part of the "Turkey MCP factory" (Sepehr-style pipeline). Not affiliated with Nefis Yemek Tarifleri.

## For AI agents

Add to your MCP client config (stdio):

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

Or run locally: `python -m nefisyemektarifleri_mcp.server` (needs `mcp<2`, `curl-cffi`, `requests`).

## Tools

| Tool | Description |
|------|-------------|
| `nefis_search_recipes` | Search recipes by dish/ingredient — `query`, `limit` (1..30), `page` (1..20) |
| `nefis_get_recipe` | Full recipe — full URL or bare slug (e.g. `mercimek-corbasi-tarifi`) |
| `nefis_list_categories` | Full category tree (24 top-level, 74 subcategories) |
| `nefis_category_recipes` | Recipes in one category — `category` slug/URL, `limit`, `page`, `sort` (popular/trending/newest) |
| `nefis_trending` | Most-made recipes ("En Çok Yapılanlar", 24) |
| `nefis_daily_menu` | Today's "Bugün Ne Pişirsem?" menu (25 recipes) |
| `nefis_health` | Reachability check (ok / blocked / error) |

Example output (`nefis_search_recipes` / "mercimek"):

```json
{"query": "mercimek", "total_results": 14694,
 "recipes": [{"title": "Mercimek Köftesi Nasıl Yapılır? (videolu)",
   "serves": "4-6", "prep_time": "50dk", "cook_time": "30dk",
   "author": "Elif Atalar", "saved_count": 385000, "comment_count": 2985}]}
```

Example output (`nefis_get_recipe` / "mercimek-corbasi-tarifi"):

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

Notes:
- Search is the site's own `/ara/` page — Cloudflare rejects requests without a Referer, so every request sends the site as Referer; transport uses Chrome TLS impersonation with a plain-requests fallback.
- Listings paginate WordPress-style (`/page/2/`); `?sayfa=N` is ignored by the site (it silently returns page 1), so the server never uses it.
- Times are Turkish shorthand (`15dk`); servings are Turkish (`6-8 kişilik`); saved/comment counts parse Turkish abbreviations (`385bin` → 385000).
- Data: nefisyemektarifleri.com public pages — no key, no login, read-only.

## Development

```bash
python3 -m venv .venv && .venv/bin/pip install "mcp<2" curl-cffi requests pytest
.venv/bin/python -m pytest tests/ -q
```

- `tests/test_recipes.py` — offline, parses real captured fixtures
- `tests/test_live.py` — 9 cautious live requests against nefisyemektarifleri.com

## License

[PolyForm Noncommercial 1.0.0](https://polyformproject.org/licenses/noncommercial/1.0.0/) — free for personal and non-commercial use; commercial use needs a separate license (see LICENSE).

---

Built by [Mohammad Abbasi](https://github.com/mohammad-abbasi-sh) — [Almanibama](https://almanibama.ir) · [mabbasi.ir](https://mabbasi.ir)
