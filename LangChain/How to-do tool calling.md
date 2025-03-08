```python
# Import necessary modules
from pydantic import BaseModel, Field
from langchain.chat_models import init_chat_model

# 1️⃣ Define a Pydantic model schema for calculator tool input
class CalculatorInput(BaseModel):
    a: int = Field(description="First number")  # First number
    b: int = Field(description="Second number")  # Second number

# 2️⃣ Defining tools
@tool("multiplication-tool", args_schema=CalculatorInput, return_direct=True)
def multiply(a: int, b: int) -> int:
    """Multiply two numbers."""
    return a * b

@tool("addition-tool", args_schema=CalculatorInput, return_direct=True)
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b

# 3️⃣ Listing tools
tools = [add, multiply]

# Initialize the LLM
llm = init_chat_model("llama3-8b-8192", model_provider="groq")
 
# 4️⃣ Bind the tools to the LLM, allowing it to use them when answering queries
llm_with_tools = llm.bind_tools(tools)

# 5️⃣ Ask a question and invoke the model
query = "What is 3 * 12?"
result = llm_with_tools.invoke(query)
print(result)
```

---

## 🚀 How to Do Function/Tool Calling in LangChain

### 1️⃣ Define the Tool’s Input Schema
Think of it as telling the tool what kind of numbers it will work with. We use `Pydantic` to define structured inputs.

### 2️⃣ Define the Tool as a Python Function
This is where the actual work happens, like adding or multiplying numbers.

### 3️⃣ List All Your Tools
Just put them in a list so the model knows what’s available.

### 4️⃣ Bind the Tools to the Model
This connects everything so the LLM can use the tools when needed.

### 5️⃣ Ask a Question!
The model will pick the right tool and give you the answer! 🚀

