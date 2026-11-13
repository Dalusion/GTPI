# GTPI 2026 Preview

**Global Tax Performance Index**: an exploratory ranking of country outcomes relative to collected taxes. This edition is a redesign of the original [Dalusion/GTPI](https://github.com/Dalusion/GTPI), preserving the same 50 countries and the same three-file, no-build GitHub Pages setup.

> **Data status, November 13, 2026:** This is a **preview**, not a complete refresh of all three datasets. The human development and governance observations are refreshed. Tax-to-GDP values are carried over from the previous GTPI CSV, whose observation years were not documented. The upcoming UNDP Human Development Report (December 1, 2026) and OECD Revenue Statistics 2026 (December 10, 2026) are mentioned on the site but **not yet incorporated**.

## What's new

- Redesigned, responsive, dark-mode dashboard with no external JavaScript or CSS dependencies.
- 2023 **HDI** values published in the **UNDP Human Development Report 2025**.
- 2025 **Government Effectiveness 0–100** values published in the **World Bank's 2026 Worldwide Governance Indicators update**. This is **not** the older -2.5 to 2.5 estimate scale.
- Recalculated ranking scores from individual data inputs, correcting inconsistencies in the original hardcoded `Index Score` and `Rank` fields.
- A **2026 preview** model with adjustable HDI versus government-effectiveness weighting.
- A **Classic recalculated** model using the original HDI and original governance estimate. The two modes have different score scales, so **do not** compare score values between modes.
- Country search, sorting, interactive scatter chart, three-country comparison, and CSV export.
- Visible explanation of mixed-year data, upcoming releases, sources and limitations.

## Sources and reproducibility

| Data column                         | Data year  | Source                                                                                                                                                                                                          | Status                          |
| ----------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| `HDI_2023`                          | 2023       | [UNDP, Human Development Report 2025 statistical annex, Table 1](https://hdr.undp.org/sites/default/files/2025_HDR/HDR25_Statistical_Annex_HDI_Table.pdf)                                                       | Refreshed                       |
| `GovEffectiveness_2025_Score`       | 2025       | [World Bank, Government Effectiveness, governance score 0–100](https://data.worldbank.org/indicator/GOV_WGI_GE_SC); [WGI methodology](https://www.worldbank.org/en/publication/worldwide-governance-indicators) | Refreshed                       |
| `TaxToGDP_LegacyPct`                | Unrecorded | [Original GTPI](https://github.com/Dalusion/GTPI/blob/main/data.csv) `data.csv`                                                                                                                                 | **Legacy, not refreshed**       |
| `OriginalHDI`                       | Unrecorded | Original GTPI CSV                                                                                                                                                                                               | Preserved only for Classic mode |
| `OriginalGovEffectiveness_Estimate` | Unrecorded | Original GTPI CSV                                                                                                                                                                                               | Preserved only for Classic mode |

Country name harmonization: `Czech Republic` = `Czechia`, `South Korea` = `Korea (Republic of)`, `Turkey` = `Türkiye`, `Vietnam` = `Viet Nam`, `Russia` = `Russian Federation`, `Slovakia` = `Slovak Republic`. For each, the updated data correspond to the same country.

### 2026 preview formula

Let `w` range from 0 to 1 (default 0.5), and let `tax` be the tax-to-GDP **percentage**, not a fraction:

```
Outcome = w × (HDI_2023 × 100) + (1 - w) × GovEffectiveness_2025_Score
GTPI_preview = Outcome / max(TaxToGDP_LegacyPct, 15)
```

The inherited 15-percentage-point floor prevents extraordinarily large results for countries with very low tax percentages. This is a **modeling assumption** and should be revisited before final release.

### Classic recalculated formula

Let the tax-to-GDP percentage be divided by 100:

```
GTPI_classic = (OriginalHDI + OriginalGovEffectiveness_Estimate)
             / max(TaxToGDP_LegacyPct / 100, 0.15)
```

The old stored index scores and ranks are deliberately ignored because they contained calculation/sorting inconsistencies. The Classic scores can be negative because original governance estimates can be negative. They are not comparable numerically with preview scores.

**Interpretation caution:** Tax revenue as a share of GDP is not the same as government expenditure or an individual's tax rate. HDI contains income and other outcomes not wholly determined by public spending. Governance Effectiveness relies on estimates of perceptions, not direct audits. The index does not establish causal efficiency and is not an official ranking from any of these data providers.

## December update workflow

After **December 1, 2026**, check whether the new UNDP report includes a refreshed comparable HDI series. After **December 10, 2026**, check the OECD Revenue Statistics 2026 publication and the global database for tax-to-GDP observations with known years. Before publishing a final release:

1. Replace outdated rows in `data.csv` **only with verified, source-compatible values**.
2. Track the tax **observation year for each country** rather than assuming it equals the publication year.
3. If adding new countries, verify HDI, governance and tax inputs match the same territories, and add missing-data handling.
4. Update the version date, data labels, upcoming-release banner, methodology and README.
5. Check rank sensitivity to weights, minimum tax floor and inconsistent observation years.

## Credits

Originally built by **DevDevDavid**, with help from **Grok (xAI)**, in March 2025. 2026 preview updates provide a redesigned interface and refreshed datasets, while retaining the original project's independent and exploratory spirit.
