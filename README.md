# 🛒 Fast Cart Completion Recommender

This is a **hackathon-ready cart completion recommendation system** designed for very fast offline validation and inference.  
It predicts the top-K items likely to be added to a customer's cart based on **co-occurrence**, **popularity**, and optional **embeddings**.

## 🚀 Features
- **Catalog Capping** – Keeps only the most frequent items to avoid huge class explosion.
- **Recency-Weighted Co-occurrence** – Gives more importance to recent purchase patterns.
- **Popularity Models** – Global, store-level, and hour-of-day popularity.
- **Fast Inference** – Candidate set generation and lightweight scoring (no heavy re-rankers).
- **Optional Word2Vec embeddings** for semantic similarity between items.
- **Offline Evaluation** – Computes Recall@K on a holdout set.

---

## 📂 Project Structure
.
├── recommender.py # Main pipeline implementation
├── order_data.csv # Training data (orders)
├── test_data_question.csv # Test data for inference
├── README.md # This file
├── requirements.txt # Python dependencies
└── TEAM_WWT_Comp2025_Output_fast.xlsx # Generated output (predictions)

markdown
Copy
Edit

---

## ⚙️ Configuration
All settings are in the `CFG` class inside the script:
- **Data paths**
- **Catalog & sampling controls** (`MAX_ITEMS`, `SAMPLE_RECENT_DAYS`, etc.)
- **Co-occurrence parameters** (`ALPHA`, `RECENCY_HALF_LIFE_DAYS`)
- **Candidate generation** (`TOP_POPULAR_K`, `CAND_LIMIT`)
- **Scoring weights** (`WEIGHT_COOCC`, `WEIGHT_POP`, `WEIGHT_EMB`)
- **Word2Vec** options

---

## 📊 Data Format

### Training Data (`order_data.csv`)
Must contain at least:
- `ORDERS` – JSON-like list of item names
- `ORDER_CREATED_DATE` – purchase timestamp
- *(Optional)* `STORE_NUMBER`

### Test Data (`test_data_question.csv`)
Must contain:
- `CUSTOMER_ID`
- `ORDER_ID`
- `item1`, `item2`, ... – items already in the cart (missing items can be `"missing"`)
- *(Optional)* `STORE_NUMBER` and `ORDER_CREATED_DATE`

---

## ▶️ Running the Pipeline

### 1️⃣ Install dependencies
```bash
pip install -r requirements.txt
2️⃣ Update config paths
Edit the CFG class in recommender.py to match your CSV file locations.

3️⃣ Run
bash
Copy
Edit
python recommender.py
4️⃣ Output
Offline Recall@3 is printed to the console.

Predictions are written to:

Copy
Edit
TEAM_WWT_Comp2025_Output_fast.xlsx
🧪 Optional: Enable Embeddings
If you have gensim installed:

Set USE_W2V = True in CFG.

Word2Vec will train on carts and add semantic similarity to the scoring.

📈 Example Output
CUSTOMER_ID	ORDER_ID	item1	item2	RECOMMENDATION 1	RECOMMENDATION 2	RECOMMENDATION 3
1234	5678	Coke	Fries	Burger	Nuggets	Sundae

