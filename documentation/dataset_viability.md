# Dataset viability scorecard — 2026-11-18 (approved analytical design)

| Course criterion | Status | Evidence / caveat |
|---|---|---|
| Free public download | PASS | Exact uploaded `additional.zip` MD5 matches Zenodo v2 4904639. |
| Terms permit academic work | CONCERN | Original publisher calls the data open/unrestricted for download and research, but the specific rights grant was not readable in retrieved metadata; avoid raw redistribution until confirmed. |
| Research relevance | PASS / NARROWED | Historical Billboard entry and Spotify popularity snapshot; not real-time virality. |
| Original publisher and codebook | PASS | Silva, Rocha and Moro 2019; MusicOSet original publisher website and Zenodo DOI 10.5281/zenodo.4904639. |
| Sufficient sample size | PASS | 911,027 source tracks; 71,411 modeled observations in student-approved 5:1 sample. |
| >= 3 numeric variables | PASS | Numerous acoustic variables, popularity, duration, etc. |
| >= 2 categorical variables | PASS | Song type, explicitness, musical mode/key, etc. |
| One binary categorical | PASS | Label `hit` and `explicit` / collaboration. |
| Numeric target correlation >= 0.20 | PASS | Popularity vs loudness +0.254; instrumentalness -0.251 (full source). |
| Binary target correlation >= 0.20 | CONDITIONAL PASS | Full dataset fails for pre-outcome features; student-approved 5:1 sampled dataset produces instrumentalness r~-0.221. Sampling affects interpretation. |
| Model predictors available at conceptual decision moment | CONCERN | Audio/track metadata plausible by release; timestamps not independently verified; current Spotify popularity not used as hit predictor. |

**Overall:** Conditional. Strong real historical research source for *retrospective* association. The student approved class sampling, missing-value handling, and chart duplicate/timing conventions on November 18, 2026. The **instructor has not yet confirmed** that altered class prevalence is acceptable for the viability correlation test. The proposed binary model has no lift over the baseline at 0.5, which must be reported as a result, not disguised. Do not use findings as credible release-day hit forecasts.