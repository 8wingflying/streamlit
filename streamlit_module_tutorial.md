# 📘 Streamlit 模組教學（進階版）

---

## 🧩 一、Streamlit 模組概述

`streamlit` 是 Python 的一個開源模組，用於快速建立**互動式資料分析與 AI 視覺化應用**。其核心理念為：**Python 即介面（Python-as-UI）**，讓開發者不需撰寫 HTML、CSS 或 JavaScript，就能直接以 Python 建構網頁介面。

---

## 🧠 二、模組結構與子模組功能

| 子模組 | 功能說明 |
|--------|----------|
| `streamlit.runtime` | 控制應用執行流程與快取管理。 |
| `streamlit.elements` | 管理前端顯示元件（按鈕、圖表、文字等）。 |
| `streamlit.delta_generator` | 動態更新前端內容。 |
| `streamlit.state` | 管理使用者互動後的狀態資料。 |
| `streamlit.cache_data` / `cache_resource` | 資料與模型快取功能。 |
| `streamlit.session_state` | 儲存使用者狀態。 |
| `streamlit.config` | 應用設定（主題、埠號、開發模式等）。 |
| `streamlit.logger` | 記錄日誌與錯誤。 |
| `streamlit.components.v1` | 支援自訂 HTML / JS 元件嵌入。 |
| `streamlit.web` | 負責 WebSocket 通訊。 |

---

## ✏️ 三、核心功能模組與範例

### 1️⃣ 輸出與顯示元件
| 函數 | 功能 | 範例 |
|------|------|------|
| `st.title()` | 顯示主標題 | `st.title("AI Dashboard")` |
| `st.header()` | 顯示段落標題 | `st.header("資料分析結果")` |
| `st.markdown()` | 顯示 Markdown 文字 | `st.markdown("**粗體文字**")` |
| `st.latex()` | 顯示數學公式 | `st.latex("E=mc^2")` |

### 2️⃣ 互動式輸入元件
| 函數 | 功能 | 範例 |
|------|------|------|
| `st.button()` | 按鈕 | `if st.button("分析開始"):` |
| `st.text_input()` | 文字輸入 | `name = st.text_input("請輸入姓名")` |
| `st.slider()` | 數值滑桿 | `num = st.slider("選擇數字", 0, 100)` |
| `st.file_uploader()` | 檔案上傳 | `file = st.file_uploader("上傳 CSV 檔")` |
| `st.selectbox()` | 下拉選單 | `opt = st.selectbox("選擇項目", ["A","B","C"])` |

### 3️⃣ 資料視覺化元件
| 函數 | 圖表類型 | 範例 |
|------|------------|------|
| `st.line_chart()` | 折線圖 | `st.line_chart(df)` |
| `st.bar_chart()` | 長條圖 | `st.bar_chart(df)` |
| `st.area_chart()` | 區域圖 | `st.area_chart(df)` |
| `st.map()` | 地圖 | `st.map(df)` |
| `st.pyplot()` | Matplotlib | `st.pyplot(fig)` |
| `st.plotly_chart()` | Plotly | `st.plotly_chart(fig)` |

---

## 🧱 四、進階章節：自訂組件開發與部署

Streamlit 不僅可使用內建元件，也能整合 **自訂 HTML / JavaScript 元件**，打造獨特的互動式介面。

### 🔹 1. 匯入自訂組件模組
```python
import streamlit.components.v1 as components
```

### 🔹 2. 直接嵌入 HTML / JS
```python
components.html(
    """
    <div style='background-color:#222;color:white;padding:20px;border-radius:10px;'>
        <h2>🌐 自訂 HTML 元件示範</h2>
        <p>這是一個自訂的前端內容，可嵌入 JS、CSS、Chart.js、D3.js 等視覺化庫。</p>
    </div>
    """,
    height=200,
)
```

### 🔹 3. 建立獨立組件專案
若要開發可重複使用的前端組件，可使用：
```bash
streamlit hello
```
瀏覽官方範例後，使用以下命令產生組件模板：
```bash
streamlit component create my_component
```
結構如下：
```
my_component/
 ├── frontend/ （React or JS）
 ├── __init__.py
 ├── my_component.py
 └── package.json
```
可透過 `npm run build` 建立前端，並由 Python 模組讀取。

---

## ☁️ 五、部署方法總覽

| 平台 | 說明 |
|------|------|
| **Streamlit Cloud** | 官方雲端部署服務，直接從 GitHub 專案部署。 |
| **Hugging Face Spaces** | 支援 Streamlit、Gradio 應用的免費部署平台。 |
| **Docker 部署** | 使用 `Dockerfile` 建立映像並上傳至雲端。 |

### 📦 Docker 部署範例
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8501
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```
執行：
```bash
docker build -t streamlit-app .
docker run -p 8501:8501 streamlit-app
```

---

## 🎯 六、實務應用與延伸

| 領域 | 範例應用 |
|------|-----------|
| 資料分析 | 動態報表、EDA 可視化面板 |
| 機器學習 | 模型預測結果展示與比較平台 |
| 資安分析 | 威脅狩獵視覺化儀表板（Threat Hunting Dashboard） |
| 教育培訓 | AI 教學互動教材平台 |
| API 服務 | 與 FastAPI / Flask 結合的 AI Web 應用 |

---

## 🧩 七、延伸參考

- 📘 官方網站：[https://streamlit.io](https://streamlit.io)
- 💻 文件中心：[https://docs.streamlit.io](https://docs.streamlit.io)
- 🧠 範例倉庫：[https://github.com/streamlit/examples](https://github.com/streamlit/examples)

---

> ✅ 建議：若要整合更豐富的前端功能（例如互動圖表或即時更新），可搭配 **Plotly、Altair、ECharts、或 React-based 前端模組** 使用。

