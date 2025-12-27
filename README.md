# Project SocioScope 🔭

**SocioScope** is an Aspect-Based Sentiment Analysis (ABSA) project focused on analyzing socio-economic discourse on social media. 

Due to the scarcity of high-quality, labeled datasets for specific socio-economic domains, this project implements a robust **Synthetic Data Generation Pipeline** using Large Language Models (LLMs) to create a supervised training dataset.

## 🚀 Overview

The goal of this project is to train a model capable of detecting sentiment towards specific aspects (e.g., Cost of Living, Healthcare) within a single tweet. To achieve this, we employed a **"Reverse Generation"** approach: generating the ground-truth labels first, and then instructing an LLM to generate matching text.

## 🛠️ Data Generation Pipeline

We built a custom pipeline to ensure 100% label accuracy. Instead of letting the model generate text freely and then labeling it, we dictate the sentiment mathematically before text creation.

### The 4-Step Process:

1.  **Configuration & Diversity** 🎭
    We defined pools of attributes to ensure data variance:
    * **10 Aspects:** Cost of Living, Healthcare, Education, Personal Security, Employment, Transportation, Government, Environment, Social Equality, Taxation.
    * **7 Personas:** e.g., Struggling student, Wealthy business owner, Political activist.
    * **6 Styles:** e.g., Sarcastic, Formal complaint, Slang & Emojis.

2.  **Label Generation (Ground Truth)** 📊
    The `generate_target_vector()` function selects 1-2 active aspects and assigns a sentiment score (`-1` or `1`).
    * *Result:* A vector like `{'Healthcare': -1, 'Taxes': 0, ...}` exists before the text does.

3.  **Prompt Construction** 📝
    The `create_prompt()` function combines the vector, a random persona, and a random style into a natural language instruction.
    * *Example:* "You are a **retired teacher**. Write a **formal complaint** tweet about **Education (Negative)**."

4.  **Execution (Llama 3.2)** 🤖
    The `generate_synthetic_data()` loop orchestrates the process using **Ollama** and **Llama 3.2**. It captures the output and pairs it with the pre-generated vector in a Pandas DataFrame.
