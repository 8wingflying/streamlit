# 🇹🇼 TAIWAN-LLM 教學文件 (中英對照版)

---

## 🧠 概述 | Overview

**TAIWAN-LLM** 是由國立臺灣大學開發的第一個針對 **台灣繁體中文語言與文化** 微調的大型語言模型 (Large Language Model, LLM)。  
它旨在解決目前主流 LLM（如 GPT-4、Claude 2）偏向英文與簡體中文語料，導致模型對台灣語境理解不足的問題。

> **TAIWAN-LLM** bridges linguistic and cultural gaps by fine-tuning on Traditional Chinese used in Taiwan, making it both linguistically precise and culturally aligned.

---

## 📚 研究動機 | Motivation

| 中文敘述 | English Description |
|------------|------------------------|
| 現有 LLM 多以英文與簡體中文為主，對繁體中文理解不足。 | Most LLMs are trained on English and Simplified Chinese corpora, lacking proficiency in Traditional Chinese. |
| 臺灣繁體中文具獨特語彙與文化背景。 | Taiwan’s Traditional Chinese carries distinct vocabulary and cultural nuances. |
| 缺乏在地化語言模型造成「語言數位落差」。 | This linguistic gap leads to a digital divide for Traditional Chinese users. |

---

## ⚙️ 三階段微調架構 | Three-Stage Fine-Tuning Pipeline

```mermaid
flowchart LR
A[📖 Continue-Pretraining (cPT)] --> B[🗣️ Supervised Fine-Tuning (SFT)]
B --> C[💬 Feedback Supervised Fine-Tuning (Feedback SFT)]
C --> D[🚀 Evaluation & Deployment]
```

### 1️⃣ Continue-Pretraining (cPT)
- 在 LLaMA 2 基礎上，持續於 **3,510 億字詞 (35.1B tokens)** 的台灣語料中再訓練。
- 語料分布：
  | 類別 | 文件數 | 詞元數 (億) | 佔比 |
  |------|----------|-------------|------|
  | 社群媒體 | 824 萬 | 1660 | 47% |
  | 新聞 | 860 萬 | 1040 | 30% |
  | 知識庫 | 319 萬 | 570 | 16% |
  | 書籍 | 4000 | 240 | 7% |

### 2️⃣ Supervised Fine-Tuning (SFT)
- 使用多輪對話資料集（Alpaca、Flan V2、OpenAssistant 等）+
  **Taiwan Instruction Dataset（人工撰寫在地語境對話）**。

### 3️⃣ Feedback Supervised Fine-Tuning (Feedback SFT)
- 透過 [twllm.com](https://twllm.com) 收集 **20,000 筆使用者互動回饋**。
- 僅使用「正向評價」樣本再次微調，以貼近使用者文化偏好。

---

## 🧪 實驗設計 | Experimental Setup

| 項目 | 說明 |
|------|------|
| 基礎模型 | LLaMA 2 (7B / 13B) |
| 訓練環境 | 48 × H100 GPU, bfloat16, DeepSpeed ZeRO-2, FlashAttention-2 |
| 優化器 | AdamW (無 weight decay) |
| 評測平台 | **TC-Eval Benchmark Suite** |
| 任務類型 | 問答、摘要、分類、表格理解、文化知識測試 |

---

## 📈 實驗結果 | Results Summary

| 模型 | 平均分數 | 備註 |
|------|-----------|------|
| **TAIWAN-LLM (13B)** | **53.99%** | ≈ GPT-3.5 Turbo |
| GPT-4 | 58.03% | 商業封閉模型最高 |
| Claude 2.1 | 58.49% | 文化理解強 |
| LLaMA 2 13B Chat | 34.06% | 明顯落後 |

**關鍵觀察 | Key Insights**  
- 移除 cPT → 明顯退步（DRCD 準確率由 87.6% → 75.8%）。  
- Feedback SFT 效果較小，但提升文化適應度。  
- 加入低品質 Web 資料（CommonCrawl）反而造成性能下降。

> "Data quality outweighs quantity — high-quality Taiwanese corpora are essential."

---

## 💬 文化理解測試 | Cultural Understanding Examples

| 問題 | ChatGPT 回答 | TAIWAN-LLM 回答 |
|------|---------------|----------------|
| **NTU 在哪裡？** | 南洋理工大學（新加坡） | 國立臺灣大學（台北市） |
| **什麼是 22K？** | 金的純度等級 | 大學生起薪 22,000 元代稱 |
| **載具是什麼？** | 購物袋或容器 | 電子發票載具（手機 / APP） |

✅ 結論：TAIWAN-LLM 在「文化語境對齊 (Cultural Alignment)」方面顯著優於主流模型。

---

## 🏁 結論 | Conclusion

- **TAIWAN-LLM** 是全球首個針對台灣繁體中文文化微調的開源大型語言模型。
- 三階段訓練流程（cPT → SFT → Feedback SFT）有效結合語言與文化層面。
- 模型效能可與 GPT-3.5 Turbo 媲美，且具備更強的在地文化理解力。
- 完全開源，鼓勵社群共同開發，推動台灣 AI 語言主權。

> **"Bridging the linguistic divide — empowering Traditional Chinese AI for Taiwan."**

---

## 🔭 未來方向 | Future Directions

- 導入 **RLHF (Reinforcement Learning with Human Feedback)** 與 **DPO (Direct Preference Optimization)**。  
- 擴充多語混合（台語、客語、原民語）。  
- 強化知識檢索與持續學習能力。

---

## 📎 參考來源 | References
- Lin, Yen-Ting & Chen, Yun-Nung (2023). *TAIWAN-LLM: Bridging the Linguistic Divide with a Culturally Aligned Language Model.* arXiv:2311.17487v1  
- [https://twllm.com](https://twllm.com)  
- [https://github.com/MiuLab/Taiwan-LLaMa](https://github.com/MiuLab/Taiwan-LLaMa)

---

📘 **作者註記**：此文件可作為教學教材、LLM 微調入門指引，或文化對齊研究案例。

