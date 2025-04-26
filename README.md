# São Paulo Open Political Debate Project

## 🌱 Overview

Welcome to the **Open Political Debate** project — an open-source initiative designed to promote transparency, accessibility, and civic engagement for the 2024 **São Paulo Mayoral Election**.

Our goal was to allow citizens to **ask questions directly to candidates**, and get **responses based on the candidates' officially registered political platforms**, fairly and without bias.  
This project empowered voters with information, helping create a more informed electorate.

You can give it a try at [https://bot.auto.ronaldo.blog.br/debate-sampa](https://bot.auto.ronaldo.blog.br/debate-sampa). (It is available in Portuguese, but you could ask it "please answer in English" and it would do so.)

---

## 🧠 How It Works (Technical Summary)

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

## 📜 Prompts (currently available in portuguese)

### 1. **Candidate Proposal Retrieval**
```
Você é um(a) candidato(a) a prefeito(a) de São Paulo chamada {{ candidate_name }}. Responda as perguntas do debate com base no seu plano de governo.

{{ candidate_proposal_rag }}

Responda apenas perguntas diretamente relacionadas a gestão pública da cidade de São Paulo e suas propostas para ela. Se algo for perguntado e não tiver a ver com isto, diga "Vamos conversar apenas sobre a cidade de São Paulo".

Apenas responda perguntas se encontrar informações nos arquivos enviados, pois nestes arquivos está sua proposta de governo. Se não houver arquivos enviados sobre este tema, diga "Eu ainda não tenho uma proposta sobre este assunto em meu plano de governo".
```

### 2. **Political Analyst Commentary**
```
Você é um especialista em gestão pública que conhece profundamente a cidade de São Paulo e seus desafios. Você está participando de um debate entre candidatos a prefeitura de São Paulo no ano de 2024 e foi convidado a comentar as propostas dos candidatos de acordo com as perguntas feitas pelo usuário.

Resuma as propostas dos candidatos, mencione as que são parecidas e diga quais são viáveis e as não viáveis em sua opinião.
```

The agent is then feeded with the candidates' answers and the user's question.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|:---|:---|
| **Redis** | Temporarily stored answers for cross-candidate analysis |
| **TypeBot** | Built the conversational interface for voters |
| **PostgreSQL** | Stored conversation metadata |
| **Docker** | Containerized and orchestrated the full solution |
| **Custom LLM Agents** | Generated candidate-specific responses and political analysis |
| **n8n** | Automated ETL workflows: pulling data, chunking texts, generating embeddings |
| **ElasticSearch** | Stored embeddings for fast semantic search |
| **TSE API** | Provided the official candidate platforms |

---

## 💜 Social Impact

- **Transparency:** Responses were grounded in candidates' official proposals — no hallucinations or fake content.
- **Fairness:** Every candidate received equal treatment through a standardized and unbiased process.
- **Civic Engagement:** Encouraged citizens to engage thoughtfully with electoral content.
- **Open Source:** Everything was developed transparently so that it could be audited and reused for future elections.

---

## 📊 Architecture Diagram

![Architecture Diagram](Screenshots/sao-paulo-political-debate.drawio.png)


## 📸 Screenshots

Some screenshots of the project are available at the [Screenshots](Screenshots) folder.
