# 📘 Streamlit 概述

**Streamlit** 是一個開源的 Python 框架，主要用於快速建立互動式的資料分析與機器學習應用網頁。它讓開發者能用簡潔的 Python 程式碼，將資料、模型與視覺化即時呈現在網頁介面上，不需撰寫 HTML、CSS 或 JavaScript。

---

## 🧬 一、主要特點

| 特點 | 說明 |
|------|------|
| 🚀 快速開發 | 使用純 Python 撰寫，不需額外的前端知識即可建立互動網頁。 |
| 🧠 與 AI/ML 整合性強 | 常與 Pandas、Matplotlib、Plotly、Scikit-learn、TensorFlow 等庫搭配使用。 |
| 🎛️ 即時互動 | 提供多種互動元件（按鈕、滑標、文字輸入、選單等），方便用戶操作。 |
| 🌐 自動網頁化 | 只需執行 `streamlit run app.py` 即可自動開啟本地網頁伺服器。 |
| 🧱 部署簡單 | 可部署到 Streamlit Cloud、Hugging Face Spaces 或自行架設伺服器。 |

---

## 🧮 二、基本架構與範例程式

### 🔹 1. 安裝
```bash
pip install streamlit
```

### 🔹 2. 建立範例應用程式 `app.py`
```python
import streamlit as st
import pandas as pd
import numpy as np

# 標題
st.title("📊 Streamlit 入門範例")

# 子標題
st.subheader("互動式資料展示")

# 建立假資料
data = pd.DataFrame(
    np.random.randn(20, 3),
    columns=['A', 'B', 'C']
)

# 顯示表格
st.dataframe(data)

# 畫折線圖
st.line_chart(data)

# 互動元件
number = st.slider("選擇顯示的列數", 1, 20, 5)
st.write("你選擇顯示前", number, "筆資料")
st.dataframe(data.head(number))
```

### 🔹 3. 執行應用
```bash
streamlit run app.py
```

會自動開啟瀏覽器（預設網址為 `http://localhost:8501`）。

---

## 🎨 三、常用互動元件

| 元件 | 函數 | 範例 |
|------|------|------|
| 文字輸入 | `st.text_input()` | `name = st.text_input("請輸入姓名")` |
| 數字輸入 | `st.number_input()` | `age = st.number_input("請輸入年齡", 0, 100)` |
| 滑標 | `st.slider()` | `value = st.slider("選擇數值", 0, 10)` |
| 選單 | `st.selectbox()` | `option = st.selectbox("選擇項目", ["A", "B", "C"])` |
| 按鈕 | `st.button()` | `if st.button("按我"): st.write("你按了按鈕")` |
| 檔案上傳 | `st.file_uploader()` | `uploaded = st.file_uploader("上傳CSV", type="csv")` |

---

## 📊 四、資料視覺化整合

Streamlit 支援多種圖表工具：

| 圖表工具 | 函式 | 範例 |
|-----------|------|------|
| Matplotlib | `st.pyplot(fig)` | `st.pyplot(plt.figure())` |
| Plotly | `st.plotly_chart(fig)` | `st.plotly_chart(px.line(data))` |
| Altair | `st.altair_chart(chart)` | `st.altair_chart(alt.Chart(df))` |
| Map 地圖 | `st.map()` | `st.map(df_with_lat_lon)` |

---

## 🤖 五、機器學習範例整合

```python
import streamlit as st
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier

st.title("🌸 Iris 鳶尾花分類示範")

# 載入資料
iris = load_iris()
X = iris.data
y = iris.target

# 建立模型
model = RandomForestClassifier()
model.fit(X, y)

# 使用者輸入
sepal_length = st.slider("花萼長", 4.0, 8.0, 5.0)
sepal_width  = st.slider("花萼寬", 2.0, 4.5, 3.5)
petal_length = st.slider("花瓣長", 1.0, 7.0, 4.0)
petal_width  = st.slider("花瓣寬", 0.1, 2.5, 1.0)

# 預測
prediction = model.predict([[sepal_length, sepal_width, petal_length, petal_width]])
st.write("預測結果：", iris.target_names[prediction][0])
```

---

## 🛠️ 六、部署方式

| 部署平台 | 說明 |
|-----------|------|
| **Streamlit Cloud** | 官方雲端服務，免費註冊即可上傳 GitHub 專案並自動部署。 |
| **Hugging Face Spaces** | 支援 Streamlit、Gradio 等前端框架的免費部署平台。 |
| **自架伺服器** | 可透過 `Docker` 或 `gunicorn` + `nginx` 部署於本地或雲端環境。 |

---

## 📚 七、常見應用場景

- 📊 **資料探索與視覺化**
- 🤖 **AI 模型互動展示**
- 🧾 **報表與商業儀表板**
- 🧠 **教育與教學示範平台**
- 🔍 **Threat Hunting / 資安可視化工具（結合 YARA、Sigma）**

---

