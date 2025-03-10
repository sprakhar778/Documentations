
---

# 🚀 How to stream a response back

Experience real-time, piece-by-piece text generation that enhances interactivity and user engagement.

**Example**:
A live AI assistant in a customer support chat. Instead of making users wait for a full answer, the bot streams each sentence as it processes, making conversations feel more natural and responsive.


---

## 🔄 Synchronous Streaming

### 💡 What It Does:

✅ **Initialization:** Creates a  object with the model.  

✅ **Streaming Response:** Sends a prompt and receives the answer in multiple small chunks.  
✅ **Immediate Output:** Each chunk is printed as soon as it's available, separated by a pipe (`|`).

### 📝 Example Code:

```python
from langchain_anthropic.chat_models import ChatAnthropic

chat = ChatAnthropic(model="claude-3-haiku-20240307")
for chunk in chat.stream("Write me a 1 verse song about goldfish on the moon"):
    print(chunk.content, end="|", flush=True)
```

### 🎵 Sample Output:

```
Here| is| a| |1| |verse| song| about| gol|dfish| on| the| moon|:|
Floating| up| in| the| star|ry| night|,|
Fins| a|-|gl|im|mer| in| the| pale| moon|light|.|
Gol|dfish| swimming|,| peaceful| an|d free|,|
Se|ren|ely| |drif|ting| across| the| lunar| sea|.
```

---

## ⏩ Asynchronous Streaming

### 💡 What It Does:

✅ **Async Initialization:** Uses `async for` for asynchronous iteration.  
✅ **Non-Blocking:** Runs tasks while waiting for text chunks.  
✅ **Dynamic Response:** Ideal for multitasking applications.

### 📝 Example Code:

```python
from langchain_anthropic.chat_models import ChatAnthropic

chat = ChatAnthropic(model="claude-3-haiku-20240307")
async for chunk in chat.astream("Write me a 1 verse song about goldfish on the moon"):
    print(chunk.content, end="|", flush=True)
```

### 🎵 Sample Output:

```
Here| is| a| |1| |verse| song| about| gol|dfish| on| the| moon|:|
Floating| up| above| the| Earth|,|
Gol|dfish| swim| in| alien| m|irth|.|
In| their| bowl| of| lunar| dust|,|
Gl|it|tering| scales| reflect| the| trust|
Of| swimming| free| in| this| new| worl|d,|
Where| their| aqu|atic| dream|'s| unf|ur|le|d.|
```

---

## ✨ Why Use Streaming?

🚀 **Immediate Feedback:** Start displaying text as soon as it's generated.  
🎭 **Enhanced Interactivity:** Users experience a dynamic, evolving response.  
⚡ **Efficiency:** Ideal for long responses, creative applications, or real-time interactions.

---

## 🔍 Summary

✅ **Synchronous Streaming:** Uses a simple `for` loop to process and print text chunks as they arrive.  
✅ **Asynchronous Streaming:** Utilizes `async for` for non-blocking execution, making it perfect for concurrent tasks.

Leverage streaming to create engaging, interactive experiences that captivate users! 🚀✨

