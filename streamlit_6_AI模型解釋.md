---

## 📊 九、實戰案例：AI 模型解釋 Dashboard 全面範例

以下為結合 **Scikit-learn + SHAP + Streamlit** 打造的完整可視化 Dashboard 範例，用於展示模型訓練、推論、與特徵解釋結果。

### 🧱 1️⃣ 專案結構
```
📁 ai_explain_dashboard/
 ├── app.py            ← Streamlit 主應用
 ├── requirements.txt  ← 依賴套件
 └── data/iris.csv     ← 範例資料
```

### 📦 2️⃣ requirements.txt
```text
streamlit
scikit-learn
pandas
matplotlib
shap
numpy
```

### 🧩 3️⃣ app.py 主程式
```python
import streamlit as st
import pandas as pd
import numpy as np
import shap
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.ensemble import RandomForestClassifier

# 🧠 頁面設定
st.set_page_config(page_title="AI 模型解釋 Dashboard", layout="wide")
st.title("📊 AI 模型解釋 Dashboard")
st.markdown("此範例示範如何整合 **Scikit-learn + SHAP** 建立互動式模型分析平台。")

# 📥 載入資料
iris = load_iris()
X = pd.DataFrame(iris.data, columns=iris.feature_names)
y = iris.target
model = RandomForestClassifier(random_state=42)
model.fit(X, y)

# 🎯 側邊欄選單
st.sidebar.header("模型控制")
selected_class = st.sidebar.selectbox("選擇欲分析類別", iris.target_names)
selected_idx = list(iris.target_names).index(selected_class)

# 🧩 SHAP 模型解釋
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X)

# 📊 主區域顯示
col1, col2 = st.columns([2, 1])

with col1:
    st.subheader(f"🔍 {selected_class} 類別的特徵貢獻圖")
    fig1, ax1 = plt.subplots()
    shap.summary_plot(shap_values[selected_idx], X, show=False)
    st.pyplot(fig1)

with col2:
    st.subheader("📈 特徵平均重要性")
    importance = pd.DataFrame({
        'Feature': X.columns,
        'Importance': np.abs(shap_values[selected_idx]).mean(axis=0)
    }).sort_values(by='Importance', ascending=False)
    st.bar_chart(importance.set_index('Feature'))

# 🔬 單筆預測分析
st.subheader("🔬 單筆樣本解釋")
idx = st.slider("選擇樣本索引", 0, len(X)-1, 0)
fig2, ax2 = plt.subplots()
shap.waterfall_plot(shap.Explanation(values=shap_values[selected_idx][idx], base_values=explainer.expected_value[selected_idx], data=X.iloc[idx], feature_names=X.columns))
st.pyplot(fig2)

st.success("✅ 模型解釋完成！可即時調整類別與樣本以觀察特徵影響。")
```

---

### 💡 4️⃣ 儀表板互動功能說明
| 模組區塊 | 功能 |
|-----------|------|
| 側邊欄控制區 | 選擇欲分析的類別與樣本索引 |
| 特徵貢獻圖 | 顯示全局特徵對分類結果的影響程度（SHAP Summary Plot） |
| 特徵平均重要性 | 以長條圖呈現全局平均重要性排名 |
| 單筆樣本解釋 | 顯示該樣本的特徵貢獻瀑布圖（Waterfall Plot） |

---

### ⚙️ 5️⃣ 執行應用
```bash
streamlit run app.py
```
瀏覽器預設開啟網址：[http://localhost:8501](http://localhost:8501)

---

### 🧠 6️⃣ 延伸應用構想
- 整合 **LIME** 與 **SHAP** 切換分析模式。
- 新增 **模型訓練區塊**（使用者可上傳資料重新訓練）。
- 與 **FastAPI** 結合，實現即時預測服務。
- 搭配 **Plotly / ECharts** 提升視覺互動效果。

---

## 🎯 十、應用與延伸

| 領域 | 範例應用 |
|------|-----------|
| 機器學習 | 模型推論與可解釋性展示 |
| 資料分析 | 可互動式資料儀表板 |
| 負責任 AI | 模型透明度與信任度分析平台 |
| 教學研究 | 模型可視化與解釋教學工具 |

---

## 📚 十一、延伸參考

- 📘 官方網站：[https://streamlit.io](https://streamlit.io)
- 💻 文件中心：[https://docs.streamlit.io](https://docs.streamlit.io)
- 🧠 範例倉庫：[https://github.com/streamlit/examples](https://github.com/streamlit/examples)
- 🔍 SHAP 文件：[https://shap.readthedocs.io](https://shap.readthedocs.io)
- 🔬 LIME 文件：[https://github.com/marcotcr/lime](https://github.com/marcotcr/lime)

---

> ✅ 提示：此 Dashboard 可作為「Explainable AI 儀表板」模板，結合 SHAP、LIME、與即時推論 API，打造一個完整的 AI 透明度分析平台。

