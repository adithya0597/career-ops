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

---

## 2026-05-03 — Week 3 (second weekly review)

**Window:** 2026-04-20 to 2026-05-03 (49 new entries: #041–#089)
**New entries last 7d:** #063–#089 (April 26 scan batch, 27 entries)
**Applications submitted this week:** 0 (cumulative: 0 across all 89 evaluations)

### Findings

#### Cluster 1 — Hard blockers slipping through scan (5 entries, all 1.5/5)
Companies: Outreach (#073, comp $70–130K < $100K floor), xAI Voice (#074, on-site London/SF), AHEAD/Thinkahead (#075, Australian legal entity), Variance (#076, SF in-person only), Lyra Tech (#088, on-site at client sites).

**Pattern:** Location and comp hard blockers cannot be detected by title filter. All 5 were correctly caught and scored 1.5/5 — system working as intended. Hard blocker detection working correctly.

**Action taken:** No title filter change possible. Scoring correctly caps at 1.5/5.

#### Cluster 2 — IT services / consulting / BPO companies (7 entries, 3.0–3.3)
Companies: Techtorch (#081), Caylent (#083, AWS cloud consultancy), Percepta (#080, BPO contact center services), Paraform (#082, hiring marketplace startup), BrightAI (#085), CoreStory (#086), Concentrate AI (#079).

**Pattern:** These companies are NOT AI-native product companies. They consistently score 3.0–3.3 due to H1B unconfirmed (-0.3) + non-AI-native environment. The title filter cannot distinguish them — they have valid "Applied AI Engineer" or "FDE" titles.

**Action taken:** Added -0.2 scoring modifier to `modes/_profile.md` for "IT services / consulting / BPO / managed services" company types. This correctly separates them from AI-native companies with similar titles.

#### Cluster 3 — Domain mismatch: cybersecurity, OSINT, supply chain (5 entries, 3.2–3.7)
Companies: Horizon3 AI (#069, cybersecurity/pentesting), GuidePoint Security (#087, GenAI for cybersecurity), Sayari (#070, #071, OSINT analytics), Resilinc (#068, supply chain AI), Mercura (#089, pharma supply chain).

**Pattern:** Security and supply chain domain companies keep appearing. These have valid "Applied AI" / "FDE" / "Sr Applied AI Engineer" titles — title filter cannot distinguish. Domain mismatch detected correctly at evaluation (3.2–3.7 range, all SKIP).

**Action taken:**
- Added `"OSINT"`, `"Pentesting"`, `"Pen Test"` to `portals.yml title_filter.negative` (partial mitigation for most egregious cases)
- Added -0.2 scoring modifier to `modes/_profile.md` for cybersecurity/OSINT/pentesting domain
- Added -0.2 scoring modifier for supply chain/logistics/pharma operations domain

### Config Changes Made

| File | Change |
|------|--------|
| `portals.yml` | Created from `templates/portals.example.yml` (again — gitignored). Added Week 1 negatives: `Computer Vision`, `Research Scientist`, `Founding Engineer`. Added Week 3 negatives: `OSINT`, `Pentesting`, `Pen Test` |
| `modes/_profile.md` | Added 2026-05-03 weekly review addendum with 3 new scoring modifiers (-0.2 for consulting/BPO, -0.2 for cybersecurity/OSINT, -0.2 for supply chain/pharma) |
| `data/applications.md` | Reconstructed from 89 merged TSVs (was missing — gitignored) |
| `data/metrics.tsv` | Appended Week 3 row (still 0 applied; false positive rate 0%) |

### North Star

- **response_rate:** 0.00% — two consecutive weeks below 10% → spec flag triggered
- **Status:** FLAT (no applications submitted; metrics unchanged from baseline)
- **⚠️ Spec flag:** `ACTION: re-tune profile or narrow archetypes` — triggered by 2 consecutive weeks response_rate < 10%. **However:** the real issue is 0 applications submitted, not scoring calibration. False positive rate = 0%; 36 Evaluated entries with valid scores. User must act.
- **Break-glass list:** #004 Perplexity Agents (4.8, 23 days), #017 TRM Labs (4.8, 23 days), #001 Ramp (4.7, 23 days), #002 Cohere (4.7, 23 days), #043 Cresta (4.6, 15 days)
