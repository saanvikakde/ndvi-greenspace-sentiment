# Investigating the Correlation Between Urban Greenspace and Mental Health Through Geospatial and NLP Analysis

**Saanvi Kakde** | Computer Science, Arizona State University
**Mentor:** Adwith Malpe, Engineering Lecturer, Ira A. Fulton Schools of Engineering
**Program:** FURI (Fulton Undergraduate Research Initiative), Spring 2026

---

## Abstract

Urbanization shapes many aspects of city life, including the emotional wellbeing of residents. The level of greenery in a given area, quantified by the Normalized Difference Vegetation Index (NDVI), is a meaningful factor in city planning and access to higher levels has been linked to alleviate stress and anxiety. Pre-existing studies suggest this correlation; however, most are reliant on surveys, questionnaires, and traditional methods of data collection. This project maps vegetation density, using artificial intelligence to interpret sentiment from geo-tagged posts and local reviews, and models the correlation. NLP-based BERT sentiment classifiers were applied to analyze tone and extract indicators of psychological wellbeing. The two datasets were spatially joined to NDVI coordinate maps of multiple cities and analyzed using regression modeling to evaluate whether vegetation density meaningfully predicted sentiment patterns. While no statistically significant relationship was found, this research addresses the effect of urban environments on mental health, providing insight on urban planning, public health, and equitable access to greenspace.

---

## Key Findings

- **Pearson r = 0.004, p = 0.594** (n = 17,082) - no significant correlation between NDVI and tweet sentiment
- Median NDVI nearly identical across sentiment classes: negative (0.123), neutral (0.117), positive (0.125)
- Google Reviews for greenspace locations were overwhelmingly positive: NYC 91%, Chicago 97%, Maricopa 88%
- Average greenspace star rating exceeded 4.5/5 across all three cities
- Suggests intentional greenspace visits drive positive sentiment more than passive ambient exposure

---

## Pipeline

```
Sentinel-2 Imagery -> NDVI Computation -> Spatial Join -> BERT Sentiment Classifier <- Geo-tagged Text Data
```

---

## Data Sources

| Source | Description |
|--------|-------------|
| [Google Earth Engine](https://earthengine.google.com/) | Sentinel-2 satellite imagery for NDVI computation |
| [OpenStreetMap](https://www.openstreetmap.org/) via OSMnx | Greenspace polygon boundaries |
| Twitter CIKM 2010 dataset | 20,000 geo-tagged NYC tweets |
| Google Maps API | Greenspace reviews across NYC, Chicago, and Maricopa County |

> **Note:** Raw NDVI `.tif` files, `nyc_tweets.csv`, and the raw Twitter dataset are not included due to file size and redistribution restrictions. All processed CSVs needed to reproduce figures are included in `data/processed/`.

---

## Notebooks

| Notebook | Description |
|----------|-------------|
| `03_filter_nyc_tweets` | Filters Twitter dataset to NYC users and samples 20k tweets |
| `04_sentiment_analysis` | Runs Cardiff NLP RoBERTa sentiment classifier on tweets |
| `05_spatial_integration` | Joins tweet sentiment to NDVI raster; runs correlation analysis |
| `06_google_reviews_sentiment` | Collects and classifies Google Reviews via Maps API |
| `07_google_spatial_analysis` | Spatial join and correlation for Google Reviews across all cities |
| `08_graphs` | Generates poster figures — sentiment distributions and pipeline diagram |
| `09_heatmap` | Additional visualizations including NDVI explainer graphic |

Run in order: `03 -> 04 -> 05 -> 06 -> 07 -> 08 -> 09`

---

## Setup

```bash
git clone https://github.com/saanvikakde/ndvi-greenspace-sentiment.git
cd ndvi-greenspace-sentiment
pip install -r requirements.txt
```

**To reproduce from notebook 05 onward** (using included processed data):
```bash
jupyter lab
# Open notebooks/05_spatial_integration.ipynb and run from there
```

**To reproduce from scratch** (requires raw data):
1. Download Sentinel-2 NDVI rasters via [Google Earth Engine](https://earthengine.google.com/) and place in `data/raw/ndvi/`
2. Download the Twitter CIKM 2010 dataset and place in `data/raw/twitter/`
3. Add your Google Maps API key to notebook 06
4. Run all notebooks in order starting from `03`

---

## Tech Stack

- **Languages:** Python
- **NLP:** Hugging Face Transformers, Cardiff NLP `twitter-roberta-base-sentiment-latest`, DistilBERT
- **Geospatial:** GeoPandas, Rasterio, OSMnx, Contextily
- **Data:** Pandas, NumPy, SciPy
- **Visualization:** Matplotlib
- **Environment:** JupyterLab

---

## Project Structure

```
├── data/
│   ├── raw/                    # Not tracked - see Data Sources above
│   │   ├── ndvi/               # Sentinel-2 NDVI rasters
│   │   └── twitter/            # CIKM 2010 tweet dataset
│   └── processed/              # CSV outputs included in repo
│       ├── nyc_sample_20k.csv
│       ├── nyc_sample_with_sentiment.csv
│       ├── nyc_spatial_joined.csv
│       ├── nyc_users.csv
│       ├── google_reviews_nyc.csv
│       ├── google_reviews_ch.csv
│       └── google_reviews_mc.csv
├── notebooks/
└── README.md
```

---

## License

MIT License — see `LICENSE` for details.
