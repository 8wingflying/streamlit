# 🤖 Transformers Pipeline vs 🧠 Scikit-Learn Pipeline 衝突與解法教學

## 📘 一、問題說明
在 Python 中同時使用：
```python
from transformers import pipeline
from sklearn.pipeline import Pipeline
```
若不注意命名空間管理，容易出現 **命名衝突（Name Shadowing）**，導致：
```python
TypeError: 'Pipeline' object is not callable
```

---

## 🧩 二、衝突原因
在同一支程式中：
- `pipeline`（小寫）屬於 Hugging Face Transformers。
- `Pipeline`（大寫）屬於 Scikit-Learn。
- 若誤將 `pipeline` 當變數名稱使用，就會覆蓋掉原本的函式。

### ⚠️ 錯誤範例
```python
from transformers import pipeline
from sklearn.pipeline import Pipeline

# ❌ 覆蓋了 pipeline 函式
pipeline = Pipeline([('scaler', None)])

# 下面這行會報錯
nlp = pipeline("sentiment-analysis")  # TypeError
```

---

## ✅ 三、解法一：使用別名（最推薦）
給兩者不同名稱以避免混淆。
```python
from transformers import pipeline as hf_pipeline
from sklearn.pipeline import Pipeline as sk_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

# 使用 transformers
nlp = hf_pipeline("sentiment-analysis")
print(nlp("I love AI!"))

# 使用 scikit-learn
clf = sk_pipeline([
    ('scaler', StandardScaler()),
    ('model', LogisticRegression())
])
```

---

## ✅ 四、解法二：延遲匯入（較少使用）
可在不同區段再載入需要的 pipeline。
```python
from transformers import pipeline
nlp = pipeline("translation", model="Helsinki-NLP/opus-mt-en-zh")
print(nlp("AI will change the world."))

# 用完再導入 sklearn pipeline
del pipeline
from sklearn.pipeline import Pipeline
```
> ⚠️ 此方法不建議，程式可讀性與可維護性差。

---

## ✅ 五、解法三：使用完整模組名稱
直接使用完整模組名稱呼叫。
```python
import transformers
import sklearn.pipeline

# 呼叫 transformers pipeline
nlp = transformers.pipeline("summarization")

# 呼叫 sklearn pipeline
ml_pipeline = sklearn.pipeline.Pipeline([('scaler', None)])
```

---

## 🧭 六、最佳實務建議

| 情境 | 建議作法 |
|------|-----------|
| 同一程式需同時使用 Transformers 與 Scikit-Learn | 使用別名（`hf_pipeline` / `sk_pipeline`） |
| 僅使用其中一者 | 可直接 import，但避免重複命名變數 |
| 在多人專案或 Notebook 中混合使用 | 明確指定模組路徑或使用別名 |

---

## 🧠 七、安全共存範例
```python
# 同時使用 Hugging Face 與 scikit-learn
from transformers import pipeline as hf_pipeline
from sklearn.pipeline import Pipeline as sk_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

# === Transformers: NLP 任務 ===
nlp = hf_pipeline("sentiment-analysis")
print(nlp("Transformers and Scikit-learn work great together!"))

# === Scikit-learn: 傳統 ML 任務 ===
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)
model = sk_pipeline([
    ('scaler', StandardScaler()),
    ('clf', LogisticRegression())
])
model.fit(X_train, y_train)
print("Accuracy:", model.score(X_test, y_test))
```

---

## 📊 八、總結比較表

| 項目 | 🤗 Transformers `pipeline` | 🧠 Scikit-Learn `Pipeline` |
|------|-----------------------------|-----------------------------|
| 套件 | transformers | scikit-learn |
| 用途 | NLP / CV / Audio 預訓練任務推論 | 結構化資料訓練流程串接 |
| 是否需訓練 | 否（使用預訓練模型） | 是（需 `.fit()` 訓練） |
| 支援 GPU | ✅ 是 | ❌ 否 |
| 模型儲存 | `.save_pretrained()` | `.joblib.dump()` |
| 適用資料型態 | 文字、影像、音訊 | 表格、向量 |
| 常見任務 | 翻譯、摘要、分類、NER | 標準化 + 模型訓練 |

---

## 🧾 九、總結
- 兩者名稱相似，但用途完全不同。  
- 同時使用時，**使用別名或完整模組名稱最安全。**  
- 在專案開發或 Notebook 教學中，建議使用：
  ```python
  from transformers import pipeline as hf_pipeline
  from sklearn.pipeline import Pipeline as sk_pipeline
  ```

---

📘 作者：T Ben × ChatGPT GPT-5  
📅 版本：2025-10-21  
🧭 主題：Transformers Pipeline vs Scikit-Learn Pipeline 衝突與解法