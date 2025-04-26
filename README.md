# São Paulo Open Political Debate Project

Welcome to the **Open Political Debate** project — an open-source initiative designed to promote transparency, accessibility, and civic engagement for the 2024 **São Paulo Mayoral Election**.

Our goal was to allow citizens to **ask questions directly to candidates**, and get **responses based on the candidates' officially registered political platforms**, fairly and without bias.  
This project empowered voters with information, helping create a more informed electorate.

You can give it a try at [https://bot.auto.ronaldo.blog.br/debate-sampa](https://bot.auto.ronaldo.blog.br/debate-sampa). (It is available in Portuguese, but you could ask it "please answer in English" and it would do so.)

---

## How It Worked

This project automated the collection, processing, and interaction with candidates' political proposals through a chatbot, powered by AI and public data.

Here's a step-by-step flow:

### 1. **Collecting Candidate Proposals**
- We **automatically pulled** political platforms from the **Superior Electoral Court (TSE)** using their public **REST API**.
- Platforms were **split into chunks** to make processing manageable.
- Each chunk was processed to **generate an embedding** — a numerical vector representation of the text.

### 2. **Storing Embeddings**
- The embeddings were stored in an **ElasticSearch** instance with a **dense vector field**, allowing for efficient semantic search.

### 3. **User Interaction (Debate Interface)**
- Through a chatbot built with **TypeBot**, users:
  - Received a **disclaimer** explaining how the system worked.
  - **Submitted questions** to the candidates.

### 4. **Answer Generation (Debate Phase)**
- The user's question was converted into an embedding.
- The system **searched for relevant proposals** from each candidate based on semantic similarity.
- For each candidate:
  - A **dedicated LLM agent** retrieved and integrated the candidate’s proposals (using Retrieval-Augmented Generation, RAG) to formulate a response.
  - The candidate's answer for the specific topic was **stored**.
  - The **candidate's proposal** was shown to the user.

### 5. **Political Analyst Commentary (Final Phase)**
- After collecting all candidates' answers, a **Political Analyst Agent** generated:
  - A **summary** and **comparative analysis** across all candidate responses.
  - Additional **commentary** providing context, strengths, and weaknesses in the proposals.

---

## Technical Stack

| Technology | Purpose |
|:---|:---|
| **n8n** | Automated ETL workflows: pulling data, chunking texts, generating embeddings |
| **ElasticSearch** | Stored embeddings for fast semantic search |
| **Redis** | Temporarily stored answers for cross-candidate analysis |
| **TypeBot** | Built the conversational interface for voters |
| **PostgreSQL** | Stored conversation metadata |
| **Docker** | Containerized and orchestrated the full solution |
| **Custom LLM Agents** | Generated candidate-specific responses and political analysis |
| **TSE API** | Provided the official candidate platforms |

---

## Social Impact

- **Transparency:** Responses were grounded in candidates' official proposals — no hallucinations or fake content.
- **Fairness:** Every candidate received equal treatment through a standardized and unbiased process.
- **Civic Engagement:** Encouraged citizens to engage thoughtfully with electoral content.
- **Open Source:** Everything was developed transparently so that it could be audited and reused for future elections.

---

## Diagram

![Technical Diagram](Screenshots/sao-paulo-political-debate.drawio.png)
