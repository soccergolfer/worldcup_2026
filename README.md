# WC 2026 Performance Tracker

Interactive team performance ranking for the 2026 FIFA World Cup, using a customisable weighted formula across goal margin, possession, shots, opponent difficulty, xGF, and xGA.

## 🚀 Deploy to GitHub Pages (5 minutes)

1. **Create a new GitHub repo** — e.g. `wc2026-tracker` (can be private or public)
2. **Upload both files** — `index.html` and `data.json` to the root of the repo
3. **Enable GitHub Pages**:
   - Go to repo → Settings → Pages
   - Source: `Deploy from a branch`
   - Branch: `main` / `(root)`
   - Click Save
4. Your site will be live at `https://yourusername.github.io/wc2026-tracker/` within ~60 seconds

---

## 📋 Updating data after each matchday

Open `data.json` and:

1. Update `"last_updated"` (format: `YYYY-MM-DD`)
2. Update `"matchday"` number
3. For each new match, **add two entries** (one per team) — copy an existing entry as a template:

```json
{
  "name": "England",
  "flag": "🏴󠁧󠁢󠥿",
  "group": "L",
  "opp": "Croatia",
  "oppRank": 14,
  "result": "2–0",
  "outcome": "W",
  "margin": 2,
  "poss": 61,
  "shots": 17,
  "xgf": 1.85,
  "xga": 0.42,
  "est": false
}
```

4. Set `"est": true` if any stats are estimated from match reports rather than confirmed Opta data
5. Commit and push — GitHub Pages updates automatically

### Field reference

| Field | Type | Description |
|---|---|---|
| `name` | string | Team name |
| `flag` | string | Flag emoji |
| `group` | string | Group letter (A–L) |
| `opp` | string | Opponent name |
| `oppRank` | number | Opponent's FIFA ranking |
| `result` | string | Score e.g. `"2–0"` |
| `outcome` | string | `"W"`, `"D"`, or `"L"` |
| `margin` | number | Goal difference (negative for losses) |
| `poss` | number | Possession % (e.g. `65`) |
| `shots` | number | Total shots attempted |
| `xgf` | number | Expected goals for |
| `xga` | number | Expected goals against |
| `est` | boolean | `true` if any stat is estimated |

---

## 📊 Data sources

- **xG**: [RealGM xG Tracker](https://soccer.realgm.com/analysis/559) · [xGscore](https://xgscore.io/xg-statistics/world-cup/2026)
- **Possession / Shots**: FIFA match centres / Opta
- **FIFA Rankings**: June 2026 update

---

## Formula

```
Score = (GoalMargin × W₁) + (Possession × W₂) + (Shots × W₃)
      + (OppDifficulty × W₄) + (xGF × W₅) + (xGA_defense × W₆)
```

All metrics are normalised 0–100 before weighting. Opponent difficulty = `100 − FIFA rank`. xGA defense is inverted (lower xGA conceded = higher defensive score). Weights auto-normalise proportionally if they don't sum to 100.
