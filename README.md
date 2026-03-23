<div align="center">
<h2>Failure Modes for Deep Learning–Based Online Mapping: How to Measure and Address Them</h2> 
<h4>CVPR 2026</h4>
</div>
<p align="center">
    <!-- doc badges -->
    <a href="https://arxiv.org/abs/2603.19852v1">
        <img src='https://img.shields.io/badge/arXiv-Page-aff'>
    </a>
</p>


## Overview

This repository accompanies the paper  
**"Failure Modes for Deep Learning–Based Online Mapping: How to Measure and Address Them"**. We provide dataset splits and evaluation subsets for analyzing generalization in deep learning–based online mapping.

The focus is on separating two key effects:
- Generalization beyond **memorized geographic locations**
- Generalization to **new map geometries**

To this end, we provide the sample token lists for the following parts of the paper: **O_loc evaluation**, **O_geom evaluation** and **geometric splits**. In the following, we provide a brief explaination of the individual concepts, for more detailed information we refer to the paper.

---

### O_loc (Localization Overfitting)

The **O_loc evaluation** measures how much a model relies on **geographic proximity** to training data.

- Based on a distance threshold $T_\text{dist}$ (here 5 m, following [previous work](https://arxiv.org/abs/2312.06420)), validation data is split into:
  - **Close** (geographically near training samples)
  - **Far** (geographically distant)
- Geometric similarity distributions between **close** and **far** subsets are **aligned** to isolate localization effects and avoid confounding with geometric differences
- The relative performance drop defines O_loc

⚠️ **Important:**  
This evaluation **requires significant geographical overlap** between training and validation data.  
Therefore, **O_loc is only applicable to the original dataset splits provided in this repository.**

Interpretation:
- **Low O_loc** → generalizes beyond memorized locations  
- **High O_loc** → relies on location-specific cues  

---

### O_geom (Geometric Overfitting)

The **O_geom evaluation** measures how model performance changes as validation samples become **increasingly dissimilar in map element geometry** compared to the training set.

- Validation samples are grouped into bins (here 5) based on **geometric similarity**
- Performance is evaluated per bin
- The **rate of performance degradation** defines O_geom

Interpretation:
- **Low O_geom** → robust to new geometries  
- **High O_geom** → overfits to training geometries  

---

### Geometric Splits

The geometric splits are designed to **maximize differences in map element geometry** between training and validation sets.

- Training and validation data share **minimal geometric similarity of map elements**
- Reduces bias from repeated or similar road layouts
- Enables evaluation of **true geometric generalization**

These splits are useful when testing whether a model can handle **previously unseen map structures**, rather than relying on familiar patterns.

---

## Repository Structure

> ⚠️ **Disclaimer**  
> The content of this repository consists of **sample splits and evaluation sets only**.  
> The work originated in collaboration with *Aptiv Services Germany GmbH*, and publication of any code is permitted.  
> We provide these sets to **reproduce the results from the paper** and to **rebuild the benchmarks with minimal effort** in your own online mapping repository for truthful generalization evaluations.

---

We denote the geographical splits introduced by [Localization Is All You Evaluate: Data Leakage in Online Mapping Datasets and How to Fix It by Lilja et al.](https://arxiv.org/abs/2312.06420) and [StreamMapNet: Streaming Mapping Network for Vectorized Online HD Map Construction by Yuan et al.](https://arxiv.org/abs/2308.12570) as geo_split_local and geo_split_stream, respectively.

The repository is organized into three main components:
1. **O_loc validation sets** (localization-based evaluation)
2. **O_geom validation sets** (geometry-based evaluation bins)
3. **Geometric splits** (train/val/test)

---


### O_loc Validation Sets

| Dataset     | Split | Close samples (s(v) aligned) | Far samples (s(v) aligned) |
|-------------|-------|-----------------|---------------|
| Argoverse&nbsp;2 | original | 4,699           | 4,699         |
| nuScenes    | original | 1,236           | 1,236         |


### O_geom Validation Sets

n: number of samples

s<sub>i</sub>: average s(v) per bin

<table>
  <thead>
    <tr>
      <th rowspan="2">Dataset</th>
      <th rowspan="2">Split</th>
      <th colspan="2">Bin 1</th>
      <th colspan="2">Bin 2</th>
      <th colspan="2">Bin 3</th>
      <th colspan="2">Bin 4</th>
      <th colspan="2">Bin 5</th>
    </tr>
    <tr>
      <th>n</th><th>s₁</th>
      <th>n</th><th>s₂</th>
      <th>n</th><th>s₃</th>
      <th>n</th><th>s₄</th>
      <th>n</th><th>s₅</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Argoverse&nbsp;2</td>
      <td>geo_local</td>
      <td>5,653</td><td>3.29</td>
      <td>9,010</td><td>8.59</td>
      <td>6,408</td><td>13.90</td>
      <td>2,497</td><td>19.20</td>
      <td>226</td><td>24.50</td>
    </tr>
    <tr>
      <td>Argoverse&nbsp;2</td>
      <td>geo_stream</td>
      <td>7,090</td><td>3.27</td>
      <td>10,951</td><td>8.81</td>
      <td>4,652</td><td>14.34</td>
      <td>668</td><td>19.88</td>
      <td>21</td><td>25.42</td>
    </tr>
    <tr>
      <td>Argoverse&nbsp;2</td>
      <td>original</td>
      <td>3,011</td><td>3.59</td>
      <td>5,008</td><td>8.43</td>
      <td>3,286</td><td>13.27</td>
      <td>1,545</td><td>18.11</td>
      <td>90</td><td>22.95</td>
    </tr>
    <tr>
      <td>nuScenes</td>
      <td>geo_local</td>
      <td>868</td><td>5.87</td>
      <td>3,004</td><td>12.78</td>
      <td>979</td><td>19.69</td>
      <td>114</td><td>26.59</td>
      <td>25</td><td>33.50</td>
    </tr>
    <tr>
      <td>nuScenes</td>
      <td>geo_stream</td>
      <td>1,098</td><td>5.08</td>
      <td>2,473</td><td>10.40</td>
      <td>2,044</td><td>15.72</td>
      <td>313</td><td>21.04</td>
      <td>53</td><td>26.36</td>
    </tr>
    <tr>
      <td>nuScenes</td>
      <td>original</td>
      <td>155</td><td>5.09</td>
      <td>327</td><td>9.02</td>
      <td>415</td><td>12.96</td>
      <td>295</td><td>16.89</td>
      <td>44</td><td>20.83</td>
    </tr>
  </tbody>
</table>


### Geometric Splits

| Dataset     | Train samples   | Val samples    | Test samples  |
|------------|--------|--------|--------|
| Argoverse&nbsp;2 | 107,449 | 26,404 | 22,183 |
| nuScenes   | 27,326  | 5,942  | 6,889  |


---

We provide the sample tokens for all nuScenes/Argoverse&nbsp;2 splits mentioned in the paper. Each `.txt` file contains newline-separated sample tokens.

### Directory Tree

```text
online_mapping_geometric_splits/
├── O_geom_validation_sets/
│   ├── argoverse2/
│   │   ├── geo_split_local/ (23794 samples total)
│   │   │   ├── O_geom_av2_geo_split_local_val_far_bin_1.txt (5653 samples)
│   │   │   ├── O_geom_av2_geo_split_local_val_far_bin_2.txt (9010 samples)
│   │   │   ├── O_geom_av2_geo_split_local_val_far_bin_3.txt (6408 samples)
│   │   │   ├── O_geom_av2_geo_split_local_val_far_bin_4.txt (2497 samples)
│   │   │   ├── O_geom_av2_geo_split_local_val_far_bin_5.txt (226 samples)
│   │   │   └── O_geom_av2_geo_split_local_val_far_bins_s_avg.txt
│   │   ├── geo_split_stream/ (23382 samples total)
│   │   │   ├── O_geom_av2_geo_split_stream_val_far_bin_1.txt (7090 samples)
│   │   │   ├── O_geom_av2_geo_split_stream_val_far_bin_2.txt (10951 samples)
│   │   │   ├── O_geom_av2_geo_split_stream_val_far_bin_3.txt (4652 samples)
│   │   │   ├── O_geom_av2_geo_split_stream_val_far_bin_4.txt (668 samples)
│   │   │   ├── O_geom_av2_geo_split_stream_val_far_bin_5.txt (21 samples)
│   │   │   └── O_geom_av2_geo_split_stream_val_far_bins_s_avg.txt
│   │   └── original_split/ (12940 samples total)
│   │       ├── O_geom_av2_original_split_val_far_bin_1.txt (3011 samples)
│   │       ├── O_geom_av2_original_split_val_far_bin_2.txt (5008 samples)
│   │       ├── O_geom_av2_original_split_val_far_bin_3.txt (3286 samples)
│   │       ├── O_geom_av2_original_split_val_far_bin_4.txt (1545 samples)
│   │       ├── O_geom_av2_original_split_val_far_bin_5.txt (90 samples)
│   │       └── O_geom_av2_original_split_val_far_bins_s_avg.txt
│   └── nuscenes/
│       ├── geo_split_local/ (4990 samples total)
│       │   ├── O_geom_nuscenes_geo_split_local_val_far_bin_1.txt (868 samples)
│       │   ├── O_geom_nuscenes_geo_split_local_val_far_bin_2.txt (3004 samples)
│       │   ├── O_geom_nuscenes_geo_split_local_val_far_bin_3.txt (979 samples)
│       │   ├── O_geom_nuscenes_geo_split_local_val_far_bin_4.txt (114 samples)
│       │   ├── O_geom_nuscenes_geo_split_local_val_far_bin_5.txt (25 samples)
│       │   └── O_geom_nuscenes_geo_split_local_val_far_bins_s_avg.txt
│       ├── geo_split_stream/ (5981 samples total)
│       │   ├── O_geom_nuscenes_geo_split_stream_val_far_bin_1.txt (1098 samples)
│       │   ├── O_geom_nuscenes_geo_split_stream_val_far_bin_2.txt (2473 samples)
│       │   ├── O_geom_nuscenes_geo_split_stream_val_far_bin_3.txt (2044 samples)
│       │   ├── O_geom_nuscenes_geo_split_stream_val_far_bin_4.txt (313 samples)
│       │   ├── O_geom_nuscenes_geo_split_stream_val_far_bin_5.txt (53 samples)
│       │   └── O_geom_nuscenes_geo_split_stream_val_far_bins_s_avg.txt
│       └── original_split/ (1236 samples total)
│           ├── O_geom_nuscenes_original_split_val_far_bin_1.txt (155 samples)
│           ├── O_geom_nuscenes_original_split_val_far_bin_2.txt (327 samples)
│           ├── O_geom_nuscenes_original_split_val_far_bin_3.txt (415 samples)
│           ├── O_geom_nuscenes_original_split_val_far_bin_4.txt (295 samples)
│           ├── O_geom_nuscenes_original_split_val_far_bin_5.txt (44 samples)
│           └── O_geom_nuscenes_original_split_val_far_bins_s_avg.txt
├── O_loc_validation_sets/
│   ├── argoverse2/
│   │   └── original_split/
│   │       ├── O_loc_av2_original_split_val_close_dist_aligned.txt (4699 samples)
│   │       └── O_loc_av2_original_split_val_far_dist_aligned.txt (4699 samples)
│   └── nuscenes/
│       └── original_split/
│           ├── O_loc_nuscenes_original_split_val_close_dist_aligned.txt (1236 samples)
│           └── O_loc_nuscenes_original_split_val_far_dist_aligned.txt (1236 samples)
└── geometric_splits/
    ├── argoverse2/
    │   ├── av2_geometric_split_test.txt (22183 samples)
    │   ├── av2_geometric_split_train.txt (107449 samples)
    │   └── av2_geometric_split_val.txt (26404 samples)
    └── nuscenes/
        ├── nuscenes_geometric_split_test.txt (6889 samples)
        ├── nuscenes_geometric_split_train.txt (27326 samples)
        └── nuscenes_geometric_split_val.txt (5942 samples)