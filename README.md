# Course_FAQ_Chatbot

This chatbot answers **FAQs** from users based on the course data (in this case 233 page PDF file)

This data is collected from [Data Engineering Zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp) FAQ document.

I used `llava7b` which is a `large multimodal model`. It combines a `vision encoder` and `Vicuna` for general-purpose visual and language understanding.

I used `llava` because in the dataset there are some instructions that were given using `images`.

**NOTE:** You have to have `Ollama` installed in your local machine

<p></p><p></p><p></p>

![RAG BASED APP](https://machinelearningmastery.com/wp-content/uploads/2024/08/mlm-awan-rag-applications-llamaindex.png)

<p align="center">RAG based application cycle</p>

### **Summary of Steps**  

---

## **1. Import Required Libraries**  
- Import **Streamlit** for UI.
- Import **OS** and **Logging** for file handling and debugging.
- Import **LangChain modules** for PDF processing, embeddings, vector storage, and AI model interactions.
- Import **Ollama** for embedding and LLM functionalities.

---

## **2. Configure Logging & Constants**  
- Set logging level to `INFO` to track progress.  
- Define key **constants**:
  - `DOC_PATH`: Path to the PDF file.
  - `MODEL_NAME`: AI model (`LLaVA`).
  - `EMBEDDING_MODEL`: Model for text embeddings.
  - `VECTOR_STORE_NAME`: Name of the vector database.
  - `PERSIST_DIRECTORY`: Path to store **ChromaDB**.

---

## **3. Load and Process PDF Document**  
✅ **`ingest_pdf(doc_path)`**  
- Checks if the PDF exists.
- Loads it using **`UnstructuredPDFLoader`**.
- Returns the document content.

✅ **`split_documents(documents)`**  
- Splits the PDF into **1200-character chunks**.
- Uses a **300-character overlap** for better context retention.
- Returns the list of document chunks.

---

## **4. Create or Load Vector Database (ChromaDB)**  
✅ **`load_vector_db()`**  
- **Pulls the embedding model** if not available.
- **Checks if the database already exists**:
  - ✅ If yes → Loads the existing database.
  - ❌ If no → Loads the PDF, splits it into chunks, **creates and saves** the vector database.

---

## **5. Create Multi-Query Retriever**  
✅ **`create_retriever(vector_db, llm)`**  
- Uses **MultiQueryRetriever** to:
  - **Rephrase the user’s question** into three variations.
  - Improve search accuracy for retrieving relevant document chunks.
- Returns the retriever.

---

## **6. Build Response Chain for LLM**  
✅ **`create_chain(retriever, llm)`**  
- **Defines a structured RAG prompt**:
  - Restricts AI to answer **only based on retrieved documents**.
- **Creates a LangChain pipeline**:
  1. Fetch context using **retriever**.
  2. Format the prompt.
  3. Pass data to **ChatOllama** model.
  4. Parse and return the AI’s response.

---

## **7. Build Streamlit Web UI**  
✅ **`main()`**  
- **Displays** Streamlit title and input box.
- When a user enters a **question**:
  - Shows a **loading spinner**.
  - Loads **LLM & Vector Database**.
  - Runs **retriever & response chain**.
  - **Displays the AI-generated response**.

---

## **8. Run the Application**  
- Use the command:  
  ```sh
  streamlit run app.py
  ```
- The **FAQ Assistant** loads in a browser window.

---

### **🎯 Final Output**  
- The user **enters a question**.  
- The system **retrieves relevant document sections**.  
- The **AI generates an answer** based on those sections.  
- The answer is **displayed in the UI**. 🚀

### **App Output**  

![RAG BASED APP](https://github.com/Shafin008/Course_FAQ_Chatbot/blob/master/images/q1.png)

![RAG BASED APP](https://github.com/Shafin008/Course_FAQ_Chatbot/blob/master/images/q2.png)

![RAG BASED APP](https://github.com/Shafin008/Course_FAQ_Chatbot/blob/master/images/q23.png)
