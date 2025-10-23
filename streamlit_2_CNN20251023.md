## 📘 Streamlit × CNN Advanced Demo 
此範例展示如何在 **Streamlit** 中：
- 訓練 CNN 模型（TensorFlow / Keras）
- 動態顯示訓練過程曲線（Accuracy / Loss）
- 即時預測上傳圖片類別
- 顯示模型預測信心分布

---

- pip install streamlit tensorflow pillow matplotlib
- 程式碼
```python

import streamlit as st
import tensorflow as tf
from tensorflow.keras import layers, models
from tensorflow.keras.datasets import cifar10
from tensorflow.keras.utils import to_categorical
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import time

st.set_page_config(page_title="CNN Advanced Classifier", layout="wide")

st.title("🧠 Streamlit × CNN Advanced Demo")
st.markdown("**功能：** 顯示訓練進度條 + Accuracy/Loss 曲線 + 即時影像分類")

# -------------------------------------------------
# 1️⃣ 資料載入
# -------------------------------------------------
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
y_train, y_test = to_categorical(y_train, 10), to_categorical(y_test, 10)
class_names = ['airplane', 'automobile', 'bird', 'cat', 'deer',
               'dog', 'frog', 'horse', 'ship', 'truck']

# -------------------------------------------------
# 2️⃣ 模型建立
# -------------------------------------------------
def build_model():
    model = models.Sequential([
        layers.Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
        layers.MaxPooling2D((2, 2)),
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.MaxPooling2D((2, 2)),
        layers.Conv2D(128, (3, 3), activation='relu'),
        layers.Flatten(),
        layers.Dense(128, activation='relu'),
        layers.Dropout(0.3),
        layers.Dense(10, activation='softmax')
    ])
    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    return model

# -------------------------------------------------
# 3️⃣ 模型訓練（含進度條與曲線）
# -------------------------------------------------
st.sidebar.header("⚙️ 模型訓練設定")
epochs = st.sidebar.slider("選擇訓練 Epoch 數", 1, 10, 3)
train_button = st.sidebar.button("🚀 開始訓練模型")

if train_button:
    model = build_model()
    progress = st.progress(0)
    status_text = st.empty()

    history_data = {'loss': [], 'accuracy': [], 'val_loss': [], 'val_accuracy': []}

    for epoch in range(epochs):
        hist = model.fit(x_train, y_train,
                         epochs=1,
                         batch_size=256,
                         validation_data=(x_test, y_test),
                         verbose=0)
        # 更新曲線資料
        for key in history_data.keys():
            history_data[key].append(hist.history[key][0])

        progress.progress((epoch + 1) / epochs)
        status_text.text(f"訓練中... Epoch {epoch + 1}/{epochs}")
        time.sleep(0.5)

    progress.empty()
    status_text.text("✅ 訓練完成！")

    # 顯示訓練結果曲線
    col1, col2 = st.columns(2)
    with col1:
        fig1, ax1 = plt.subplots()
        ax1.plot(history_data['accuracy'], label='Train Accuracy')
        ax1.plot(history_data['val_accuracy'], label='Validation Accuracy')
        ax1.set_title("📈 模型準確率 (Accuracy)")
        ax1.legend()
        st.pyplot(fig1)

    with col2:
        fig2, ax2 = plt.subplots()
        ax2.plot(history_data['loss'], label='Train Loss')
        ax2.plot(history_data['val_loss'], label='Validation Loss')
        ax2.set_title("📉 模型損失 (Loss)")
        ax2.legend()
        st.pyplot(fig2)

    st.session_state['model'] = model
else:
    st.info("請點擊左側『🚀 開始訓練模型』以啟動訓練")

# -------------------------------------------------
# 4️⃣ 圖片上傳與預測
# -------------------------------------------------
if 'model' in st.session_state:
    st.header("📸 圖片分類預測")
    uploaded_file = st.file_uploader("上傳圖片 (jpg/png)", type=["jpg", "png"])
    if uploaded_file:
        image = Image.open(uploaded_file).resize((32, 32))
        img_array = np.expand_dims(np.array(image) / 255.0, axis=0)
        pred = st.session_state['model'].predict(img_array)
        pred_class = np.argmax(pred)
        confidence = np.max(pred)

        col1, col2 = st.columns([1, 2])
        with col1:
            st.image(image, caption="📤 上傳影像", use_column_width=True)
        with col2:
            st.markdown(f"### ✅ 預測結果：**{class_names[pred_class]}**")
            st.markdown(f"信心分數：`{confidence:.2f}`")

            fig, ax = plt.subplots(figsize=(5, 3))
            ax.barh(class_names, pred[0], color='lightgreen')
            ax.set_title("各類別預測信心分布")
            st.pyplot(fig)

```
