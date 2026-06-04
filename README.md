# 科技職涯基礎能力空間分析
### 以 O\*NET Essential Skills 與 Abilities 建立職涯能力地圖

---

## 一、基本資料

| 項目 | 內容 |
|---|---|
| 專題題目 | 科技職涯基礎能力空間分析：以 O\*NET Essential Skills 與 Abilities 建立職涯能力地圖 |
| 組員姓名 | 廖文瑜 |
| 學號 | 11311248 |
| 課程名稱 | 大數據資料分析 |
| 使用工具 | Python、pandas、NumPy、scikit-learn、matplotlib、adjustText、O\*NET Database |
| Colab 連結 | https://colab.research.google.com/drive/1RJ76e75LUxjKAtKKezeY5_qLmR9IyN4f?usp=sharing |

---

## 二、專題動機

在思考未來科技職涯時，常見的分類方式通常是直接以職稱區分，例如 Software Engineer、Data Scientist、Machine Learning Engineer、System Engineer 等。然而，這種分類方式容易停留在職稱表面，無法進一步回答：

1. 不同科技職涯背後需要的基礎能力是否有相同之處？又有哪些不同之處？
2. 資料科學、演算法、系統、軟體工程等方向之間，能力結構是否接近？
3. 哪些能力是大多數科技職涯都需要的通用核心？哪些能力可以區分不同職涯方向？

因此，本專題使用 O\*NET Database 中的職業資料，將每個科技職業表示成一個「能力向量」，並以 Essential Skills 與 Abilities 建立 Foundation Capability Space。接著使用 KMeans clustering、PCA 降維、cosine distance 與 capability role analysis 來觀察科技職涯的能力結構。


本專題的核心想法是

> 先讓資料本身形成能力空間，再使用 Career Directions 作為後續解讀標籤。

換句話說，本專題不是先主觀規定哪些職業屬於哪一群，而是先用能力資料建立空間，再觀察科技職涯是否在這個空間中呈現某些自然結構。

---

## 三、研究問題

本專題設定三個主要研究問題：

### RQ1：科技職涯在 Foundation Capability Space 中是否自然形成群聚？

使用 O\*NET 的 Essential Skills 與 Abilities 建立 occupation-capability matrix，接著使用 KMeans clustering 對職業能力向量進行分群，並使用 PCA 圖觀察分群結果。

### RQ2：不同 Career Directions 在能力空間中的距離有多近？

將職業依照職稱關鍵字標記為 Software Engineering、Data / AI、Algorithms / Quantitative Computing、Systems / Infrastructure 等方向，並計算各方向 centroid 之間的 cosine distance。

### RQ3：哪些 capabilities 是通用核心？哪些是專精區辨能力？

計算每個 capability 在不同 career direction 中的平均重要性與變異程度，並用象限圖觀察哪些能力屬於 Universal Core，哪些能力屬於 Specialized Differentiator 或 Niche Differentiator。

---

## 四、資料來源與分析範圍

### 4.1 資料來源

本專題使用 O\*NET text database。程式會自動下載 O\*NET 資料庫 zip 檔，並讀取以下資料表：

- `Occupation Data.txt`
- `Essential Skills.txt`
- `Abilities.txt`

其中，`Essential Skills` 與 `Abilities` 是本專題建立 Foundation Capability Space 的主要資料來源。

### 4.2 分析範圍

本專題主要分析科技相關職業，篩選條件包含：

- O\*NET-SOC Code 以 `15-` 開頭的 Computer and Mathematical Occupations
- O\*NET-SOC Code 以 `17-` 開頭，且職稱包含科技、系統、資料、數學、最佳化等關鍵字的工程類職業

程式會排除與本專題科技職涯主軸較不一致的職稱，例如：

- `actuary`
- `actuarial`

---

## 五、研究方法

### 5.1 建立 Foundation Capability Space

程式會先將 Essential Skills 與 Abilities 整理成 long table，並只保留 Importance scale 的分數。接著建立 occupation-capability matrix。

矩陣概念如下：

| occupation | capability 1 | capability 2 | capability 3 | ... |
|---|---:|---:|---:|---:|
| Software Developer | 4.1 | 3.8 | 3.7 | ... |
| Data Scientist | 4.2 | 4.5 | 4.3 | ... |
| Systems Analyst | 3.9 | 3.5 | 3.8 | ... |

每一列代表一個職業，每一欄代表一項能力，每一格代表該能力對該職業的重要性分數。

