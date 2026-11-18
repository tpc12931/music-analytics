# BUAN 314 research proposal — revised after instructor feedback

**Working title:** Can Musical Characteristics Explain Billboard Hits and Spotify Popularity?

**Research population:** The publisher's MusicOSet expanded catalog of 911,027 tracks (11,959 Billboard Hot 100 hits; 899,068 tracks classified by the publisher as non-hits), with an optional and separately analyzed original weekly Billboard chart archive. The non-hit population was sampled by the publisher from artists who have at least one Billboard hit, so it does **not** represent every released song.

**Decision-maker:** Music discovery editorial lead for a hypothetical streaming discovery platform. The aim is to understand associations useful for reviewing candidate songs; the results must not be used as automatic recommendations to buy virtual shares or as release-day forecasts.

## Descriptive business problem statement

The music discovery lead needs to know which measurable song characteristics are associated with historical Billboard Hot 100 appearance and a higher Spotify popularity score, and whether these associations vary across categorical attributes such as solo versus collaboration and explicit versus non-explicit recordings. Unit: one track. The purpose is to identify meaningful characteristics worth considering in an editorial discovery process, while describing selection bias, missingness, and class imbalance.

**Optional separate descriptive extension:** For songs **already present on the Hot 100** in the 2010–2018 weekly chart dataset, describe how many remain charting for 0–8 weeks after first entry and how their chart trajectories vary. Weekly rankings are not daily streams; chart longevity is not directly comparable with the expanded catalog's non-hit label.

## Predictive business problem statement — two required outcomes

**Numeric regression:** Estimate the Spotify popularity snapshot (0–100) of each cataloged song using song-level audio and metadata such as instrumentalness, loudness, energy, collaboration status, and explicit flag. One unit is a song; model is ordinary least squares fit and evaluated **in sample** on all selected processed rows. This is retrospective explanation, not a demonstrated forward-in-time prediction: Spotify popularity was captured after release.

**Binary classification:** Estimate whether a song is labeled as a Billboard Hot 100 hit (1) or non-hit (0) using the same pre-outcome-plausible song characteristics, with ordinary least squares as the **linear probability model**. Evaluate in sample at the mandated 0.5 cutoff, disclose out-of-bounds predictions, and compare accuracy with the majority-class baseline. Do not use the later Spotify popularity snapshot, current number of available markets, chart ranks, or any future popularity indicator as a predictor of the historical hit label. This does not measure the actual release-day probability of a hit.

**Correlation viability caveat:** The full expanded corpus does not show any known nonleaking audio feature with approximately |r|>=0.2 to hit status. An **approved** 5:1 non-hit:hit sampling design crosses that threshold but changes the class prevalence and does **not** yield hit-classification improvement over its majority baseline. The instructor should confirm whether the 5:1 case-control approach is an acceptable way of demonstrating the correlation criterion. Do not hide the full-corpus correlation failure.

## Prescriptive business problem statement — proposal only

How could a music-discovery editorial lead allocate a fixed weekly number of manual review/spotlight slots to songs using available song characteristics and contextual evidence, seeking diversity and discovery usefulness while respecting editorial staffing, genre diversity, and fairness constraints? Any future allocation would require stronger prospective validation. The class project will **not** implement optimization or Sonora trading advice.

## Why this topic fits the instructor's feedback

The central analysis is **cross-sectional** song attributes, not time-series modeling. Qualitative variables are treated as categorical: the explicit-content flag and solo/collaboration type. Numeric acoustic features are quantitative; they are not mislabeled qualitative. The historical weekly chart analysis remains a descriptive appendix, not a third forecasting model.

## Status and student decisions

The student approved decisions A–F on November 18, 2026. See `documentation/decision_sheet.md`. This approval does **not** establish that the data represent real-world hit probabilities or that the instructor accepted the case-control correlation screen.