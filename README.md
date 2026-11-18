# BUAN 314–370 | MusicOSet: Hits, Non-hits, and Chart Longevity

**Status: Student-approved analysis, November 18, 2026; deliverables pending student interpretation, recording, and publication.** This is a draft that the student must verify and interpret, not a published or approved final submission. Source data are not redistributable in this package without checking reuse permissions. No Sonora production code was modified.

See `documentation/proposal.md` for the current cross-sectional research proposal and `documentation/decision_sheet.md` for precise student approvals. The earlier chart-momentum-only topic is **not** the main question.

## Research framing

1. **Descriptive:** How do musical attributes and release metadata differ among songs labeled Billboard hits and non-hits? Among already-charting songs, what distinguishes rapid disappearance from sustained chart presence?
2. **Numeric predictive:** How well can selected song features explain a retrospective Spotify popularity score (0–100)? Fit OLS; report $R^2$, RMSE, residual diagnostics, and interpretations.
3. **Binary predictive:** How well can the same features explain a historical Billboard Hot 100 appearance? Fit an ordinary least-squares linear probability model; report $R^2$, confusion matrix at 0.5, majority baseline, out-of-bounds predictions.
4. **Prescriptive:** Proposal only: an editorial manager allocates scarce discovery spotlight capacity among songs, subject to diversity and staffing constraints. **No optimization implemented.**

**Important:** The expanded dataset supplies a later popularity snapshot, not pre-release or early-day streaming measurements. The project does **not** validate prediction of first-day virality, sustained streaming, actual market hit probability, or future prices.

## Original publisher and input files

- MusicOSet research landing page: https://marianaossilva.github.io/DSW2019/
- Expanded dataset and methodology: https://marianaossilva.github.io/DSW2019/updates.html
- `additional.zip` — from publisher files supplied by student; contains tab-separated `hits_dataset.csv` and `nonhits_dataset.csv`. SHA256 `745dcb53aa2f67f4602446804595838bbc49bede13f522086ad3ef3246d625aa`; MD5 `88626a90c7f4bdf0cfdb6c39a2c2fd31` **matches the publisher’s Zenodo checksum**.
- `musicoset_popularity.zip` — original publisher ZIP supplied by student; includes `song_chart.csv` and was previously matched to publisher MD5 `f14c16252f1cb3af49f5f8fad56deaaa`.
- `musicoset_metadata.zip` — optional original publisher ZIP for additional song-level metadata; publisher MD5 `dbf4de4942ba54e3892a213bf4856675`.

**License caution:** Do not upload raw datasets to GitHub until reuse/redistribution rights are confirmed. A public repo should point to the publisher and explain how users can obtain inputs. The analysis scripts never overwrite uploaded ZIP archives.

## Reproducing the approved exploratory analysis

Requires Python 3.10+, NumPy, pandas, matplotlib, statsmodels. Install: `python -m pip install -r requirements.txt`.

Place the two relevant files in a folder, then set `BUAN_DATA_DIR` to that folder. Alternatively use `BUAN_EXPANDED_ZIP` and `BUAN_WEEKLY_ZIP` to point to exact files.

```bash
export BUAN_DATA_DIR=/path/to/downloads
python scripts/05_provisional_analysis.py
python scripts/07_quality_and_scenarios.py
python scripts/06_build_interactive.py
python scripts/08_sampling_sensitivity.py
```

These regenerate the numeric summaries in `outputs/tables`, eight figures in `outputs/figures`, and `docs/index.html`. Figure data embedded in HTML are an **anonymized deterministic 1,500-row feature sample** without song IDs, titles, or artist names. The HTML runs offline in a browser with zero external libraries. Use `node --check docs/interactive.js` if checking the separately extracted script; the HTML itself is the self-contained deliverable.

