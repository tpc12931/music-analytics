# Provisional data-processing decision log — student sign-off required

| Proposed step | Rows before → after | Implementation | Why and what it changes | Student approval |
|---|---:|---|---|---|
| Preserve user-uploaded source ZIPs, read expanded `.csv` contents with tab separator | 911,027 → 911,027 | `05_provisional_analysis.py` reads from archive | Publisher CSV extension is misleading; parsing preserves all columns | Pending formal sign-off |
| Retain every published hit; sample 5 nonhits per hit with seed 314 | 911,027 → 71,754 | `RandomState(314).choice` | Meets approximate course binary correlation criterion but inflates hit prevalence to 16.7%; results not population predictions | **Pending formal sign-off** |
| Parse literal `-` for numeric model predictors to missing | 71,754 → 71,754 | `pd.to_numeric(errors='coerce')` | Dashes are not true zero audio values | **Pending formal sign-off** |
| Drop only missing predictor rows for both required models | 71,754 → 71,411 | `dropna(subset=model_cols + outcomes)` | 343 sampled rows omitted; all non-hits; report selection bias | **Pending formal sign-off** |
| Encode explicit / solo-versus-collaboration binary flags | 71,411 → 71,411 | Mapping flags | Supplies categorical predictors under OLS | **Pending formal sign-off** |
| Separate chart dataset; resolve duplicate song–week by highest score | 250,392 weekly rows → unique weekly pairs | Sort / drop duplicates | Preliminary tie policy; source has 13 duplicate song–week records | **Pending formal sign-off** |
| Study first observed chart entries 2010–Oct 2018 | 20,402 charted songs → 2,479 songs | Window filter and eight subsequent weeks | Protects eight-week lookahead but may include right-censoring not visible in source | **Pending formal sign-off** |

This log documents exploratory transformations already executed in separate generated artifacts, **not** changes to the original archives. No final academic analysis claim is approved until the student understands and signs off on these choices.