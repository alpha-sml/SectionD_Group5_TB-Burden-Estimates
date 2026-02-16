<div align="center">
  
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00ff88,100:009e4f&height=220&section=header&text=The%20Cleaning%20Manual&fontSize=55&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Gold%20Standard%20Metrics%20%7C%20Verified%20Data&descAlignY=60&descSize=20&descAlign=50&v=36" width="100%"/>

<br>
<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=25&pause=1000&color=00FF88&center=true&vCenter=true&width=1000&lines=Filtering+Noise...;Aligning+Variables...;Imputing+Missing+Values...;100%25+Data+Verification+Complete." />

<br>
<h3 align="center"><i>Transforming messy raw data into Precision Insights.</i></h3>

<p>
    <img src="https://img.shields.io/badge/Dataset-WHO_Tuberculosis-bd93f9?style=for-the-badge&logo=world-health-organization&logoColor=white" />
    <img src="https://img.shields.io/badge/Tool-Google_Sheets-50fa7b?style=for-the-badge&logo=google-sheets&logoColor=white" />
    <img src="https://img.shields.io/badge/Status-VERIFIED_CLEAN-ffb86c?style=for-the-badge&logo=checkmark&logoColor=white" />
</p>

</div>

---

## 🌊 1. The Data Flow
*We filtered out the noise to focus on the signal.*

```mermaid
flowchart LR
    %% Dracula Palette (Dark/Vibrant)
    classDef raw fill:#ff5555,stroke:#44475a,stroke-width:2px,color:white,rx:10,ry:10;
    classDef process fill:#8be9fd,stroke:#44475a,stroke-width:2px,color:#282a36,rx:5,ry:5;
    classDef clean fill:#50fa7b,stroke:#44475a,stroke-width:2px,color:#282a36,rx:10,ry:10;

    A([📂 50+ Messy Columns]):::raw --> B{{⚙️ Filter & Rename}}:::process
    B --> C{{🧠 Impute Missing Values}}:::process
    C --> D([✨ 13 Clean Metrics]):::clean
```

---

## 🏷️ 2. The Renaming Map
*We standardized the cryptic technical names into human-readable business terms.*

| Category | 📄 Original Name | 🏷️ New Short Name |
| :--- | :--- | :--- |
| **Identification** | `Country` | **Country** |
| | `Year` | **Year** |
| | `G_whoregion` | **Region** |
| **TB Burden** | `E_inc_100k` | **TB Incidence (per 100k)** |
| | `E_inc_num` | **TB Cases** |
| **Population** | `E_pop_num` | **Population** |
| **Mortality** | `E_mort_exc_tbhiv_100k` | **TB Death Rate (No HIV, per 100k)** |
| | `E_mort_100k` | **TB Death Rate (per 100k)** |
| | `E_mort_num` | **TB Deaths** |
| **TB + HIV** | `E_inc_tbhiv_100k` | **TB-HIV Incidence (per 100k)** |
| | `E_tbhiv_prct` | **TB-HIV %** |
| **Treatment & Detection** | `Cfr_pct` | **Fatality Rate (%)** |
| | `C_cdr` | **Detection Rate (%)** |

---

## 🧬 3. The 13 Core Metrics (Deep Dive)
*We hand-picked these variables to answer critical business questions.*

### <img src="https://img.shields.io/badge/_-IDENTIFICATION-bd93f9?style=for-the-badge&logo=google-maps&logoColor=white" />

#### <img src="https://img.shields.io/badge/01-Country-44475a?style=flat-square&logo=globe&logoColor=8be9fd" />
**Formula:**
`=ARRAYFORMULA(Raw_Data!A2:A)`

**Why:**
*   Country is the primary unit of analysis.
*   Enables country-level comparison, ranking, and trend evaluation across years.
*   No cleaning was required as it is a categorical identifier.

#### <img src="https://img.shields.io/badge/02-Year-44475a?style=flat-square&logo=google-calendar&logoColor=ffb86c" />
**Formula:**
`=ARRAYFORMULA(Raw_Data!F2:F)`

**Why:**
*   Year enables time-series analysis (2000–2024).
*   Required for calculating trend-based KPIs such as incidence change and mortality reduction.

#### <img src="https://img.shields.io/badge/03-Region-44475a?style=flat-square&logo=map&logoColor=50fa7b" />
**Formula:**
`=ARRAYFORMULA(Raw_Data!E2:E)`

