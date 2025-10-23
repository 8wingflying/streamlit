## [Streamlit 實戰指南 : 使用 Python 創建交互式數據應用 
- https://github.com/tylerjrichards/Streamlit-for-Data-Science/tree/main
- https://www.tenlong.com.tw/products/9787121484520?list_name=srh
- https://learning.oreilly.com/library/view/streamlit-for-data/9781803248226/
- https://github.com/dataprofessor/streamlit-for-datascience
- https://github.com/tylerjrichards/Getting-Started-with-Streamlit-for-Data-Science

## Chapter 1 |plot_demo.py
- streamlit run plot_demo.py
```python
import time

import numpy as np
import streamlit as st

progress_bar = st.sidebar.progress(0)
status_text = st.sidebar.empty()
last_rows = np.random.randn(1, 1)
chart = st.line_chart(last_rows)

for i in range(1, 101):
    new_rows = last_rows[-1, :] + np.random.randn(50, 1).cumsum(axis=0)
    status_text.text("%i%% Complete" % i)
    chart.add_rows(new_rows)
    progress_bar.progress(i)
    last_rows = new_rows
    time.sleep(0.05)

progress_bar.empty()

# Streamlit widgets automatically run the script from top to bottom. Since
# this button is not connected to any other logic, it just causes a plain
# rerun.
st.button("Re-run")
```

## Chapter 1  |plotting_app/tmp.py
```python
import numpy as np
import pandas as pd
import streamlit as st

st.title("MEGA APP")
st.write("This app exists to test out all the st commands we possibly can in one go")

st.balloons()
st.snow()

st.write("# input tests")
st.text_input("textbox")
st.number_input("number")
st.slider("slider")
st.button("button")
st.checkbox("checkbox")
st.radio("radio", ["cat", "dog"])
st.selectbox("selectbox", ["cat", "dog"])
st.multiselect("multiselect", ["cat", "dog"])
st.select_slider("select slider", ["cat", "dog"])
st.text_area("text area")
st.date_input("date input")
st.time_input("time input")
st.file_uploader("file input")
st.write("file uploader fails with 404 code")
st.color_picker("color picker")
text_contents = """This is some text"""
st.download_button("Download some text", text_contents)
st.camera_input("cam input")

st.write("# text elements")
st.write("st write")
st.markdown("hello markdown")
st.header("hello header")
st.subheader("hello subheader")
st.caption("hello caption")
st.code("a = 1234")
st.text("hello text")
st.latex("\int a x^2 \,dx")
"""
hello magic
"""

st.write("# data display elements")
chart_data = pd.DataFrame(np.random.randn(20, 3), columns=["a", "b", "c"])

st.dataframe(chart_data)
st.table(chart_data)
st.metric("Metric", 42, 2)
st.write("st json")
st.json(chart_data.head().to_dict())


st.write("# chart elements")
st.line_chart(chart_data)
st.area_chart(chart_data)
st.bar_chart(chart_data)

map_df = pd.DataFrame(
    np.random.randn(1000, 2) / [50, 50] + [37.76, -122.4], columns=["lat", "lon"]
)


"""
st.map does not work currently
"""

"""
matplotlib, plotly, bokeh, pydeck, graphviz, and vegalite will be available when we turn on the anaconda packages in q4
"""

st.write("# Media elements")
image_nums = np.random.randint(255, size=(144, 144), dtype=np.uint8)

st.write("st image does not work currently")
st.image(image_nums)

fs = 44100
data = np.random.uniform(-1, 1, fs)
st.audio(data)

st.video(data)
st.image(image_nums)

st.write("# layouts and containers")

st.sidebar.write("sidebar test")

a, b = st.columns(2)
with b:
    st.write("col check, should be second column")

with a:
    st.write("col check, should be first column")

taba, tabb = st.tabs(["tab a", "tab b"])
tabb.write("tab b test")

with st.expander("open to see expander"):
    st.write("works!")

c = st.container()

c.write("should write in the container")

a = st.empty()

a.write("should write in the empty block")


st.write("# display progress and status")

st.write("progress bar test")
my_bar = st.progress(0)
for percent_complete in range(100):
    my_bar.progress(percent_complete + 1)


with st.spinner("Wait!"):
    st.write("hey")

st.error("st error")
st.warning("st warning")
st.info("st info")
st.success("st success")
e = RuntimeError("example of error")
st.exception(e)
```

