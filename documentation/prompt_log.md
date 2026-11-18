# AI assistant use log (research and approved processing)

This log reflects major prompts and actions, not invented verbatim complete transcript exports. Add exact prompt excerpts and timestamps from the conversation when finalizing.

1. **Problem selection:** Student explored song virality, first-day success, and hit longevity, asking to prioritize course requirements over Sonora.
2. **Source search:** Assistant found original and expanded MusicOSet, reviewed publishers' definitions, and documented data limitations and licensing questions.
3. **Source files:** Student supplied MusicOSet original metadata / popularity ZIPs and expanded `additional.zip` when direct retrieval failed.
4. **Expanded audit:** Assistant inspected 911,027 rows, discovered severely skewed binary labels, evaluated correlation viability and risks of using the later popularity snapshot to predict the hit label.
5. **Exploratory project build:** Student authorized continued independent progress within available permissions. Assistant wrote reproducible scripts, explored fixed-seed 5:1 sampling, tested OLS and linear probability models, generated charts, and built HTML/PDF/slides.
6. **Quality review:** Assistant found hit model offers no benefit over majority baseline at threshold 0.5, reported invalid fitted values and in-sample evaluation limitations, and avoided representing results as release-day forecasting.

**AI critique:** Sampling to satisfy a correlation rubric may inflate association estimates, while missingness is differential across the outcome classes. Student approved the 5:1 design with the documented limitations on November 18, 2026. Instructor acceptance is not yet verified. Programmatically generated interpretations require independent student review.

7. **Student method approval (November 18, 2026):** Student explicitly approved decisions A–F including the documented 5:1 non-hit:hit sample, after reading the model baseline limitation. Assistant was instructed to continue to finalization without repeated check-ins.