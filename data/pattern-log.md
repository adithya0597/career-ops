# Pattern Log — career-ops

Append-only. Each entry is a dated weekly diff summary.

---

## 2026-04-19 — Week 1 (bootstrap → first weekly review)

**Window:** 2026-04-10 to 2026-04-16 (13 new entries in last 7d; 40 total in tracker)
**New entries last 7d:** 028–040 (April 16 daily batch)
**Applications submitted this week:** 0

### Findings

#### Cluster 1 — H1B unconfirmed at small/international startups (7 SKIP entries)
Roles at seed/early-stage or non-US companies with no LCA history consistently score 3.2–3.5 and are auto-SKIPped. Companies affected: fastino.ai (#006), SuperPlane (#007), Alterion (#008), Human Agency (#023), Synthflow AI (#037), Lorikeet (#038), Ascertain (#040).

**Pattern:** These companies appear in FDE/Applied AI searches because they have correct titles, but visa risk is prohibitive. Title filter cannot distinguish them.

**Action taken:** No title filter change possible for this cluster. Added `"Founding Engineer"` to portals.yml negative (Founding Engineer roles at seed stage = H1B near-certain blocker). Scoring modifier (-0.3 H1B unconfirmed for small companies) is correctly calibrated — no change.

#### Cluster 2 — Domain mismatch: Computer Vision / security ML / non-LLM (7 SKIP entries)
Roles at GameChanger (#012), CompScience (#010), Axon (#027), Gusto (#021), Censys (#026), IMO Health (#016), Formation Bio (#039) all scored 3.5–3.8 and were SKIPped. These have AI/ML titles but the underlying domain is CV, security, clinical NLP, or biotech — not Applied AI / LLM / agents.

**Pattern:** Title filter passes them (title has "ML", "AI", "NLP"), but domain mismatch caps score below 4.0.

**Action taken:** Added `"Computer Vision"` and `"Research Scientist"` to `portals.yml title_filter.negative`. Computer Vision roles are consistently mismatches; Research Scientist roles always hit -0.5 modifier and SKIP.

#### Cluster 3 — Comp hard blockers (4 SKIP entries)
Sezzle (#025, $40–50K India/LATAM), Firecrawl (#009, $5K/mo retainer AI agent role), Close (#011, USA only H1B hard blocker), Adaptive ML (#036, reported $60–71K).

**Pattern:** Comp/visa hard blockers cannot be detected via title filter. Score capped at 1.5 correctly.

**Action taken:** None needed — system correctly SKIPs these via hard blocker rules. No title changes.

### Config Changes Made

| File | Change |
|------|--------|
| `portals.yml` | Created from `templates/portals.example.yml` (did not exist); added 3 negative title patterns: `Computer Vision`, `Research Scientist`, `Founding Engineer` |
| `modes/_profile.md` | Added 2026-04-19 weekly review addendum (no weight changes; false positive rate = 0%) |
| `data/metrics.tsv` | Created with header; first row appended (Week 1 baseline) |

### North Star

- **response_rate:** 0% — but 0 applications submitted; not a scoring problem
- **Status:** FLAT (baseline week; insufficient data for trend direction)
- **⚠️ Primary action needed:** User has 19 Evaluated offers and has not applied to any. Break-glass: Perplexity #004 and TRM Labs #017 (both 4.8/5, H1B confirmed, 9+ days old).