接著使用 `StandardScaler` 將能力分數標準化，使每個 capability 在分析中具有較公平的尺度。

---

### 5.2 KMeans Clustering

為了回答 RQ1，本專題使用 KMeans 對職業能力向量進行探索式分群。程式會在內部比較不同的 k 值，並選擇較合適的群數作為後續 PCA 圖中的 cluster 標籤。


---

### 5.3 PCA 降維與主圖一：職業能力空間分布圖

由於原始能力空間是高維度資料，不容易直接觀察，因此本專題使用 PCA 將職業能力向量投影到二維平面。

PCA 圖的解讀方式如下：

- 每個點代表一個科技職業。
- 兩個點越接近，代表能力結構越相似。
- 不同顏色代表 KMeans 分出的不同 cluster。
- 圖上的文字標籤只標出距離中心較遠、較有代表性的職業，避免整張圖變成文字牆。


---

### 5.4 Direction Distance 與主圖二：職涯方向距離熱圖

為了回答 RQ2，本專題使用 Career Directions 作為解讀標籤。這些方向可以用來理解能力空間中的職涯方向關係，但它們不是拿來決定 KMeans 分群。

本專題使用四個主要方向：

| Career Direction | 說明 |
|---|---|
| Software Development | 軟體開發、系統設計與產品實作 |
| Data / AI | 資料處理、機器學習、模型分析與資料驅動決策 |
| Algorithms / Quantitative Computing | 演算法、最佳化、統計、數值方法與量化問題求解 |
| Systems / Infrastructure | 雲端、網路、系統與基礎架構 |

程式會先計算每個方向的能力中心點，也就是 centroid，再使用 cosine distance 計算不同方向之間的距離。

Cosine distance 的解讀方式如下：

- 距離越小：兩個方向的能力結構越相似。
- 距離越大：兩個方向的能力結構差異越明顯。

本版程式輸出的第二張主圖為：

```text
outputs_main_figures/figures/direction_distance_foundation.png
```

---

### 5.5 Capability Role Analysis 與主圖三：能力角色象限圖

為了回答 RQ3，本專題將 capability 依照兩個指標分析：

| 指標 | 意義 |
|---|---|
| universality score | 該能力在不同方向中的平均重要性 |
| specialization score | 該能力在不同方向中的差異程度 |

如果一項能力平均分數高，而且在不同方向之間差異小，代表它可能是 Universal Core。  
如果一項能力在不同方向之間差異大，代表它可能較能區分不同職涯方向。

能力角色大致分為：

| 角色 | 意義 |
|---|---|
| Universal Core | 多數科技方向都需要的通用核心能力 |
| Specialized Differentiator | 重要且能區分不同方向的專精能力 |
| Niche Differentiator | 偏特定方向需要的區辨能力 |
| Peripheral / Supporting | 較偏輔助性的能力 |

本版程式輸出的第三張主圖為：

```text
outputs_main_figures/figures/capability_role_quadrant.png
```

---

## 六、分析結果與討論

### 6.1 RQ1：自然群聚結果

KMeans 的結果可以用來觀察科技職涯是否在 Foundation Capability Space 中自然形成群聚。若 PCA 圖上的同色點大致聚集，代表這些職業在 Essential Skills 與 Abilities 上具有相似的能力結構。

需要注意的是，科技職涯之間通常共享許多基礎能力，例如：

- 批判思考
- 邏輯推理
- 問題解決
- 閱讀理解
- 主動學習

因此，即使 KMeans 可以分出 cluster，也不代表科技職涯是完全分裂成互不相干的領域，而是比較像在共同基礎能力上形成不同的專精差異。

---

### 6.2 RQ2：Career Direction 距離

Direction distance heatmap 可以用來觀察不同 career directions 的能力結構是否接近。

如果 Data / AI 與 Algorithms / Quantitative Computing 的距離較小，代表資料、AI、演算法與量化計算在基礎能力上有一定重疊，例如數學推理、分析能力與問題解決能力。

如果 Systems / Infrastructure 與 Algorithms / Quantitative Computing 的距離較大，代表系統基礎架構方向與數學演算法方向的能力重心較不同。前者可能更偏向系統維護、部署、網路與工程實務；後者則更偏向抽象推理、數學能力與模型化問題的能力。

---

### 6.3 RQ3：通用核心能力與專精區辨能力

Capability role quadrant 可以幫助我們區分不同能力在科技職涯中的角色。

