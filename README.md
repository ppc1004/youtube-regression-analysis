# What Drives YouTube Views? A Regression Analysis

**Can you predict YouTube views before a video goes live?** A linear regression analysis of 17,589 YouTube videos across 16 categories.

Website: https://ppc1004.github.io/youtube-regression-analysis/

Notebook: [notebooks/youtube_views_regression.ipynb](notebooks/youtube_views_regression.ipynb)

## Short answer

Not with this data. Likes explain views well, but likes only exist after a video is published. Using only before-upload information (category, length, resolution), the model was barely better than guessing the median.

## Results (3,518 test videos)

| Model | R² (log views) | Error vs baseline | Within 2x of real views |
|---|---:|---:|---:|
| Baseline: guess the median | -0.02 | 0% | 24% |
| Model A: category, length, resolution, likes, comments | 0.69 | -43% | 40% |
| Model B: category, length, resolution only | 0.11 | -6% | 25% |

## Key findings

1. **Likes are by far the strongest factor.** Roughly, 10% more likes goes with about 12% more views. This is a link, not cause and effect.
2. **Before-upload information alone predicts views poorly.** Category matters somewhat; length and resolution matter very little.
3. **Views are extremely skewed.** The median video has about 150 views, but the average is about 69,000, so the models predict log(views).
4. **Predicting views before upload would need other data**, such as channel size, title, thumbnail and upload time.

## Method

1. Load and check the data (missing values, zero-view videos, extreme values)
2. Split 80% train / 20% test **before** any analysis (random_state = 42)
3. Explore the training data with charts
4. Log-transform skewed columns, turn category into dummy columns, set a median baseline
5. Fit two linear regression models (scikit-learn)
6. Score on the test data with R², average error and share of predictions within 2x of real views

## Run it yourself

The raw dataset (`youtube.xlsx`) is not included because its source and license are not confirmed. Put it in `data/`, then:

```bash
pip install -r requirements.txt
jupyter notebook notebooks/youtube_views_regression.ipynb
```

## Project structure

```
youtube-regression-analysis/
├── notebooks/youtube_views_regression.ipynb   # full analysis
├── data/                                      # youtube.xlsx goes here (not uploaded)
└── docs/                                      # website (GitHub Pages)
    ├── index.html, style.css
    └── images/                                # charts from the notebook
```

## Limitations

- Likes and views were recorded at the same time, so Model A shows a strong link, not cause and effect.
- Some categories have very few videos (e.g. Shows: 12 in the training data), so their results are not reliable.
- One snapshot of videos, not views tracked over time.

*Independent data analysis project by Pay-Chern Liu.*
