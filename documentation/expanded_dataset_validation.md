# Expanded MusicOSet: file-level validation and decision memo

Date: 2026-11-18. Status update 2026-11-18: **raw-source audit complete; student approved decisions A–F; models and derived figures rebuilt; original source ZIPs unchanged**.

## Inputs and reproducibility

User-provided `additional.zip` has SHA-256 `745dcb53aa2f67f4602446804595838bbc49bede13f522086ad3ef3246d625aa` and contains two **tab-delimited** CSVs (`hits_dataset.csv`, `nonhits_dataset.csv`), not comma-delimited despite extension. The original publisher identifies these two files and a total of 911,027 songs: https://marianaossilva.github.io/DSW2019/updates.html . The publisher describes the nonhits as tracks by artists with Hot 100 hits that themselves never charted. The ZIP is supplied by the student; identity to the publisher's hosted archives has not been independently verified by a published checksum.

## Verified counts and schema

- Hits: **11,959**, all 11,959 song IDs unique.
- Non-hits: **899,068**, all 899,068 song IDs unique.
- Total: **911,027** unique song IDs; **zero overlap** between groups.
- **24 source columns**: `song_id, song_name, id_artists, name_artists, popularity, explicit, song_type, track_number, num_artists, num_available_markets, release_date, duration_ms, key, mode, time_signature, acousticness, danceability, energy, instrumentalness, liveness, loudness, speechiness, valence, tempo`. The eventual hit indicator is derived only from which publisher table contains the row.
- Song releases range **1995-01-01 through 2019-08-26**. This is not a song-popularity timeseries: there is only one recorded popularity value per song in these files.
- Categorical fields: `song_type` (Solo, Collaboration), `explicit` (boolean), `mode` (binary coded with missing-value dashes), music key, and time signature. Many numeric fields support the >=3 numeric/2 categorical requirement.
- Literal `-` placeholders represent missing audio data in **4,679–4,681 non-hit records** across acoustic features, tempo, duration and key. There are also four genuine null cells across `time_signature, danceability, speechiness, valence` in nonhits. No corresponding fields are missing in hits. **Do not treat `-` as zero**. The presence of different missingness by hit label is a potential measurement bias.

## Outcomes

1. `popularity`: Spotify's **0–100 popularity snapshot**, mean **35.56** for Hot 100 hits versus **7.02** for non-hits, a legitimate numeric measure (not simply `hit * 100`). Overall 455,859 songs have popularity zero, and 13,533 **non-hits have popularity >=50**; 781 **hits have popularity zero**. Thus hit status and Spotify popularity capture different constructs. Crucially, Spotify popularity is measured when collected, not demonstrably when a song was released; this is suitable for a **retrospective explanatory association** but cannot validate a release-day forecast.
2. `hit`: publisher-labeled **Billboard Hot 100 appearance**, yes/no. There are 11,959 hits (1.313%) and 899,068 non-hits (98.687%). A trivial all-nonhit prediction would have **98.687% accuracy** on the full set. 'Non-hit' **does not mean commercially unsuccessful or never discovered**; it only means not in this particular chart among sampled tracks.

## Correlations: accurate numeric parsing on FULL data

All Pearson r values below were computed with numeric fields parsed using `pd.to_numeric(errors='coerce')`, not the parser's initial object/string representation. Observation counts vary slightly with missing acoustic fields.

| Predictor (not future popularity) | r with Billboard hit | r with Spotify popularity |
|---|---:|---:|
| Instrumentalness | -0.064 | -0.251 |
| Loudness | +0.054 | +0.254 |
| Energy | +0.041 | +0.184 |
| Valence | +0.030 | -0.036 |
| Danceability | +0.014 | -0.007 |
| Explicit (0/1) | +0.025 | +0.228 |
| Collaboration (0/1) | -0.003 | +0.153 |
| Track number | -0.024 | -0.154 |