左上方通常代表：

- 平均重要性高
- 不同方向差異較小
- 屬於跨科技方向的通用核心能力

右上方通常代表：

- 平均重要性高
- 不同方向差異也大
- 屬於重要且具有方向區辨性的能力

從這個分析結果可以看出「哪些能力分數高」，也進一步回答：

> 這項能力到底是所有方向都需要，還是某些方向特別需要？

---

## 七、主要語法解析與討論

### 7.1 `Path` 與資料夾管理

```python
DATA_DIR = Path("data/onet")
ZIP_PATH = DATA_DIR / "onet_30_3_text.zip"
EXTRACT_DIR = DATA_DIR / "onet_30_3_text"
```

程式使用 `pathlib.Path` 管理檔案路徑。這種寫法比直接用字串組合路徑更清楚，也比較不容易因作業系統差異而出錯。

---

### 7.2 `ensure_dirs()`

```python
def ensure_dirs() -> None:
    DATA_DIR.mkdir(parents=True, exist_ok=True)
    FIGURE_DIR.mkdir(parents=True, exist_ok=True)
```

這個函式負責建立必要資料夾。`parents=True` 代表如果上層資料夾不存在，會一起建立；`exist_ok=True` 代表資料夾已存在也不會報錯。

---

### 7.3 `clean_text()`

```python
def clean_text(x: object) -> str:
    if pd.isna(x):
        return ""
    return re.sub(r"\s+", " ", str(x)).strip()
```

這個函式用來清理文字資料。它會先處理缺失值，再把多個空白、換行或 tab 轉成單一空格，最後去掉前後空白。

---

### 7.4 pandas 資料讀取

```python
pd.read_csv(path, sep="	", encoding="utf-8", dtype=str, low_memory=False)
```

雖然函式名稱是 `read_csv()`，但只要指定 `sep="	"`，就可以讀取 tab-separated text file。`dtype=str` 可以避免職業代碼被 pandas 誤判成數字。

---

### 7.5 建立 Pivot Table

```python
matrix_raw = foundation_long.pivot_table(
    index=["soc_code", "occupation_title"],
    columns="capability_full",
    values="data_value",
    aggfunc="mean",
)
```

這段將 long table 轉成 matrix。每一列是職業，每一欄是能力，每格是能力重要性分數。這是後續 KMeans、PCA 與 distance analysis 的基礎。

---

### 7.6 StandardScaler

```python
matrix_scaled = pd.DataFrame(
    StandardScaler().fit_transform(matrix_raw),
    index=matrix_raw.index,
    columns=matrix_raw.columns,
)
```

`StandardScaler` 會將每個能力欄位標準化，使其平均值約為 0、標準差約為 1。這樣可以避免某些能力因分數尺度或分布差異而過度影響 KMeans 與 PCA。

---

### 7.7 KMeans 與 群數選擇

```python
labels = KMeans(
    n_clusters=k,
    random_state=RANDOM_STATE,
    n_init=30,
).fit_predict(matrix_scaled)
```
KMeans 用來將職業依照能力結構分群。程式會在 `k = 2` 到 `k = 8` 中測試不同群數，並使用內部評估方式選出較合適的 `best_k`。


---

### 7.8 PCA

```python
pca = PCA(n_components=2, random_state=RANDOM_STATE)
coords = pca.fit_transform(matrix_scaled)
```

PCA 用來將高維能力資料壓縮到二維，方便視覺化。需要注意的是，PCA 圖只是高維資料的投影，不應過度解讀單一平面距離。

---

### 7.9 Cosine Distance

```python
distance = cosine_distances(centroid_df.values)
```

Cosine distance 用來衡量不同 career direction centroid 的方向差異。距離越小，代表兩個方向的能力結構越相似。

---

### 7.10 Capability Role Classification

程式根據 `universality_score` 與 `specialization_score` 將能力分為不同角色。概念上：

- 高 universality、低 specialization：通用核心能力
- 高 specialization：專精區辨能力
- 低 universality、低 specialization：輔助能力

這使分析不只停留在「哪些能力分數高」，而能進一步討論「能力在職涯結構中扮演什麼角色」。

---

## 八、實作範例

### 8.1 執行方式

在 Colab 或本機環境中安裝必要套件：

```bash
pip install pandas numpy requests matplotlib scikit-learn adjustText
```

執行主程式：

```bash
python onet_main_figures_clean.py
```

