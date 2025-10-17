## 延伸閱讀
- 初探 Langchain 與 LLM：打造簡易問診機器人
## 🧠 一、streamlit

**Streamlit** 是 Python 的開源框架，用於快速建立資料與機器學習應用程式。  
它的宗旨是讓開發者「用最少的程式碼、最快速地」把資料科學專案轉成可互動的網頁應用。

---

- 範例 https://streamlit.io/gallery
- 第三方套件 Components  https://streamlit.io/components
- [Build powerful generative AI apps](https://streamlit.io/generative-ai)
- Playground  https://streamlit.io/playground
- API reference  https://docs.streamlit.io/develop/api-reference

---

### ✨ 特點 (Key Features)

- 🧩 僅需 Python，無需 HTML、CSS、JS
- ⚙️ 自動更新頁面，所見即所得
- 🧠 可結合機器學習模型（Scikit-learn、TensorFlow、PyTorch、Hugging Face）
- 🚀 支援雲端部署（Streamlit Cloud / Docker / AWS）

---

## ⚙️ 二、安裝與啟動 (Installation & Run)

```bash
# 安裝 Streamlit
pip install streamlit

# 啟動應用
streamlit run app.py
```

執行 `streamlit run app.py` 後，瀏覽器會自動開啟應用頁面（預設埠為 `localhost:8501`）。

---

## 🧩 三、核心功能與常用指令 (Core Components)

| 模組 | 功能說明 | 範例 |
|------|-----------|------|
| `st.title()` | 顯示標題 | `st.title("AI 儀表板")` |
| `st.write()` | 顯示文字、DataFrame、圖片 | `st.write(df)` |
| `st.text_input()` | 文字輸入 | `name = st.text_input("請輸入姓名")` |
| `st.slider()` | 數值滑桿 | `age = st.slider("選擇年齡", 0, 100, 25)` |
| `st.selectbox()` | 下拉選單 | `opt = st.selectbox("選擇模型", ["SVM", "LSTM"])` |
| `st.button()` | 建立按鈕 | `if st.button("執行"): run()` |
| `st.sidebar` | 側邊欄設計 | `st.sidebar.slider("設定參數")` |
| `st.line_chart()` | 折線圖 | `st.line_chart(data)` |
| `st.bar_chart()` | 長條圖 | `st.bar_chart(data)` |
| `st.file_uploader()` | 上傳檔案 | `file = st.file_uploader("上傳 CSV")` |
| `st.map()` | 地圖視覺化 | `st.map(df)` |

---

## 📊 四、資料視覺化範例 (Data Visualization Example)

```python
import streamlit as st
import pandas as pd
import numpy as np

st.title("📊 資料視覺化範例 (Visualization Example)")

df = pd.DataFrame({
    "x": np.arange(1, 101),
    "y": np.random.randn(100).cumsum()
})

chart_type = st.radio("選擇圖表類型", ["折線圖", "長條圖", "散點圖"])

if chart_type == "折線圖":
    st.line_chart(df, x="x", y="y")
elif chart_type == "長條圖":
    st.bar_chart(df, x="x", y="y")
else:
    st.scatter_chart(df, x="x", y="y")

st.success("✅ 圖表生成完成！")
```

---

## 🧭 五、互動式控制元件 (Interactive Widgets)

```python
st.header("🎮 互動控制範例 (Interactive Example)")

name = st.text_input("請輸入您的名字")
age = st.slider("選擇年齡", 1, 100, 25)
agree = st.checkbox("我同意條款")

if st.button("送出"):
    st.write(f"👋 你好 {name}！你的年齡是 {age} 歲。")
    if agree:
        st.success("感謝您的同意！")
```

---

## 🤖 六、與機器學習模型整合 (Integration with ML Models)

本章節展示如何使用 Streamlit 結合 **Scikit-learn** 與 **Hugging Face Transformers**，打造互動式 AI 推論應用。

---

### 🔹 6.1 Scikit-learn 範例：Iris 花分類互動式預測 (Iris Classifier Demo)

```python
import streamlit as st
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier

st.title("🌼 Iris 花分類模型 (Scikit-learn Demo)")

iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)
y = iris.target

model = RandomForestClassifier()
model.fit(X, y)

st.sidebar.header("請輸入花朵特徵")
sepal_length = st.sidebar.slider("Sepal length (cm)", 4.0, 8.0, 5.1)
sepal_width  = st.sidebar.slider("Sepal width (cm)", 2.0, 5.0, 3.5)
petal_length = st.sidebar.slider("Petal length (cm)", 1.0, 7.0, 1.4)
petal_width  = st.sidebar.slider("Petal width (cm)", 0.1, 2.5, 0.2)

input_data = [[sepal_length, sepal_width, petal_length, petal_width]]
prediction = model.predict(input_data)
pred_class = iris.target_names[prediction[0]]

st.subheader("🌸 預測結果 (Prediction Result)")
st.write(f"模型預測為： **{pred_class}**")
```

---

### 🔹 6.2 Hugging Face Transformers 範例：文字情感分析 (Sentiment Analysis)

```python
import streamlit as st
from transformers import pipeline

st.title("💬 文字情感分析 (Hugging Face Demo)")

sentiment_pipeline = pipeline("sentiment-analysis")
text = st.text_area("請輸入文字：", "這部電影非常好看！")

if st.button("分析情感"):
    result = sentiment_pipeline(text)[0]
    st.write(f"🔍 標籤：**{result['label']}**")
    st.write(f"📈 信心分數：**{result['score']:.2f}**")
```

---

### 🔹 6.3 AI 模型可視化與解釋範例 (Model Explainability with SHAP)

```python
import streamlit as st
import pandas as pd
import shap
import matplotlib.pyplot as plt
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

st.title("📈 模型可解釋性分析 (SHAP + Streamlit)")

iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)
y = iris.target

model = RandomForestClassifier()
model.fit(X, y)

explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X)

fig, ax = plt.subplots()
shap.summary_plot(shap_values, X, plot_type="bar", show=False)
st.pyplot(fig)
```

---

## Streamlit LaTeX 範例

```python
import streamlit as st

st.title("🧮 Streamlit LaTeX 範例展示 (LaTeX Examples)")

st.header("1️⃣ 基本公式")
st.latex(r"E = mc^2")
st.latex(r"a^2 + b^2 = c^2")

st.header("2️⃣ 分數與開根號")
st.latex(r"\frac{a}{b} = \frac{1}{2}")
st.latex(r"x = \sqrt{y}")

st.header("3️⃣ 微分與積分表示式")
st.latex(r"\frac{d}{dx} e^x = e^x")
st.latex(r"\int_0^\infty e^{-x^2} dx = \frac{\sqrt{\pi}}{2}")

st.header("4️⃣ 向量與矩陣")
st.latex(r"\vec{v} = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}")
st.latex(r"A = \begin{pmatrix} 1 & 2 \\ 3 & 4 \end{pmatrix}")

st.header("5️⃣ 機器學習常見公式")
st.latex(r"\hat{y} = \sigma(Wx + b)")
st.latex(r"L = -\sum_{i=1}^{n} y_i \log(\hat{y_i})")

st.header("6️⃣ 內嵌於文字")
st.write("線性迴歸模型可表示為：", r"$y = wx + b$")
st.markdown("邏輯迴歸的 Sigmoid 函數：$\\sigma(x) = \\frac{1}{1 + e^{-x}}$")

```

#### **進階應用：LaTeX 與互動控制元件整合
- 你甚至可以用互動式 Slider 來顯示動態公式，例如：
```python
import streamlit as st
import math

st.title("📈 動態 LaTeX 範例 (Interactive LaTeX Example)")

x = st.slider("選擇 x 值", 0, 10, 5)
st.latex(fr"y = x^2 = {x}^2 = {x**2}")
```

這樣使用者拖動滑桿時，公式會即時更新。


## ☁️ 七、部署與分享 (Deployment & Sharing)

| 部署方式 | 指令 / 說明 |
|-----------|-------------|
| **本地端執行** | `streamlit run app.py` |
| **Streamlit Cloud** | [https://streamlit.io/cloud](https://streamlit.io/cloud) 上傳 GitHub 專案 |
| **Docker** | 建立 Dockerfile 並執行 `docker build` |
| **雲端平台** | 可於 AWS、GCP、Azure 託管 |

---
