# Returning Structured Data from a Model

## 🚀 Introduction
When working with AI models, it's useful to structure the output into a predictable format. This ensures that the responses are easy to parse and use in applications.

## 🛠️ Steps to Return Structured Data


### **1️⃣  Load the Model**
Initialize the language model using LangChain’s `init_chat_model` method.

```python
import getpass
import os
from langchain.chat_models import init_chat_model

if not os.environ.get("GROQ_API_KEY"):
  os.environ["GROQ_API_KEY"] = getpass.getpass("Enter API key for Groq: ")

llm = init_chat_model("llama3-8b-8192", model_provider="groq")
```

### **2️⃣ Create a Schema**
Define the expected output using **Pydantic**, which helps enforce structure and validation.

#### **Example: Single Schema**
```python
from typing import Optional
from pydantic import BaseModel, Field

class Joke(BaseModel):
    """Joke to tell user."""
    setup: str = Field(description="The setup of the joke")
    punchline: str = Field(description="The punchline to the joke")
    rating: Optional[int] = Field(
        default=None, description="How funny the joke is, from 1 to 10"
    )
```



### **3️⃣ Bind Schema with Model**
Now, bind the schema with the model to ensure structured output.

```python
structured_llm = llm.with_structured_output(Joke)
```

### **4️⃣ Invoke the Model to Get Structured Response**
Call the model and get the structured response.

```python
structured_llm.invoke("Tell me a joke about cats")
```
✅ **Example Output:**
```json
{
  "setup": "Why was the cat sitting on the computer?",
  "punchline": "Because it wanted to keep an eye on the mouse!",
  "rating": 7
}
```

---

## 🤹 Choosing Between Multiple Schemas
Sometimes, we need the model to return different structured outputs based on the query.

### **1️⃣ Define Multiple Schemas**

```python
from typing import Union

class ConversationalResponse(BaseModel):
    """Respond in a conversational manner. Be kind and helpful."""
    response: str = Field(description="A conversational response to the user's query")

class FinalResponse(BaseModel):
    final_output: Union[Joke, ConversationalResponse]
```

### **2️⃣ Bind Multiple Schemas to the Model**

```python
structured_llm = llm.with_structured_output(FinalResponse)
```

### **3️⃣ Invoke and Get Structured Response**

#### **Example 1: Requesting a Joke**
```python
structured_llm.invoke("Tell me a joke about cats")
```
✅ **Output:**
```json
{
  "final_output": {
    "setup": "Why was the cat sitting on the computer?",
    "punchline": "Because it wanted to keep an eye on the mouse!",
    "rating": 7
  }
}
```

#### **Example 2: Asking a Question**
```python
structured_llm.invoke("How are you today?")
```
✅ **Output:**
```json
{
  "final_output": {
    "response": "I'm just a computer program, so I don't have feelings, but I'm here and ready to help you with whatever you need!"
  }
}
```

---

## 🎯 Summary
1️⃣ **Load the Model** – Initialize the LLM.

2️⃣ **Create a Schema** – Define the expected structure using Pydantic.



3️⃣ **Bind Schema with Model** – Attach the schema to enforce structured output.

4️⃣ **Invoke Model** – Get structured responses based on the input.

