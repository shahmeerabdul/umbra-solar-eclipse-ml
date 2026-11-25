# Umbra: Solar Eclipse Classification with ML

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shahmeerabdul/umbra-solar-eclipse-ml/blob/main/solar.ipynb)

The **umbra** is the darkest part of the Moon's shadow. When it reaches Earth, you see a total solar eclipse.

This project uses machine learning to predict the **type of a solar eclipse** from its measured properties: how far the shadow passes from Earth's center, how much of the Sun is covered, where the Sun sits in the sky, and so on.

## The eclipse types

| Code | Type | What happens |
|---|---|---|
| **T** | Total | The Moon covers the Sun completely. The sky goes dark. |
| **A** | Annular | The Moon is too far from Earth to cover the Sun, leaving a bright "ring of fire". |
| **H** | Hybrid | Total in some places and annular in others along the same path. |
| **P** | Partial | Only part of the Sun is covered. |

The dataset also has sub-types such as `T+`, `A-` or `Hm`. The notebook keeps only the first letter, so every eclipse falls into one of the four main types.

## The data

The notebook reads `solar.csv`, a catalog of solar eclipses from 2000 BC to 3000 AD. It matches NASA's *Five Millennium Canon of Solar Eclipses*, which is also published on Kaggle as [Solar and Lunar Eclipses](https://www.kaggle.com/datasets/nasa/solar-eclipses).

After cleaning there are **7,698 eclipses** with these columns:

| Column | Meaning |
|---|---|
| Catalog Number | ID of the eclipse in the catalog |
| Calendar Date, Eclipse Time | When the eclipse peaks |
| Delta T (s) | Difference between Earth-rotation time and atomic time |
| Lunation Number | Count of new moons from a reference date |
| Saros Number | The ~18-year cycle the eclipse belongs to |
| **Eclipse Type** | What we predict (T, A, H, P and sub-types) |
| Gamma | How far the shadow's center passes from Earth's center (0 = dead center) |
| Eclipse Magnitude | How much of the Sun's diameter the Moon covers |
| Latitude, Longitude | Where the eclipse is greatest |
| Sun Altitude, Sun Azimuth | Where the Sun is in the sky at that point |
| Path Width (km) | Width of the total or annular shadow path |
| Central Duration | How long totality or annularity lasts |

## What the notebook does

1. **Load:** mounts Google Drive and finds `solar.csv`.
2. **Explore:** shows the columns, data types, summary statistics and first rows.
3. **Clean:**
   - Checks for duplicate rows (there are none).
   - Checks for missing values (there are none).
   - Turns `Path Width (km)` into a number. A `-` means there's no central shadow path, so it becomes `0`.
4. **Encode the target:** maps each eclipse type to a number in a new `Eclipse Class` column:
   `P = 0`, `A = 1`, `T = 2`, `H = 3`.

After cleaning, the classes are:

| Class | Count |
|---|---|
| Annular (1) | 3,956 |
| Total (2) | 3,173 |
| Hybrid (3) | 569 |

No partial eclipses are left in the cleaned data, so in practice the model chooses between annular, total and hybrid. Hybrid eclipses are much rarer than the other two. Keep that in mind when judging accuracy.

## The plan for the model

The notebook lays out the next steps:

1. **Features (X):** numeric columns such as `Gamma`, `Eclipse Magnitude`, `Sun Altitude`, `Sun Azimuth` and `Path Width (km)`.
2. **Target (y):** `Eclipse Class`.
3. **Split:** 80% of the eclipses for training and 20% held back for testing.
4. **Compare two models:**
   - **ID3 decision tree:** splits the data using information gain (entropy). It's easy to draw and explain, but it can overfit.
   - **Random Forest:** many decision trees, each trained on a random sample, that vote on the answer. It's usually more accurate and less prone to overfitting, but harder to explain.

Training and evaluating these models is the next part of the work.

## How to run it

1. Download `solar.csv` from the [Kaggle dataset](https://www.kaggle.com/datasets/nasa/solar-eclipses).
2. Put it anywhere in your Google Drive.
3. Click **Open in Colab** above and run the cells from top to bottom. Allow Drive access when asked, and the notebook will find the file.

**Needs:** Python, pandas and NumPy. These come pre-installed in Colab.