Compile `report/report.tex` in Overleaf (upload the PNG figures, with adjusted relative paths) or locally with `pdflatex`; the provided PDF is a local verification artifact, not proof of Overleaf compilation. Rebuild slides with `node slides/build_slides.js` (requires PptxGenJS and the slide helper module used in this environment) or edit the included PPTX manually. Slides have been visually rendered and passed overflow checking.

## Approved exploratory analytical design

- Use all 11,959 publisher hits and a seeded random sample (314) of five non-hits per hit; this is **case-control-style sampling for rubric viability**, not a representative music market distribution.
- `hit=1` for the publisher's `hits_dataset.csv`, `hit=0` for `nonhits_dataset.csv`.
- Parse `-` placeholders for acoustic features as missing. Drop records with missing values in model predictors only (343 sampled records, all in non-hit group). Model sample `n=71,411`.
- Regress two outcomes on the same five features: `instrumentalness`, `loudness`, `energy`, an explicit-content flag, and a collaboration flag. **Never use Spotify popularity itself to predict the hit label** due to post-outcome leakage risk.
- Both models are estimated and assessed on the exact same processed rows. **No train/test split** (course-specific instruction). Model performance is in-sample only.
- Secondary chart history: determine first observed Hot 100 week in weekly records; count separate weekly chart appearances in the following eight weeks; no observed chart week means 'no chart appearance' (not zero streams); source duplicate song-weeks resolved as approved by keeping the highest reported score. The 2010–2018 sample excludes chart entries after October 2018 to support a full subsequent observation window.
- Model and chart choices were approved by the student on November 18, 2026. Instructor acceptance of the 5:1 sampling workaround has not been verified.

## First-pass results (NOT predictive validation)

| Model | In-sample R² | Other required metric |
|---|---:|---|
| Numeric popularity OLS | 0.1684 | RMSE 16.50 popularity points |
| Hit label linear probability model | 0.0623 | Accuracy 83.25%; 83.25% majority baseline; TP 0; FN 11,959; 6.62% fitted values outside [0,1] |

Sensitivity: the *different* 1:1 sample has 65.4% in-sample accuracy against a 50.1% baseline; the 2:1 sample has 67.0% against 66.5%, and 5:1 fails to improve on baseline. All such results depend strongly on altered class prevalence; see `outputs/tables/sampling_sensitivity.json`.

The binary regression contains statistically detectable associations, but at cutoff 0.5 **fails completely to find hits**; this must be a leading finding and not hidden. Any use as a launch or investment signal is unsupported.

## Deliverables status

- `docs/index.html`: interactive, self-contained HTML report with **eight** SVG charts, including a two-control explorer and a chart-week slider.
- `report/report.tex` and `report/report.pdf`: LaTeX-based report, with eight static figures and interpretation.
- `slides/buan314_presentation_draft.pptx` and `.pdf`: 18-slide presentation, pending student narration and pacing to 18–22 minutes.
- `scripts/`: source audits and analysis/build scripts; outputs generated reproducibly from local archive inputs.
- `documentation/`: data provenance and methodological cautions, decision log, codebook, and postmortem placeholder.

**Student-only or separately authorized final steps:** personal explanation of results, completion of AI-partner postmortem with the student's real experience, compiling final report via Overleaf, recording/hosting narrated presentation with camera, and verifying the public GitHub repository and live Pages site.

## Submission and external-access checklist

The local reproducible report, figures, HTML, and slide files are prepared. To satisfy the actual course submission requirements, the student still must: (1) review the academic interpretation and complete the authentic AI-assistant postmortem; (2) compile the supplied `BUAN314_Overleaf_Import.zip` in Overleaf; (3) narrate the slide deck with camera on for 18–22 minutes and publish a video link; (4) verify the created **public** GitHub repository and GitHub Pages site (do not publish raw publisher archives without a confirmed license); and (5) check the instructor's acceptance of a 5:1 sampling strategy used to meet the correlation screening criterion.

See `documentation/instructor_review_note.md`, `documentation/approval_record.md`, and `slides/presentation_timing.md`.