程式會自動：

1. 建立資料夾
2. 下載 O\*NET database
3. 解壓縮資料
4. 讀取 Occupation Data、Essential Skills、Abilities
5. 篩選科技相關職業
6. 建立 Foundation Capability Matrix
7. 執行 KMeans、PCA、direction distance 與 capability role analysis
8. 輸出三張主圖

---

### 8.2 主要輸出檔案

本版程式只輸出三張主圖：

```text
outputs_main_figures/
└── figures/
    ├── pca_foundation_by_cluster.png
    ├── direction_distance_foundation.png
    └── capability_role_quadrant.png
```

這樣做的目的，是讓專題展示更聚焦在主要分析結果，而不是產生過多中間檔案。

---

### 8.3 圖表解讀範例

#### PCA by Cluster

如果兩個職業在 PCA 圖上距離較近，代表它們在 Essential Skills 與 Abilities 的整體能力結構較接近。

#### Direction Distance Heatmap

如果兩個 career directions 的 cosine distance 較小，代表它們在基礎能力上的需求較相似。

#### Capability Role Quadrant

此圖用 universality score 與 specialization score 區分能力角色。右上角代表重要且具方向區辨性的能力；左上角則代表廣泛適用於多數方向的通用核心能力。

---

## 九、心得與反思

這次專題讓我體會到，資料分析不只是把資料丟進模型，而是要先想清楚研究問題、資料結構與分析邏輯。原本若只是把科技職涯直接分成幾類，可能會變成主觀分類；但透過能力矩陣、KMeans、PCA 與 cosine distance，可以用比較資料導向的方式觀察職涯之間的關係。

我也發現，科技職涯之間其實不是完全分離的。不同科技方向共享許多基礎能力，例如批判思考、理解能力、推理能力與主動學習能力。這代表如果想進入科技領域，除了學某個特定工具，也需要培養可以跨領域轉移的基礎能力。

另一方面，數學相關能力通常會在 Algorithms / Quantitative Computing 方向中扮演更明顯的區辨角色。這讓我更理解數學背景在演算法、最佳化、數值計算與量化問題中的價值。

這次實作中，我學到如何將 pandas 資料整理、scikit-learn 模型與視覺化圖表整合成完整流程。比起單純呼叫套件，這個專題更重要的是理解每一步為什麼要做，以及結果可以如何被解讀。

---

## 十、限制與未來改進

本專題仍有一些限制：

1. Career Directions 是用職稱關鍵字標記，因此分類仍有近似性。
2. O\*NET 描述的是職業資料，不一定完全等於即時職缺市場需求。
3. PCA 是二維投影，無法保留所有高維能力結構。
4. KMeans 分群是探索性方法，不代表科技職涯只能分成某幾類。
5. 本版程式為主圖乾淨版，因此沒有輸出完整中間表格；若要進一步驗證數值，可以再開啟 CSV 輸出版本。

未來可以加入即時職缺資料，例如 104、LinkedIn 或其他 job posting 資料，進一步比較 O\*NET 職業能力資料與實際市場需求是否一致。

---

## 十一、完整程式碼

