# ✂️ Trimming Messages in Large Language Models (LLMs)

## 📌 Why Trim Messages?

LLMs have **finite context windows**, meaning they can only process a certain number of tokens at a time. If chat histories grow too long, we need to **trim messages** to ensure the model performs optimally.

**Key reasons to trim messages:**
✅ Prevent exceeding token limits.
✅ Improve efficiency in long-running conversations.
✅ Maintain important context while discarding unnecessary messages.

## 🛠️ Using `trim_messages` to Manage Chat History

LangChain provides `trim_messages`, a powerful tool to shorten chat histories **while preserving essential messages**.

### 📦 Install Required Packages
```bash
%pip install -qU langchain_core
```

### 🏗️ Creating an In-Memory Chat History
```python
from langchain_core.chat_history import InMemoryChatMessageHistory
from langchain_core.messages import (
    AIMessage,
    HumanMessage,
    SystemMessage,
    ToolMessage,
    trim_messages,
)

# Sample chat history with different message types
messages = [
    SystemMessage("You're a good assistant, you always respond with a joke."),
    HumanMessage("I wonder why it's called LangChain"),
    AIMessage(
        'Well, I guess they thought "WordRope" and "SentenceString" just didn't have the same ring to it!'
    ),
    HumanMessage("And who is Harrison chasing anyways?"),
    AIMessage(
        "Hmmm let me think.\n\nWhy, he's probably chasing after the last cup of coffee in the office!"
    ),
    HumanMessage("What do you call a speechless parrot?"),
]


# Storing chat messages in memory (except the last one)

chat_history = InMemoryChatMessageHistory(messages=messages[:-1])

```

### 🔗 Connecting Chat History to a Session
```python
def dummy_get_session_history(session_id):
    if session_id != "1":
        return InMemoryChatMessageHistory()
    return chat_history
```

### 🔥 Implementing `trim_messages`
```python
from langchain_core.runnables.history import RunnableWithMessageHistory
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o")

trimmer = trim_messages(
    max_tokens=45,  # Set token limit
    strategy="last",  # Keep the most recent messages
    token_counter=llm,  # Use LLM to count tokens
    include_system=True,  # Keep system messages with instructions
    start_on="human",  # Ensure chat history starts correctly
)

chain = trimmer | llm
chain_with_history = RunnableWithMessageHistory(chain, dummy_get_session_history)

```

### 📝 Example Usage
```python
response = chain_with_history.invoke(
    [HumanMessage("What do you call a speechless parrot?")],
    config={"configurable": {"session_id": "1"}},
)
print(response)
```
✅ **Ensures only relevant chat history is processed!**

---

## 🔥 Additional Advanced Features

### 1️⃣ In-Memory Chat History
Stores messages in a **memory list** for quick access.
```python
from langchain.schema import HumanMessage
from langchain_core.chat_history import InMemoryChatMessageHistory

# Creating an in-memory chat history with multiple messages
history = InMemoryChatMessageHistory(messages=[
    HumanMessage(content="Hello!"),
    HumanMessage(content="How are you?"),
    HumanMessage(content="Tell me a joke!"),
])

# Retrieving stored messages
for msg in history.messages:
    print(msg.content)
```
✅ **Stores messages efficiently for retrieval!**

### 2️⃣ Creating a Configurable Model
```python
from langchain.chat_models import init_chat_model

configurable_model = init_chat_model(temperature=0)

response = configurable_model.invoke(
    "What's your name?", 
    config={"configurable": {"model": "gpt-4o"}},
)
print(response)
```
✅ **Allows flexible model selection at runtime!**

### 3️⃣ Managing Chat History with `RunnableWithMessageHistory`

Wraps another **Runnable** and manages its chat history automatically.
```python
chat_manager = RunnableWithMessageHistory(chain, dummy_get_session_history)

# Example of invoking with a stored history
response = chat_manager.invoke(
    [HumanMessage("What's the capital of France?")],
    config={"configurable": {"session_id": "1"}},
)
print(response)
```
✅ **Handles updating and retrieving chat messages dynamically!**

---

## 🚀 Summary
✨ `trim_messages` helps manage long chat histories efficiently.

✨ Use `RunnableWithMessageHistory` to wrap chat models for **automatic history management**.

✨ **Keep only relevant messages** to **enhance model performance**.

