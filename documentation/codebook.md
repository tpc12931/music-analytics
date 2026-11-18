# Data dictionary and feature lineage

**Unit of observation (primary):** one unique `song_id`, combining the published hits and nonhits tracks. **Secondary unit:** one song–week record in the original Hot 100 history; song-level eight-week summaries are formed after grouping.

| Field | Source | Type | Definition / transformation | Timing caveat |
|---|---|---|---|---|
| `song_id` | Both expanded files | Categorical ID | Unique Spotify track/song ID; no label leaked from ID | Identifier only |
| `hit` | Expanded input filename | Binary outcome | 1 if from `hits_dataset.csv`, 0 from `nonhits_dataset.csv` | Historic chart inclusion, not day-one forecast |
| `popularity` | Expanded field | Numeric outcome, 0–100 | Spotify collection-time popularity snapshot | NOT a pre-release predictor |
| `instrumentalness` | Expanded | Numeric, 0–1 | Acoustic feature; dash `-` parsed as missing | Plausibly song-intrinsic |
| `loudness` | Expanded | Numeric, dB | Track's estimated loudness | Plausibly song-intrinsic |
| `energy` | Expanded | Numeric, 0–1 | Acoustic energy feature | Plausibly song-intrinsic |
| `explicit_01` | `explicit` | Binary derived predictor | 1 explicit, 0 not explicit | Metadata available at release |
| `collab_01` | `song_type` | Binary derived predictor | 1 if Collaboration, 0 if Solo | Metadata available at release |
| `danceability` | Expanded | Numeric, 0–1 | Audio feature for EDA | Not in fitted regression |
| `tempo` | Expanded | Numeric, beats/minute | Audio tempo | Not in fitted regression |
| `release_date` | Expanded | Date | Published release date | Dataset spans 1995–2019; not verified model-time snapshot |
| `num_available_markets` | Expanded | Numeric | Availability snapshot | Excluded: uncertain timing and possible leakage |
| `week` | Original song_chart.csv | Date | Weekly Billboard chart date | Historical chart records |
| `rank_score` | Original song_chart.csv | Numeric | Publisher's inverted chart-rank score (higher is better) | Only defined for charting entries |
| `week_offset` | Derived weekly | Numeric count | Whole weeks elapsed since first observed weekly record | Debut means first observed chart record |
| `weeks_charted_next8` | Derived | Count 0–8 | Distinct chart weeks in offsets 1–8 | Absence means off Hot 100; not zero streams |
| `future_best` | Derived | Numeric score | Highest chart-rank score observed in eight weeks after debut; assigned zero if no future chart appearance (not a stream-count zero) | Used only in secondary historical trajectory analysis |

Unmodeled additional numeric features: duration_ms, acousticness, speechiness, valence, liveness, key, mode, time signature, track_number, num_artists. All input cells originally holding literal dashes are preserved in source ZIP and only coerced to missing during analysis. Column-specific null counts and group imbalance are in `expanded_dataset_validation.md`.