```python
import re
import zipfile
from pathlib import Path
from typing import Dict, List, Tuple

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import requests
from adjustText import adjust_text
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA
from sklearn.metrics import silhouette_score
from sklearn.metrics.pairwise import cosine_distances
from sklearn.preprocessing import StandardScaler


# ============================================================
# CONFIG
# ============================================================

ONET_ZIP_URL = "https://www.onetcenter.org/dl_files/database/db_30_3_text.zip"

DATA_DIR = Path("data/onet")
ZIP_PATH = DATA_DIR / "onet_30_3_text.zip"
EXTRACT_DIR = DATA_DIR / "onet_30_3_text"

OUTPUT_DIR = Path("outputs_main_figures")
FIGURE_DIR = OUTPUT_DIR / "figures"

FORCE_DOWNLOAD = False
RANDOM_STATE = 42

K_RANGE = range(2, 9)
MIN_CLUSTER_SIZE = 3
TOP_OCCUPATION_LABELS = 18

TECH_SOC_PREFIXES = ["15-", "17-"]

TECH_TITLE_KEYWORDS = [
    "software", "computer", "database", "network", "web",
    "developer", "programmer", "systems", "data scientist",
    "data science", "machine learning", "artificial intelligence",
    "ai", "ml", "cloud", "devops", "embedded", "firmware",
    "statistician", "statistics", "mathematician",
    "operations research", "optimization", "industrial engineer",
]

EXCLUDED_TITLE_KEYWORDS = ["actuary", "actuarial"]

CAREER_DIRECTIONS: Dict[str, List[str]] = {
    "Software Engineering": [
        "software developer", "software engineer", "web developer",
        "computer programmer", "application developer", "frontend",
        "backend", "full stack",
    ],
    "Data / AI": [
        "data scientist", "data science", "data engineer", "data analyst",
        "machine learning", "machine learning engineer",
        "artificial intelligence", "ai engineer", "deep learning",
        "analytics", "research scientist", "computer and information research",
        "database architect", "business intelligence analyst",
    ],
    "Algorithms / Quantitative Computing": [
        "operations research", "optimization", "algorithm", "quantitative",
        "computational", "simulation", "numerical", "statistician",
        "mathematician",
    ],
    "Systems / Infrastructure": [
        "network", "systems administrator", "computer systems analyst",
        "cloud", "infrastructure", "devops", "platform engineer",
        "system engineer",
    ],
}


# ============================================================
# BASIC UTILITIES
# ============================================================

def ensure_dirs() -> None:
    DATA_DIR.mkdir(parents=True, exist_ok=True)
    FIGURE_DIR.mkdir(parents=True, exist_ok=True)


def clean_text(x: object) -> str:
    if pd.isna(x):
        return ""
    return re.sub(r"\s+", " ", str(x)).strip()


def get_code_col(df: pd.DataFrame) -> str:
    for col in df.columns:
        if "O*NET-SOC Code" in col:
            return col
    raise KeyError("Cannot find O*NET-SOC Code column.")


def shorten_label(text: str, max_len: int = 32) -> str:
    text = str(text)
    return text if len(text) <= max_len else text[:max_len - 3] + "..."


# ============================================================
# DATA LOADING
# ============================================================

def download_onet_database() -> None:
    ensure_dirs()

    if not ZIP_PATH.exists() or FORCE_DOWNLOAD:
        print("[Download] Downloading O*NET database...")
        response = requests.get(ONET_ZIP_URL, timeout=120)
        response.raise_for_status()
        ZIP_PATH.write_bytes(response.content)

    if not EXTRACT_DIR.exists() or FORCE_DOWNLOAD:
        print("[Extract] Extracting O*NET database...")
        EXTRACT_DIR.mkdir(parents=True, exist_ok=True)
        with zipfile.ZipFile(ZIP_PATH, "r") as zip_ref:
            zip_ref.extractall(EXTRACT_DIR)


def find_file(filename: str) -> Path:
    target = filename.lower()
    for path in EXTRACT_DIR.rglob("*"):
        if path.is_file() and path.name.lower() == target:
            return path
    raise FileNotFoundError(f"Cannot find {filename} in extracted O*NET files.")


def read_onet_txt(filename: str) -> pd.DataFrame:
    path = find_file(filename)
    print(f"[Read] {path}")
    return pd.read_csv(path, sep="\t", encoding="utf-8", dtype=str, low_memory=False)


def load_onet_tables() -> Tuple[pd.DataFrame, pd.DataFrame, pd.DataFrame]:
    occupations = read_onet_txt("Occupation Data.txt")
    essential_skills = read_onet_txt("Essential Skills.txt")
    abilities = read_onet_txt("Abilities.txt")
    return occupations, essential_skills, abilities


# ============================================================
# DATA PREPARATION
# ============================================================

def filter_target_occupations(occupations: pd.DataFrame) -> pd.DataFrame:
    code_col = get_code_col(occupations)

    def is_target(row: pd.Series) -> bool:
        code = clean_text(row[code_col])
        title = clean_text(row["Title"]).lower()

        if not any(code.startswith(prefix) for prefix in TECH_SOC_PREFIXES):
            return False

        if any(keyword in title for keyword in EXCLUDED_TITLE_KEYWORDS):
            return False

        if code.startswith("15-"):
            return True

        if code.startswith("17-"):
            return any(keyword in title for keyword in TECH_TITLE_KEYWORDS)

        return False

    result = occupations[occupations.apply(is_target, axis=1)].copy()
    result = result[[code_col, "Title"]].drop_duplicates()
    result.columns = ["soc_code", "occupation_title"]
    return result.sort_values(["soc_code", "occupation_title"]).reset_index(drop=True)


def map_occupations_to_directions(target_occupations: pd.DataFrame) -> pd.DataFrame:
    rows: List[dict] = []

    for _, occ in target_occupations.iterrows():
        title = str(occ["occupation_title"]).lower()
        matched = [
            direction
            for direction, keywords in CAREER_DIRECTIONS.items()
            if any(keyword.lower() in title for keyword in keywords)
        ]

        if not matched:
            matched = ["Other Tech"]

        for direction in matched:
            rows.append({
                "soc_code": occ["soc_code"],
                "occupation_title": occ["occupation_title"],
                "career_direction": direction,
            })

    return pd.DataFrame(rows)


def prepare_foundation_table(
    df: pd.DataFrame,
    target_occupations: pd.DataFrame,
    domain_name: str,
) -> pd.DataFrame:
    code_col = get_code_col(df)

    temp = df[df[code_col].isin(target_occupations["soc_code"])].copy()
    temp["data_value"] = pd.to_numeric(temp["Data Value"], errors="coerce")
    temp = temp.dropna(subset=["data_value"])

    temp = temp.rename(columns={
        "Element Name": "capability",
        "Scale ID": "scale_name",
    })

    temp["scale_name"] = temp["scale_name"].map({"IM": "Importance", "LV": "Level"})
    temp = temp[temp["scale_name"] == "Importance"].copy()

    temp = temp.merge(
        target_occupations,
        left_on=code_col,
        right_on="soc_code",
        how="left",
    )

    temp["domain"] = domain_name
    temp["capability"] = temp["capability"].apply(clean_text)
    temp["capability_full"] = temp["domain"] + " | " + temp["capability"]

    return temp[[
        "soc_code", "occupation_title", "domain",
        "capability", "capability_full", "data_value",
    ]].drop_duplicates()


def build_foundation_matrix(foundation_long: pd.DataFrame) -> Tuple[pd.DataFrame, pd.DataFrame]:
    matrix_raw = foundation_long.pivot_table(
        index=["soc_code", "occupation_title"],
        columns="capability_full",
        values="data_value",
        aggfunc="mean",
    )

    matrix_raw = matrix_raw.replace([np.inf, -np.inf], np.nan)
    matrix_raw = matrix_raw.fillna(matrix_raw.mean()).fillna(0)

    matrix_scaled = pd.DataFrame(
        StandardScaler().fit_transform(matrix_raw),
        index=matrix_raw.index,
        columns=matrix_raw.columns,
    )

    return matrix_raw, matrix_scaled


# ============================================================
# ANALYSIS FOR MAIN FIGURES
# ============================================================

def choose_best_k(matrix_scaled: pd.DataFrame) -> int:
    rows: List[dict] = []
    n_samples = matrix_scaled.shape[0]

    for k in K_RANGE:
        if k >= n_samples:
            continue

        labels = KMeans(
            n_clusters=k,
            random_state=RANDOM_STATE,
            n_init=30,
        ).fit_predict(matrix_scaled)

        counts = pd.Series(labels).value_counts()
        rows.append({
            "k": k,
            "silhouette_score": silhouette_score(matrix_scaled, labels),
            "min_cluster_size": counts.min(),
        })

    result = pd.DataFrame(rows)
    valid = result[result["min_cluster_size"] >= MIN_CLUSTER_SIZE]
    if valid.empty:
        valid = result

    return int(valid.sort_values("silhouette_score", ascending=False).iloc[0]["k"])


def run_kmeans(matrix_scaled: pd.DataFrame, k: int) -> pd.DataFrame:
    labels = KMeans(
        n_clusters=k,
        random_state=RANDOM_STATE,
        n_init=30,
    ).fit_predict(matrix_scaled)

    result = matrix_scaled.index.to_frame(index=False)
    result.columns = ["soc_code", "occupation_title"]
    result["cluster"] = labels
    return result


def run_pca(matrix_scaled: pd.DataFrame) -> Tuple[pd.DataFrame, pd.DataFrame]:
    pca = PCA(n_components=2, random_state=RANDOM_STATE)
    coords = pca.fit_transform(matrix_scaled)

    pca_df = matrix_scaled.index.to_frame(index=False)
    pca_df.columns = ["soc_code", "occupation_title"]
    pca_df["PC1"] = coords[:, 0]
    pca_df["PC2"] = coords[:, 1]

    variance_df = pd.DataFrame({
        "component": ["PC1", "PC2"],
        "explained_variance_ratio": pca.explained_variance_ratio_,
    })

    return pca_df, variance_df


def build_direction_distance(
    matrix_scaled: pd.DataFrame,
    direction_mapping: pd.DataFrame,
) -> pd.DataFrame:
    centroids = []

    for direction in CAREER_DIRECTIONS:
        part = direction_mapping[direction_mapping["career_direction"] == direction]
        keys = sorted({
            (row["soc_code"], row["occupation_title"])
            for _, row in part.iterrows()
            if (row["soc_code"], row["occupation_title"]) in matrix_scaled.index
        })

        if keys:
            centroid = matrix_scaled.loc[keys].mean(axis=0)
            centroid.name = direction
            centroids.append(centroid)

    centroid_df = pd.DataFrame(centroids)
    distance = cosine_distances(centroid_df.values)
    return pd.DataFrame(distance, index=centroid_df.index, columns=centroid_df.index)


def build_capability_roles(
    matrix_raw: pd.DataFrame,
    direction_mapping: pd.DataFrame,
) -> pd.DataFrame:
    direction_means = []

    for direction in CAREER_DIRECTIONS:
        part = direction_mapping[direction_mapping["career_direction"] == direction]
        keys = sorted({
            (row["soc_code"], row["occupation_title"])
            for _, row in part.iterrows()
            if (row["soc_code"], row["occupation_title"]) in matrix_raw.index
        })

        if keys:
            mean_row = matrix_raw.loc[keys].mean(axis=0)
            mean_row.name = direction
            direction_means.append(mean_row)

    direction_means = pd.DataFrame(direction_means)

    rows = []
    for capability in direction_means.columns:
        values = direction_means[capability].to_numpy(dtype=float)
        rows.append({
            "capability": capability,
            "capability_name": capability.split(" | ", 1)[1],
            "universality_score": float(np.mean(values)),
            "specialization_score": float(np.std(values, ddof=0)),
        })

    result = pd.DataFrame(rows)

    high_universality = result["universality_score"].quantile(0.70)
    high_specialization = result["specialization_score"].quantile(0.70)
    low_specialization = result["specialization_score"].quantile(0.35)
    median_universality = result["universality_score"].median()

    def classify(row: pd.Series) -> str:
        if row["universality_score"] >= high_universality and row["specialization_score"] <= low_specialization:
            return "Universal Core"
        if row["specialization_score"] >= high_specialization and row["universality_score"] >= median_universality:
            return "Specialized Differentiator"
        if row["specialization_score"] >= high_specialization:
            return "Niche Differentiator"
        return "Peripheral / Supporting"

    result["capability_role"] = result.apply(classify, axis=1)
    return result


# ============================================================
# MAIN FIGURES
# ============================================================

def plot_pca_by_cluster(
    pca_df: pd.DataFrame,
    cluster_assignments: pd.DataFrame,
    variance_df: pd.DataFrame,
) -> None:
    data = pca_df.merge(cluster_assignments, on=["soc_code", "occupation_title"])

    plt.figure(figsize=(11, 8))
    ax = plt.gca()

    for cluster_id, group in data.groupby("cluster"):
        ax.scatter(
            group["PC1"],
            group["PC2"],
            label=f"Cluster {cluster_id}",
            alpha=0.75,
            s=55,
        )

    data["distance_from_origin"] = np.sqrt(data["PC1"] ** 2 + data["PC2"] ** 2)
    label_df = data.sort_values("distance_from_origin", ascending=False).head(TOP_OCCUPATION_LABELS)

    texts = [
        ax.text(row["PC1"], row["PC2"], shorten_label(row["occupation_title"]), fontsize=8)
        for _, row in label_df.iterrows()
    ]
    adjust_text(texts, ax=ax, arrowprops=dict(arrowstyle="-", color="gray", lw=0.5))

    pc1 = variance_df.loc[variance_df["component"] == "PC1", "explained_variance_ratio"].iloc[0]
    pc2 = variance_df.loc[variance_df["component"] == "PC2", "explained_variance_ratio"].iloc[0]

    ax.set_xlabel(f"PC1 ({pc1:.1%} variance)")
    ax.set_ylabel(f"PC2 ({pc2:.1%} variance)")
    ax.set_title("PCA Map of Foundation Capability Space by Natural Clusters")
    ax.legend(loc="best", fontsize=9)
    ax.grid(True, alpha=0.25)

    plt.tight_layout()
    plt.savefig(FIGURE_DIR / "pca_foundation_by_cluster.png", dpi=220)
    plt.close()


def plot_direction_distance_heatmap(distance_matrix: pd.DataFrame) -> None:
    plt.figure(figsize=(8, 6))
    ax = plt.gca()
    image = ax.imshow(distance_matrix.values, aspect="auto")
    plt.colorbar(image, ax=ax, label="Cosine distance")

    ax.set_xticks(np.arange(distance_matrix.shape[1]))
    ax.set_yticks(np.arange(distance_matrix.shape[0]))
    ax.set_xticklabels(distance_matrix.columns, rotation=35, ha="right")
    ax.set_yticklabels(distance_matrix.index)

    for i in range(distance_matrix.shape[0]):
        for j in range(distance_matrix.shape[1]):
            ax.text(j, i, f"{distance_matrix.iloc[i, j]:.2f}", ha="center", va="center", fontsize=9)

    ax.set_title("Capability Distance Between Career Directions")
    plt.tight_layout()
    plt.savefig(FIGURE_DIR / "direction_distance_foundation.png", dpi=220)
    plt.close()


def plot_capability_role_quadrant(capability_roles: pd.DataFrame) -> None:
    plt.figure(figsize=(12, 8))
    ax = plt.gca()

    for role, group in capability_roles.groupby("capability_role"):
        ax.scatter(
            group["specialization_score"],
            group["universality_score"],
            label=role,
            alpha=0.75,
            s=55,
        )

    ax.axvline(capability_roles["specialization_score"].median(), color="gray", linestyle="--", linewidth=1)
    ax.axhline(capability_roles["universality_score"].median(), color="gray", linestyle="--", linewidth=1)

    label_df = pd.concat([
        capability_roles[capability_roles["capability_role"] == "Universal Core"]
        .sort_values("universality_score", ascending=False)
        .head(6),
        capability_roles[capability_roles["capability_role"] == "Specialized Differentiator"]
        .sort_values("specialization_score", ascending=False)
        .head(6),
        capability_roles[capability_roles["capability_role"] == "Niche Differentiator"]
        .sort_values("specialization_score", ascending=False)
        .head(4),
    ]).drop_duplicates("capability")

    texts = [
        ax.text(
            row["specialization_score"],
            row["universality_score"],
            shorten_label(row["capability_name"], 28),
            fontsize=8,
        )
        for _, row in label_df.iterrows()
    ]
    adjust_text(texts, ax=ax, arrowprops=dict(arrowstyle="-", color="gray", lw=0.5))

    ax.set_xlabel("Specialization score: variation across directions")
    ax.set_ylabel("Universality score: average importance across directions")
    ax.set_title("Capability Roles: Universal Core vs Specialized Differentiators")
    ax.grid(True, alpha=0.25)
    ax.legend(bbox_to_anchor=(1.02, 1), loc="upper left", fontsize=8)

    plt.tight_layout(rect=[0, 0, 0.82, 1])
    plt.savefig(FIGURE_DIR / "capability_role_quadrant.png", dpi=220)
    plt.close()


# ============================================================
# PIPELINE
# ============================================================

def run_pipeline() -> None:
    ensure_dirs()
    download_onet_database()

    occupations, essential_skills, abilities = load_onet_tables()

    target_occupations = filter_target_occupations(occupations)
    direction_mapping = map_occupations_to_directions(target_occupations)

    essential_long = prepare_foundation_table(essential_skills, target_occupations, "Essential Skills")
    abilities_long = prepare_foundation_table(abilities, target_occupations, "Abilities")
    foundation_long = pd.concat([essential_long, abilities_long], ignore_index=True)

    foundation_raw, foundation_scaled = build_foundation_matrix(foundation_long)

    best_k = choose_best_k(foundation_scaled)
    cluster_assignments = run_kmeans(foundation_scaled, best_k)
    pca_df, variance_df = run_pca(foundation_scaled)

    direction_distance = build_direction_distance(foundation_scaled, direction_mapping)
    capability_roles = build_capability_roles(foundation_raw, direction_mapping)

    plot_pca_by_cluster(pca_df, cluster_assignments, variance_df)
    plot_direction_distance_heatmap(direction_distance)
    plot_capability_role_quadrant(capability_roles)

    print("Done.")
    print(f"Best k: {best_k}")
    print(f"Main figures saved to: {FIGURE_DIR}")


if __name__ == "__main__":
    run_pipeline()

```