**Core viability challenge:** The full-corpus Billboard binary outcome has no observed *song-feature* predictor at |r|>=0.20, so it **fails that rubric requirement** as currently configured. `popularity` correlates with `hit` at r=+0.253, but should NOT be used as a hit-model predictor because it is a post-release popularity snapshot and risks outcome leakage. Conversely, the *numeric popularity outcome* has multiple predictors above the threshold (loudness, instrumentalness, explicitness).

## Sensitivity-only test: controlled class sampling

Sample all 11,959 hits plus a seeded (`random_state=314`) random subset of non-hits. These correlations are *conditional on choosing a sample with a different class prevalence*, not results for the full 911,027-song population.

| Non-hits per hit | Study hit proportion | instrumentalness vs hit | loudness vs hit |
|---:|---:|---:|---:|
| Full population (~75.2) | 1.31% | -0.064 | +0.054 |
| 10:1 | 9.09% | -0.166 | +0.139 |
| **5:1** | **16.67%** | **-0.221** | +0.182 |
| 2:1 | 33.33% | -0.298 | +0.239 |
| 1:1 | 50.00% | -0.343 | +0.270 |

A 5:1 **case-control-style analytical sample** would exceed the rubric's 0.20 threshold for both model outcomes using song-feature predictors and retain all charting songs. But changing prevalence strongly changes probability interpretation, intercepts, the majority-class baseline, and claimed predictive utility. **This is a design choice requiring approval and transparent reporting**; one cannot pretend 16.7% is the real-world hit rate. A sensitivity appendix on the full corpus would be useful.

## Additional evidence and caution

- Source publisher: https://marianaossilva.github.io/DSW2019/ and https://marianaossilva.github.io/DSW2019/updates.html
- Research methodology: https://journals-sol.sbc.org.br/index.php/jis/article/download/3137/2266/14882
- Publisher describes Spotify popularity as a recency-weighted measure based on plays, updated over time, not a pre-release feature. Historical streaming acceleration and early social media momentum **are not present** in these tables.
- No explicit reuse/redistribution license has been verified. For public GitHub reproducibility, provide source download links and a retrieval script; do not republish raw tables without confirming permission.
- The professor calls for ordinary linear regression for numeric and binary (linear probability) outcomes on the **same processed data used for fitting**. No holdout split should be introduced. Any resulting performance is in-sample only.

## Cleaning/modeling plan proposed, NOT implemented

1. Preserve the ZIP and raw files unchanged; parse TSV explicitly.
2. For missing fields, convert literal `-` to genuine missing values; retain a raw-missingness report; consider complete-case exclusion only for the specific model predictor columns (likely 4,681 rows out of 911,027), and report exclusions by class.
3. Add `hit=1` for source hits, `hit=0` for source nonhits; define `collaboration`, `explicit_binary`, release year with documented transformations. Avoid using numerical Spotify popularity to predict Billboard hit status.
4. Use only acoustic/song metadata plausibly determined on or before release (e.g., instrumentalness, loudness, energy, collaboration, explicit flag, duration). Exclude `num_available_markets` unless time availability is demonstrated; exclude any current artist popularity or chart outcomes as predictors.
5. Decide between **(A) full-corpus honesty with a professor correlation failure**, or **(B) transparently designed case-control-style sampling (suggest 5 non-hits per hit, seed 314) with changed class prevalence**. The user must approve before implementing this.
6. Frame research as **retrospective characteristics associated with Spotify popularity and Billboard hits**, not validated foresight into viral songs or future virtual-stock prices. Retain an editorial prescriptive proposal about prioritizing tracks for review under a capacity/diversity constraint, with no deployed buy signals.

## Questions needing student choice

- Authorize documented 5:1 case-control-style sampling to meet the numerical correlation criterion, understanding it is not representative of real-world prevalence? (Or instead use a different dataset/research outcome.)
- Approve the narrowed interpretation: retrospective hit-versus-nonhit association, **not** early-time breakout prediction unsupported by this data?