**Why:**
*   WHO Region enables regional aggregation and segmentation.
*   It is also used in regional-year median imputation for missing values.

#### <img src="https://img.shields.io/badge/04-Population-44475a?style=flat-square&logo=myspace&logoColor=ff79c6" />
**Formula:**
`=ARRAYFORMULA(Raw_Data!G2:G)`

**Why:**
*   Population allows calculation of total burden and weighted comparisons.
*   Since missing values were minimal (<1%), no imputation was required.

---

### <img src="https://img.shields.io/badge/_-TB_DISEASE_BURDEN-ff5555?style=for-the-badge&logo=microgen&logoColor=white" />

#### <img src="https://img.shields.io/badge/05-TB_Incidence_(per_100k)-ff5555?style=flat-square&logo=activity&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!H2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!H:H, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!H:H<>"")
     ),
   ""),
   Raw_Data!H2
)
```

**Why:**
*   TB incidence per 100k is the primary burden metric.
*   Median was chosen to reduce outlier influence.

#### <img src="https://img.shields.io/badge/06-TB_Death_Rate_(No_HIV)-ff5555?style=flat-square&logo=template&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!W2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!W:W, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!W:W<>"")
     ),
   ""),
   Raw_Data!W2
)
```

**Why:**
*   This metric measures TB mortality excluding HIV interaction.
*   Region-year median imputation ensures regional consistency and maintains temporal accuracy.

#### <img src="https://img.shields.io/badge/07-TB/HIV_Incidence-ff5555?style=flat-square&logo=virus&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!Q2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!Q:Q, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!Q:Q<>"")
     ),
   ""),
   Raw_Data!Q2
)
```

**Why:**
*   Measures TB burden among HIV-positive individuals.
*   Imputed using regional-year median to preserve comorbidity patterns within regions.

#### <img src="https://img.shields.io/badge/08-Total_TB_Death_Rate-ff5555?style=flat-square&logo=heartbeat&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!AI2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!AI:AI, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!AI:AI<>"")
     ),
   ""),
   Raw_Data!AI2
)
```

**Why:**
*   Represents total TB mortality rate.
*   Imputation ensures mortality trend continuity across regions and years.

#### <img src="https://img.shields.io/badge/09-Total_TB_Deaths-ff5555?style=flat-square&logo=hospital&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!AL2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!AL:AL, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!AL:AL<>"")
     ),
   ""),
   Raw_Data!AL2
)
```

**Why:**
*   Represents absolute TB deaths.
*   Imputed conservatively using region-year median due to minimal missingness.

#### <img src="https://img.shields.io/badge/10-Fatality_Rate_(%25)-ffb86c?style=flat-square&logo=percent&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!AR2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!AR:AR, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!AR:AR<>"")
     ),
   ""),
   Raw_Data!AR2
)
```

**Why:**
*   Case Fatality Rate measures disease severity and treatment effectiveness.
*   Median imputation prevents distortion from extreme high-burden countries.

#### <img src="https://img.shields.io/badge/11-Detection_Rate_(%25)-50fa7b?style=flat-square&logo=search&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!AV2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!AV:AV, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!AV:AV<>"")
     ),
   ""),
   Raw_Data!AV2
)
```

**Why:**
*   Detection rate measures health system performance and case identification efficiency.
*   Region-year median maintains structural integrity of reporting systems.

#### <img src="https://img.shields.io/badge/12-Total_TB_Cases-ff5555?style=flat-square&logo=chart-bar&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!K2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!K:K, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!K:K<>"")
     ),
   ""),
   Raw_Data!K2
)
```

**Why:**
*   Represents total estimated TB cases.
*   Imputed using regional-year median to maintain burden comparability.

#### <img src="https://img.shields.io/badge/13-TB/HIV_Percentage-ff79c6?style=flat-square&logo=dna&logoColor=white" />
**Formula:**
```excel
=IF(Raw_Data!N2="",
   IFERROR(
     MEDIAN(
       FILTER(Raw_Data!N:N, Raw_Data!E:E=C2, Raw_Data!F:F=B2, Raw_Data!N:N<>"")
     ),
   ""),
   Raw_Data!N2
)
```

**Why:**
*   Represents percentage of TB cases associated with HIV.
*   Median imputation preserves regional co-infection patterns.

---

<div align="center">
<b>Clean. Verified. Ready.</b>
</div>
