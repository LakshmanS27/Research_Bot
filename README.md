
# arXiv Computer Science Chatbot

This project builds a **research chatbot** for arXiv Computer Science papers.  
It allows you to search, retrieve, and summarize relevant papers using SBERT embeddings, FAISS vector search, and Streamlit UI.

---

## Features

✅ Download and preprocess the arXiv metadata dataset  
✅ Filter for Computer Science papers  
✅ Clean and save the data  
✅ Create sentence embeddings using `sentence-transformers`  
✅ Build a FAISS vector store for fast retrieval  
✅ Interactive Streamlit chatbot with summarization  
✅ Deploy the chatbot using `ngrok` to get a public URL

---

## Dataset

- Source: [arXiv Metadata on Kaggle](https://www.kaggle.com/datasets/Cornell-University/arxiv)
- File: `arxiv-metadata-oai-snapshot.json`

---

## Installation

Run these commands step by step in **Google Colab**.

### Install dependencies
```bash
!pip install pandas tqdm
!pip install sentence-transformers faiss-cpu
!pip install streamlit transformers sentence-transformers faiss-cpu
!pip install pyngrok
!pip install kaggle
```

### Save dependencies
```bash
!pip freeze > requirements.txt
```

You can download it:
```python
from google.colab import files
files.download("requirements.txt")
```

---

## Kaggle API Setup

1️⃣ Download your `kaggle.json` from [Kaggle API](https://www.kaggle.com/settings/account)  
2️⃣ Upload it:
```python
from google.colab import files
files.upload()
```

3️⃣ Move and set permissions:
```bash
!mkdir -p ~/.kaggle
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
```

---

## Download Dataset

```bash
!kaggle datasets download -d Cornell-University/arxiv
!unzip -o arxiv.zip
```

---

## Preprocess Data

- Parse JSON
- Filter Computer Science papers
- Clean text
- Save as CSV (`cs_arxiv_cleaned.csv`)

---

## Build Vector Store

- Create embeddings with `all-MiniLM-L6-v2`
- Build FAISS index
- Save index and metadata

---

## Run the Chatbot

### Local setup with Streamlit & ngrok

1️⃣ Kill existing Streamlit processes (optional)
```bash
!pkill -f streamlit || echo "No Streamlit process to kill"
```

2️⃣ Configure ngrok
```python
from pyngrok import ngrok
ngrok.kill()

NGROK_TOKEN = input("Enter your ngrok authtoken: ").strip()
os.environ["NGROK_AUTHTOKEN"] = NGROK_TOKEN
!ngrok config add-authtoken $NGROK_AUTHTOKEN
```

3️⃣ Launch Streamlit
```python
from pyngrok import ngrok
import threading, os

public_url = ngrok.connect(8501)
print(f"🌐 Streamlit app running at: {public_url}")

def run():
    os.system("streamlit run main.py")

thread = threading.Thread(target=run)
thread.start()
```

---

## File Structure

```
.
├── cs_arxiv_cleaned.csv       # Cleaned dataset
├── cs_arxiv_index.faiss       # FAISS index
├── cs_arxiv_ids.pkl           # Paper IDs
├── cs_arxiv_texts.pkl         # Text content
├── main.py                    # Streamlit app (see below)
└── requirements.txt           # Python dependencies
```

---

## Streamlit App (`main.py`)

The `main.py` contains:
- Query input box
- Retrieves top-5 relevant papers
- Optional summarization of abstracts using `distilbart-cnn-12-6`

---

## Notes

- SBERT model: `all-MiniLM-L6-v2`
- Summarizer: `sshleifer/distilbart-cnn-12-6`
- Recommended to test on GPU runtime in Colab.

---

## License

This project is open-sourced under MIT License.

---

## Credits

- [Sentence Transformers](https://www.sbert.net/)
- [FAISS](https://faiss.ai/)
- [Streamlit](https://streamlit.io/)
- [arXiv Dataset](https://www.kaggle.com/datasets/Cornell-University/arxiv)

---

Happy Researching!
