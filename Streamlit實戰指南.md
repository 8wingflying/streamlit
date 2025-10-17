## [Streamlit 實戰指南 : 使用 Python 創建交互式數據應用 
- https://github.com/tylerjrichards/Streamlit-for-Data-Science/tree/main
- https://www.tenlong.com.tw/products/9787121484520?list_name=srh
- https://learning.oreilly.com/library/view/streamlit-for-data/9781803248226/
- https://github.com/dataprofessor/streamlit-for-datascience
- https://github.com/tylerjrichards/Getting-Started-with-Streamlit-for-Data-Science

##
```python

```


##
```python
import openai
import streamlit as st
from transformers import pipeline

st.title("Hugging Face Demo")
text = st.text_input("Enter text to analyze")


@st.cache_resource()
def get_model():
    return pipeline("sentiment-analysis")


model = get_model()
if text:
    result = model(text)
    st.write("Sentiment:", result[0]["label"])
    st.write("Confidence:", result[0]["score"])

st.title("OpenAI Version")


openai.api_key = st.secrets["OPENAI_API_KEY"]

system_message_default = """You are a helpful sentiment analysis assistant. You always respond with the sentiment of the text you are given and the confidence of your sentiment analysis with a number between 0 and 1"""

system_message = st.text_area(
    "Enter a System Message to instruct OpenAI", system_message_default
)
analyze_button = st.button("Analyze Text")

if analyze_button:
    messages = [
        {
            "role": "system",
            "content": f"{system_message}",
        },
        {
            "role": "user",
            "content": f"Sentiment analysis of the following text: {text}",
        },
    ]
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
    )
    sentiment = response.choices[0].message["content"].strip()
    st.write(sentiment)
```

##
```python

```

##
```python

```

##
```python

```

##
```python

```

##
```python

```

