# India Air Quality Analysis (2015–2020) — Power BI Dashboard

An interactive Power BI dashboard that analyses daily air quality (AQI) across Indian cities from 2015 to mid-2020. It answers five questions: which cities are the most polluted, whether air quality improved, when pollution peaks during the year, which pollutants move most closely with AQI, and how often each city falls into the unhealthy AQI categories.

**Tools:** Power BI Desktop (Power Query for cleaning, report view for visuals)
**Author:** Jeet Rathod, B.Sc. Data Science student, Mumbai

---

## Dashboard preview

| Overview | Trends |
|---|---|
| ![Overview](C:\Users\Jeet\Downloads\India-AQI-Analysis\screenshots.png) | ![Trends](screenshots/Screenshot%202026-11-24%20100945.png) |

| Pollutants | AQI Categories |
|---|---|
| ![Pollutants](screenshots/Screenshot%202026-11-24%20101000.png) | ![AQI Categories](screenshots/Screenshot%202026-11-24%20101251.png) |

---

## Dataset

- **Source:** [Air Quality Data in India (2015–2020)](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india) on Kaggle
- **File used:** `city_day.csv` — daily readings, about 29,500 rows, 16 columns, 26 cities
- **Columns:** City, Date, PM2.5, PM10, NO, NO2, NOx, NH3, CO, SO2, O3, Benzene, Toluene, Xylene, AQI, AQI_Bucket
- The raw data is not included in this repository. Download it from the Kaggle link above.

The dataset also has hourly and station-level files. Only the daily city file was needed for these questions.

---

## Questions answered

1. Which cities are the most polluted?
2. Which pollutants track AQI most closely?
3. Did air quality improve between 2015 and 2020?
4. Which months have the worst and best air quality?
5. How large a share of each city's days falls in each AQI category (Good to Severe)?

---

## Data cleaning and preparation

| Step | What I did | Why |
|---|---|---|
| Headers | Promoted the first row to column headers | The CSV loaded with the real names sitting in row 1 as data |
| Data types | Set `Date` to Date, pollutants and AQI to Decimal Number | Everything loaded as text, and text can't be averaged or grouped by time |
| Missing values | Profiled every column on the full dataset and left blanks as blanks | A blank means no reading that day, not zero pollution. Power BI skips blanks when averaging |
| Duplicates | Checked City + Date pairs | None found |
| New columns | Added `Year` and `Month` from `Date` | Needed for the yearly trend and seasonality |
| Category order | Added a `Bucket Order` column (Good = 1 ... Severe = 6) and used it to sort `AQI_Bucket` | Power BI sorts text alphabetically, which put Poor before Satisfactory |

**Missing data by column:** City and Date 0%, CO 7%, NO and NO2 12%, SO2 13%, NOx and O3 14%, PM2.5 and AQI 16%, Benzene 19%, Toluene 27%, NH3 35%, PM10 38%, Xylene 61%. Xylene was left out of the analysis because most of its values are missing.

---

## Analysis decisions

- **Cities with short histories were excluded.** Some cities only started reporting in 2018–2020. 2020 is also the COVID-lockdown year, so those cities looked cleaner only because of when they were measured, and they also pulled the yearly average down. I excluded the 7 cities with fewer than 3 years of data (Aizawl, Chandigarh, Coimbatore, Ernakulam, Guwahati, Kochi, Shillong), leaving 19. The data stays in the model; this is a filter, not a deletion.
- **Median checked alongside the average.** Some cities have extreme readings, so I compared the average with the median for the city ranking. The top 3 stayed the same either way.
- **Readings above 500 were kept.** The official AQI scale ends at 500, but 541 readings exceed it. Because the ranking holds with both mean and median, I retained them instead of editing the data, and I note it here.
- **Averages, not sums.** Power BI adds numbers by default, which would make cities with more recorded days look dirtier. All AQI visuals use the average.

---

## Key findings

| Question | Finding |
|---|---|
| Most polluted cities | **Ahmedabad (avg 452, median 385), Delhi (259) and Patna (241)**, with Ahmedabad far ahead of the rest |
| Trend over time | Average AQI fell from about **212 in 2015 to about 122 in 2020** |
| Seasonality | Worst in **November–January**, best in **July–September** (monsoon) |
| Pollutants | Of the three compared (PM2.5, PM10, NO), **PM2.5 and PM10** track AQI most closely; NO shows a weaker link |
| AQI categories | Ahmedabad is in the Severe category on roughly half of its days, far above any other city. Delhi, Patna and Lucknow have large Very Poor shares, while Bengaluru, Chennai and Thiruvananthapuram are mostly Satisfactory or Moderate |

---

## Dashboard pages

1. **Overview** — KPI cards (average AQI, number of cities, readings above 500), a bubble map of average AQI by city, and a ranked bar chart.
2. **Trends** — yearly line chart, monthly column chart, and a City × Year matrix that shows which cities reported in which years.
3. **Pollutants** — scatter plots of average AQI against PM2.5, PM10 and NO, by city, with trend lines.
4. **AQI Categories** — a 100% stacked bar chart showing, for each city, the share of days in Good, Satisfactory, Moderate, Poor, Very Poor and Severe. Days with no category are excluded.

---

## Limitations

- **2020 is a partial year** (data ends mid-year) and includes the COVID lockdown, so it is not directly comparable with full years. The improvement from 2015 to 2020 should not be read as a clean trend.
- **Cities did not all report for the same years**, even after excluding short histories. Averages mix different periods for different cities.
- **The link between particulates and AQI is partly built in.** AQI is calculated from the worst pollutant at a station, and that is usually PM2.5 or PM10. The charts show which pollutants move with AQI, not what causes pollution.
- **Explanations for the seasonal pattern** (monsoon rain clearing the air, winter conditions trapping pollution, crop-residue burning, Diwali) are general air-quality knowledge. The data shows the pattern, not the cause.
- About 16% of AQI values are missing, mostly in the early years and in some cities.

---

## How to open

1. Download `city_day.csv` from the Kaggle link above.
2. Open `IndianAQI.pbix` in Power BI Desktop.
3. If Power BI can't find the file, go to **Transform data → Data source settings** and point it to your copy of `city_day.csv`.

---

## Repository structure

```
India-AQI-Analysis/
├── IndianAQI.pbix
├── README.md
└── screenshots/
    ├── Screenshot 2026-11-24 100856.png
    ├── Screenshot 2026-11-24 100945.png
    ├── Screenshot 2026-11-24 101000.png
    └── Screenshot 2026-11-24 101251.png
```

---
