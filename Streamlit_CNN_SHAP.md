## 🧠 Streamlit × CNN × SHAP Demo
> 以 **SHAP (SHapley Additive Explanations)** 解釋 CNN 為何做出預測  
> 資料集：CIFAR-10  
> 框架：TensorFlow + Streamlit + SHAP

---

## 🔍 為什麼要用 SHAP？
SHAP 透過 **Shapley 值**（博弈論概念）量化每個特徵（此處為每個像素）對模型預測的影響。  
在影像模型中，SHAP 可視化出 CNN 哪些區域最影響分類結果。

---

## 🧩 範例程式碼

```python
# app_shap.py
import streamlit as st
import tensorflow as tf
from tensorflow.keras import layers, models
from tensorflow.keras.datasets import cifar10
from tensorflow.keras.utils import to_categorical
import numpy as np
import shap
import matplotlib.pyplot as plt
from PIL import Image

st.set_page_config(page_title="CNN SHAP Explainer", layout="wide")
st.title("🧠 CNN × SHAP 解釋性展示 (CIFAR-10)")

# -------------------------------------------------
# 1️⃣ 載入資料
# -------------------------------------------------
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
y_train, y_test = to_categorical(y_train, 10)
class_names = ['airplane', 'automobile', 'bird', 'cat', 'deer',
               'dog', 'frog', 'horse', 'ship', 'truck']

# -------------------------------------------------
# 2️⃣ 模型建立與訓練 (簡化示範)
# -------------------------------------------------
@st.cache_resource
def train_cnn():
    model = models.Sequential([
        layers.Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
        layers.MaxPooling2D((2, 2)),
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.MaxPooling2D((2, 2)),
        layers.Flatten(),
        layers.Dense(64, activation='relu'),
        layers.Dense(10, activation='softmax')
    ])
    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    model.fit(x_train[:10000], y_train[:10000], epochs=2, batch_size=128, verbose=0)
    return model

model = train_cnn()

# -------------------------------------------------
# 3️⃣ 上傳影像或選擇測試樣本
# -------------------------------------------------
st.sidebar.header("📸 選擇輸入影像")
uploaded_file = st.sidebar.file_uploader("上傳圖片 (jpg/png)", type=["jpg", "png"])
random_button = st.sidebar.button("🎲 隨機測試影像")

if uploaded_file:
    image = Image.open(uploaded_file).resize((32, 32))
    img = np.array(image) / 255.0
elif random_button:
    idx = np.random.randint(0, len(x_test))
    img = x_test[idx]
else:
    st.warning("請上傳圖片或點擊隨機測試")
    st.stop()

st.image(img, caption="輸入影像", width=200)
input_data = np.expand_dims(img, axis=0)

# -------------------------------------------------
# 4️⃣ 模型預測
# -------------------------------------------------
pred = model.predict(input_data)
pred_class = np.argmax(pred)
st.markdown(f"### ✅ 模型預測：**{class_names[pred_class]}** (信心：{np.max(pred):.2f})")

# -------------------------------------------------
# 5️⃣ SHAP 解釋模型行為
# -------------------------------------------------
st.subheader("📊 SHAP 模型解釋結果")

background = x_train[:100]
explainer = shap.DeepExplainer(model, background)
shap_values = explainer.shap_values(input_data)

# 繪製 SHAP 解釋圖
fig, axes = plt.subplots(1, 2, figsize=(8, 4))
axes[0].imshow(img)
axes[0].set_title("輸入影像")
axes[0].axis("off")

shap.image_plot(shap_values, -input_data, show=False)
st.pyplot(bbox_inches='tight', pad_inches=0)
