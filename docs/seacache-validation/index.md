# Cosmos 3 SeaCache validation report

SeaCache was evaluated on **Cosmos 3 Nano and Cosmos 3 Super** against the no-cache path using matched seeds, cookbook prompts/assets, and regional diffusion compilation. Quality was assessed with the full PaiBench-I2V suite, visual review, and fidelity metrics.

Benchmarked SeaCache parameters: `sea_threshold=0.25`, `sea_residual_order=1`, `sea_max_consecutive_cached=2`, and `sea_power_exp=3.0`.

## PAIBench aggregated I2V quality

Score: **82.6 overall**, **87.1 domain**, and **78.2 quality**.

The domain score is 0.1 points below no-cache leaderboard reference. Manual inspection of the lowest-scoring samples did not reveal a consistent or clearly attributable visual degradation. This small VQA movement is therefore best treated as a weak aggregate signal rather than evidence of a visible regression.

## Result

- Commit: [`640daae0041555ac0cee78840880e1b39399f9f4`](https://github.com/yzhautouskay/vllm-omni/commit/640daae0041555ac0cee78840880e1b39399f9f4)
- Paired visual generations evaluated by task:
  - T2I: **9 pairs** (**18 outputs**: 9 no-cache + 9 SeaCache)
  - I2V: **24 pairs** (**48 outputs**: 24 no-cache + 24 SeaCache)
  - T2V: **36 pairs** (**72 outputs**: 36 no-cache + 36 SeaCache)
  - V2V: **9 pairs** (**18 outputs**: 9 no-cache + 9 SeaCache)
  - TRANSFER: **24 pairs** (**48 outputs**: 24 no-cache + 24 SeaCache)
  - Total: **102 pairs / 204 outputs**
- Visual parallelism coverage:
  - Nano, 1 GPU: **33 pairs**, **1.91×** mean speedup
  - Super, 1 GPU: **33 pairs**, **1.98×** mean speedup
  - Super, 2 GPUs (CFG=2, HSDP=2): **21 pairs**, **1.88×** mean speedup
  - Super, 4 GPUs (CFG=2, Ulysses=2, HSDP=4): **15 pairs**, **1.84×** mean speedup
- Paired action-generation workflows evaluated by task:
  - AV forward dynamics: **2 pairs** (Nano and Super)
  - DROID policy: **1 pair** (Nano)
- Mean visual-pair speedup: **1.92×**
- Visual-pair speedup range: **1.75–2.16×**



## Core matrix

Nano was tested on one GPU. Super was tested on one, two, and four GPUs; the four-GPU topology uses CFG=2, Ulysses=2, and HSDP=4.


| Model | Topology            | Mode | Pairs | Mean speedup | Mean LPIPS | Mean PSNR (dB) | Mean SSIM |
| ----- | ------------------- | ---- | ----- | ------------ | ---------- | -------------- | --------- |
| Nano  | single              | i2v  | 6     | 1.83×        | 0.0937     | 25.68          | 0.8264    |
| Nano  | single              | t2v  | 9     | 1.84×        | 0.2211     | 20.55          | 0.7320    |
| Super | cfg2_hsdp2          | i2v  | 6     | 1.89×        | 0.0977     | 24.55          | 0.8022    |
| Super | cfg2_hsdp2          | t2v  | 9     | 1.89×        | 0.1813     | 21.80          | 0.7505    |
| Super | cfg2_ulysses2_hsdp4 | i2v  | 6     | 1.84×        | 0.0977     | 24.55          | 0.8022    |
| Super | cfg2_ulysses2_hsdp4 | t2v  | 9     | 1.84×        | 0.1813     | 21.80          | 0.7505    |
| Super | single              | i2v  | 6     | 1.93×        | 0.1106     | 23.75          | 0.7745    |
| Super | single              | t2v  | 9     | 1.92×        | 0.1883     | 21.52          | 0.7423    |




## Mode and transfer coverage


| Model | Topology   | Mode     | Pairs | Mean speedup | Mean LPIPS | Mean PSNR (dB) | Mean SSIM |
| ----- | ---------- | -------- | ----- | ------------ | ---------- | -------------- | --------- |
| Nano  | single     | t2i      | 3     | 1.80×        | 0.2663     | 16.46          | 0.7951    |
| Nano  | single     | transfer | 12    | 2.05×        | 0.0659     | 31.33          | 0.9109    |
| Nano  | single     | v2v      | 3     | 1.79×        | 0.1910     | 22.31          | 0.7172    |
| Super | cfg2_hsdp2 | t2i      | 3     | 1.97×        | 0.2684     | 16.30          | 0.7757    |
| Super | cfg2_hsdp2 | v2v      | 3     | 1.78×        | 0.1086     | 25.98          | 0.8166    |
| Super | single     | t2i      | 3     | 1.92×        | 0.2632     | 15.87          | 0.7672    |
| Super | single     | transfer | 12    | 2.10×        | 0.0634     | 31.35          | 0.9204    |
| Super | single     | v2v      | 3     | 1.82×        | 0.1105     | 25.79          | 0.8097    |


Coverage includes T2I, V2V, edge, blur, depth, segmentation, world-space-map, and multi-control transfer inputs.

## Action compatibility


| Model | Topology   | Case                                                        | Shape   | Speedup | Video                                                                   |
| ----- | ---------- | ----------------------------------------------------------- | ------- | ------- | ----------------------------------------------------------------------- |
| Nano  | single     | [av_fd](action_compat/nano_av/comparisons/av_fd.json)       | `60x9`  | 1.47×   | [side-by-side](action_compat/nano_av/comparisons/av_fd_synced.mp4)      |
| Nano  | single     | [policy](action_compat/nano_policy/comparisons/policy.json) | `16x10` | 1.44×   | [side-by-side](action_compat/nano_policy/comparisons/policy_synced.mp4) |
| Super | cfg2_hsdp2 | [av_fd](action_compat/super_cfg2/comparisons/av_fd.json)    | `60x9`  | 1.66×   | [side-by-side](action_compat/super_cfg2/comparisons/av_fd_synced.mp4)   |




## Representative visual evidence

The aggregate results above cover all **102 paired visual generations**. This page publishes a curated set of **18 visual comparisons** plus **3 action comparisons**, rather than every seed. Complete per-seed artifacts remain in the validation output and were not copied to this report branch.

Each comparison uses matched seeds and inputs: **no cache is on the left, SeaCache is on the right**. Players use `preload="metadata"` so opening the report does not download every video in full.

### Nano — 1 GPU

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:1rem">
  <figure style="margin:0">
    <figcaption><strong>T2I</strong> · <a href="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png">open</a> · <a href="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/t2i/t2i_robot_draping__seed_0/comparison.json">metrics</a></figcaption>
    <img loading="lazy" src="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png" alt="Nano single-GPU T2I no-cache and SeaCache comparison" style="width:100%">
  </figure>
  <figure style="margin:0">
    <figcaption><strong>I2V</strong> · <a href="core_preview/lane0/cosmos3_nano__single/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4">open</a> · <a href="core_preview/lane0/cosmos3_nano__single/comparisons/i2v/i2v_humanoid_robot__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="core_preview/lane0/cosmos3_nano__single/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>T2V</strong> · <a href="core_preview/lane0/cosmos3_nano__single/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4">open</a> · <a href="core_preview/lane0/cosmos3_nano__single/comparisons/t2v/t2v_robot_kitchen__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="core_preview/lane0/cosmos3_nano__single/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>V2V</strong> · <a href="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4">open</a> · <a href="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/v2v/v2v_car_driving__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>Control transfer (blur)</strong> · <a href="breadth/nano_visual/nano_transfer_a/cosmos3_nano__single/comparisons/transfer/transfer_blur__seed_2026/synced.mp4">open</a> · <a href="breadth/nano_visual/nano_transfer_a/cosmos3_nano__single/comparisons/transfer/transfer_blur__seed_2026/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="breadth/nano_visual/nano_transfer_a/cosmos3_nano__single/comparisons/transfer/transfer_blur__seed_2026/synced.mp4" type="video/mp4"></video>
  </figure>
</div>

### Super — 1 GPU

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:1rem">
  <figure style="margin:0">
    <figcaption><strong>T2I</strong> · <a href="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png">open</a> · <a href="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/t2i/t2i_robot_draping__seed_0/comparison.json">metrics</a></figcaption>
    <img loading="lazy" src="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png" alt="Super single-GPU T2I no-cache and SeaCache comparison" style="width:100%">
  </figure>
  <figure style="margin:0">
    <figcaption><strong>I2V</strong> · <a href="super_core/single/lane0/cosmos3_super__single/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4">open</a> · <a href="super_core/single/lane0/cosmos3_super__single/comparisons/i2v/i2v_humanoid_robot__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/single/lane0/cosmos3_super__single/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>T2V</strong> · <a href="super_core/single/lane0/cosmos3_super__single/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4">open</a> · <a href="super_core/single/lane0/cosmos3_super__single/comparisons/t2v/t2v_robot_kitchen__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/single/lane0/cosmos3_super__single/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>V2V</strong> · <a href="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4">open</a> · <a href="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/v2v/v2v_car_driving__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="breadth/nano_visual/super_t2i_v2v/cosmos3_super__single/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>Control transfer (blur)</strong> · <a href="breadth/super_transfer/super_transfer_a/cosmos3_super__single/comparisons/transfer/transfer_blur__seed_2026/synced.mp4">open</a> · <a href="breadth/super_transfer/super_transfer_a/cosmos3_super__single/comparisons/transfer/transfer_blur__seed_2026/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="breadth/super_transfer/super_transfer_a/cosmos3_super__single/comparisons/transfer/transfer_blur__seed_2026/synced.mp4" type="video/mp4"></video>
  </figure>
</div>

### Super — 2 GPUs (`CFG=2`, `HSDP=2`)

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:1rem">
  <figure style="margin:0">
    <figcaption><strong>T2I</strong> · <a href="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png">open</a> · <a href="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/t2i/t2i_robot_draping__seed_0/comparison.json">metrics</a></figcaption>
    <img loading="lazy" src="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png" alt="Super two-GPU T2I no-cache and SeaCache comparison" style="width:100%">
  </figure>
  <figure style="margin:0">
    <figcaption><strong>I2V</strong> · <a href="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4">open</a> · <a href="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/i2v/i2v_humanoid_robot__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/i2v/i2v_humanoid_robot__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>T2V</strong> · <a href="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4">open</a> · <a href="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/t2v/t2v_robot_kitchen__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/cfg2_hsdp2/lane0/cosmos3_super__cfg2_hsdp2/comparisons/t2v/t2v_robot_kitchen__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>V2V</strong> · <a href="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4">open</a> · <a href="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/v2v/v2v_car_driving__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="breadth/super_transfer/super_cfg2_breadth/cosmos3_super__cfg2_hsdp2/comparisons/v2v/v2v_car_driving__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
</div>

### Super — 4 GPUs (`CFG=2`, `Ulysses=2`, `HSDP=4`)

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:1rem">
  <figure style="margin:0">
    <figcaption><strong>I2V</strong> · <a href="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/i2v/i2v_car_driving__seed_0/synced.mp4">open</a> · <a href="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/i2v/i2v_car_driving__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/i2v/i2v_car_driving__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>T2V</strong> · <a href="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/t2v/t2v_car_colliding__seed_0/synced.mp4">open</a> · <a href="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/t2v/t2v_car_colliding__seed_0/comparison.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="super_core/cfg2_ulysses2_hsdp4/lane0/cosmos3_super__cfg2_ulysses2_hsdp4/comparisons/t2v/t2v_car_colliding__seed_0/synced.mp4" type="video/mp4"></video>
  </figure>
</div>

### Action-generation coverage

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:1rem">
  <figure style="margin:0">
    <figcaption><strong>Nano, 1 GPU — AV forward dynamics</strong> · <a href="action_compat/nano_av/comparisons/av_fd_synced.mp4">open</a> · <a href="action_compat/nano_av/comparisons/av_fd.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="action_compat/nano_av/comparisons/av_fd_synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>Nano, 1 GPU — DROID policy</strong> · <a href="action_compat/nano_policy/comparisons/policy_synced.mp4">open</a> · <a href="action_compat/nano_policy/comparisons/policy.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="action_compat/nano_policy/comparisons/policy_synced.mp4" type="video/mp4"></video>
  </figure>
  <figure style="margin:0">
    <figcaption><strong>Super, 2 GPUs — AV forward dynamics</strong> · <a href="action_compat/super_cfg2/comparisons/av_fd_synced.mp4">open</a> · <a href="action_compat/super_cfg2/comparisons/av_fd.json">metrics</a></figcaption>
    <video controls preload="metadata" style="width:100%"><source src="action_compat/super_cfg2/comparisons/av_fd_synced.mp4" type="video/mp4"></video>
  </figure>
</div>

## Worst-case observed fidelity by generation task



### T2I

The highest T2I LPIPS is **0.3453** for Nano `single` / `t2i_robot_draping` seed 0. PSNR is **14.36 dB** and SSIM is **0.7478**. This is the task's worst case by LPIPS, but manual review found no clear visual degradation.

![Worst-case T2I side-by-side comparison](breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/t2i/t2i_robot_draping__seed_0/side_by_side.png)

[Open per-pair metrics](breadth/nano_visual/nano_t2i_v2v/cosmos3_nano__single/comparisons/t2i/t2i_robot_draping__seed_0/comparison.json)

### I2V

The highest I2V LPIPS is **0.2042** for Super `single` / `i2v_car_driving` seed 2. PSNR is **20.66 dB** and SSIM is **0.5741**. This is the task's worst case by LPIPS, but manual review found no clear visual degradation.

<video controls preload="metadata" style="width: 100%; max-width: 960px">
  <source src="super_core/single/lane3/cosmos3_super__single/comparisons/i2v/i2v_car_driving__seed_2/synced.mp4" type="video/mp4">
</video>

[Open synchronized I2V comparison](super_core/single/lane3/cosmos3_super__single/comparisons/i2v/i2v_car_driving__seed_2/synced.mp4)

[Open per-pair metrics](super_core/single/lane3/cosmos3_super__single/comparisons/i2v/i2v_car_driving__seed_2/comparison.json)

### T2V

The highest T2V LPIPS is **0.2978** for Nano `single` / `t2v_street_musicians` seed 1. PSNR is **16.87 dB** and SSIM is **0.5950**. This is the task's worst case by LPIPS, but manual review found no clear visual degradation.

**Prompt:** “Three people are playing guitar on the street. It is a bright daylight in the city center. In the background, there is a banner with the text "Music Fest 2026"”

Interestingly, the SeaCache output followed the prompt more accurately in this case: it rendered all three musicians, while the no-cache baseline rendered only two.

<video controls preload="metadata" style="width: 100%; max-width: 960px">
  <source src="core_preview/lane2/cosmos3_nano__single/comparisons/t2v/t2v_street_musicians__seed_1/synced.mp4" type="video/mp4">
</video>

[Open synchronized T2V comparison](core_preview/lane2/cosmos3_nano__single/comparisons/t2v/t2v_street_musicians__seed_1/synced.mp4)

[Open per-pair metrics](core_preview/lane2/cosmos3_nano__single/comparisons/t2v/t2v_street_musicians__seed_1/comparison.json)