## Chapter 1  Central Limit Theorem app
```python
import streamlit as st 
import numpy as np 
import matplotlib.pyplot as plt 

st.title('Illustrating the Central Limit Theorem with Streamlit') 
st.subheader('An App by XXXXXX') 
st.write(('This app simulates a thousand coin flips using the chance of heads input below,'  
'and then samples with replacement from that population and plots the histogram of the' 
' means of the samples, in order to illustrate the Central Limit Theorem!')) 


perc_heads = st.number_input(label='Chance of Coins Landing on Heads', min_value=0.0, max_value=1.0, value=.5) 
binom_dist = np.random.binomial(1, perc_heads, 1000) 

list_of_means = [] 
for i in range(0, 1000): 
    list_of_means.append(np.random.choice(binom_dist, 100, replace=True).mean()) 

fig, ax = plt.subplots() 
ax = plt.hist(list_of_means) 

st.pyplot(fig) 
```

## Chapter 1  clt_app|/example.py
```python
import streamlit as st   
import numpy as np   
import matplotlib.pyplot as plt  

with st.form('first form'):
	perc_heads = st.number_input(label='Chance of Coins Landing on Heads', min_value=0.0, 	max_value=1.0, value=.5)  
	graph_title = st.text_input(label='Graph Title') 
	my_submit_button = st.form_submit_button()

binom_dist = np.random.binomial(1, perc_heads, 1000)  
list_of_means = []  
for i in range(0, 1000):
	list_of_means.append(np.random.choice(binom_dist, 100, replace=True).mean())  

fig, ax = plt.subplots()  
plt.hist(list_of_means, range=[0,1]) 
plt.title(graph_title) 
st.pyplot(fig) 
```

## ch4
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

#### Chapter 2: Uploading, Downloading, and Manipulating Data
- penguin_app
```python

import altair as alt
import pandas as pd
import seaborn as sns
import streamlit as st

st.title("Palmer's Penguins")
st.markdown("Use this Streamlit app to make your own scatterplot about penguins!")

selected_x_var = st.selectbox(
    "What do you want the x variable to be?",
    ["bill_length_mm", "bill_depth_mm", "flipper_length_mm", "body_mass_g"],
)
selected_y_var = st.selectbox(
    "What about the y?",
    ["bill_depth_mm", "bill_length_mm", "flipper_length_mm", "body_mass_g"],
)

penguin_file = st.file_uploader("Select Your Local Penguins CSV")
if penguin_file is not None:
    penguins_df = pd.read_csv(penguin_file)
else:
    st.stop()

alt_chart = (
    alt.Chart(penguins_df, title="Scatterplot of Palmer's Penguins")
    .mark_circle()
    .encode(
        x=selected_x_var,
        y=selected_y_var,
        color="species",
    )
    .interactive()
)
st.altair_chart(alt_chart, use_container_width=True)

```

##
```python
import streamlit as st

st.title('My To-Do List Creator')

if 'my_todo_list' not in st.session_state:
    st.session_state.my_todo_list = ["Buy groceries", "Learn Streamlit", "Learn Python"]

new_todo = st.text_input("What do you need to do?")
if st.button('Add the new To-Do item'):
    st.write('Adding a new item to the list')
    st.session_state.my_todo_list.append(new_todo)

st.write('My To-Do list is:', st.session_state.my_todo_list)
```

## Chapter 3: Data Visualization
```python

```

##
```python

```
## Chapter 4: Using Machine Learning with Streamlit
- https://github.com/tylerjrichards/Streamlit-for-Data-Science/tree/main/penguin_ml
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


##
```python

```


##
```python

```


##
```python

```

