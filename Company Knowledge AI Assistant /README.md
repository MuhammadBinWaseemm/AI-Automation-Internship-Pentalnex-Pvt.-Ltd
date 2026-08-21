# Company Knowledge AI Assistant — Day 24

## Project Name
Company Knowledge AI Assistant — Day 24

---

## Problem Statement
Companies and organizations often have important information stored across documents, spreadsheets, and internal knowledge bases. A general-purpose AI model may not have access to this information and can generate inaccurate or invented answers.

This project builds a Retrieval-Augmented Generation (RAG) based AI Assistant that can answer questions using company-specific knowledge stored in a vector database.

The assistant retrieves relevant information from the knowledge base before generating an answer and provides a source where possible. If the requested information is not available, the assistant returns a clear fallback instead of inventing an answer.

---

## Objective
The objective of this project is to build a complete Company Knowledge AI Assistant that:

- Ingests company knowledge from a Google Sheet.
- Splits and processes the knowledge into searchable chunks.
- Generates vector embeddings.
- Stores the embeddings in a Supabase vector database.
- Retrieves relevant information when a user asks a question.
- Uses an AI Agent to generate a grounded response.
- Provides the source/topic used for the answer.
- Prevents the AI from inventing company policies.
- Returns an "Information not available in the knowledge base." fallback when appropriate.
- Supports conversation memory.

---

## Workflow Architecture
The project consists of two main workflows.

### 1. Document Ingestion Workflow
The ingestion workflow loads company knowledge into the vector database.

```text
Google Sheets
      ↓
Extract / Clean Content
      ↓
Recursive Character Text Splitter
      ↓
OpenAI Embeddings
      ↓
Supabase Vector Store
```

The Google Sheet contains 50 company knowledge entries. The ingestion workflow converts these entries into embeddings and stores them in the Supabase `documents` table.

### 2. Company Knowledge AI Assistant
The assistant uses the stored knowledge to answer user questions.

```text
User Question
      ↓
Chat Trigger
      ↓
AI Agent
      ↓
Company Knowledge Base Tool
      ↓
Supabase Vector Store
      ↓
Retrieve Relevant Knowledge
      ↓
AI Model
      ↓
Grounded Answer
      ↓
Source / Fallback
```

Conversation memory is also connected to the AI Agent.

---

## Technologies Used
- n8n
- Google Sheets
- Supabase
- Supabase Vector Store
- OpenAI Embeddings
- OpenAI Chat Model
- AI Agent
- Retrieval-Augmented Generation (RAG)
- Recursive Character Text Splitter
- Conversation Memory
- JSON
- Postman

---

## Nodes Used

### Ingestion Workflow
- Google Sheets
- Data Cleaning / Transformation
- Recursive Character Text Splitter
- OpenAI Embeddings
- Supabase Vector Store

### Company Knowledge AI Assistant
- Chat Trigger
- AI Agent
- Chat Model
- Supabase Vector Store
- OpenAI Embeddings
- Simple Memory
- Response / Output formatting

The Day-24 assistant also preserves the additional tools developed during Day 23, including:
- Bitcoin Price API Tool
- Save User Preference
- Get User Preference

---

## Setup Instructions

### 1. Prepare the Knowledge Source
Create or open the Google Sheet containing the company knowledge. The dataset contains at least 20 knowledge entries (the current dataset contains 50 entries).

The entries cover topics such as:
- Leave
- Attendance
- Working Hours
- Internship Guidelines
- Code of Conduct
- Meetings
- Communication
- Security
- Project Guidelines
- Remote Work
- Performance
- Reporting
- Holidays
- Equipment
- Data Protection
- Training
- Feedback
- Escalation
- Task Submission
- General Procedures

### 2. Configure the Ingestion Workflow
Open the RAG Document Ingestion workflow in n8n. The workflow should read the Google Sheet and process the entries. The data is cleaned and split into chunks using the Recursive Character Text Splitter. The chunks are converted into embeddings using OpenAI Embeddings, then stored in the Supabase vector database.

### 3. Run the Ingestion Workflow
Run the ingestion workflow after the Google Sheet has been populated. Verify that the documents/vectors appear in the Supabase `documents` table. The vector database should contain at least 20 searchable knowledge entries.

### 4. Open the Company Knowledge AI Assistant
Open the **Company Knowledge AI Assistant (Day 24)** workflow. Use the Chat Trigger to open the chat interface.

### 5. Test the Assistant
Ask questions about the company knowledge stored in the vector database.

Example:
> What is the company leave policy for interns?

The assistant should retrieve the relevant knowledge and generate a grounded response.

---

## Credentials Required
Only credential names are listed here. No API keys, passwords, tokens, or other secret values should be stored in the repository.

- Google Sheets OAuth2 Credential
- Supabase Credential
- OpenAI API Credential

---

## Workflow Explanation

### Document Ingestion
The Google Sheet acts as the source of company knowledge. The ingestion workflow reads the entries and prepares their content for embedding. The Recursive Character Text Splitter divides larger pieces of information into smaller chunks so they can be efficiently searched. OpenAI Embeddings converts the chunks into numerical vector representations. These vectors and their associated metadata are stored in the Supabase vector database.

### Retrieval
When a user asks a question, the AI Agent determines that company knowledge is required. The Company Knowledge Base tool searches the Supabase vector database for relevant information. The most relevant knowledge is retrieved and provided to the AI model as context.

### Generation
The AI model generates an answer using the retrieved knowledge. The system prompt strictly instructs the model to avoid inventing company policies. The response includes the answer and relevant source/topic information where available.

### Source Attribution
The assistant uses the topic metadata from the retrieved documents to identify the source.

Example:
```text
Answer:
Interns should submit leave requests in advance to their team lead.

Source:
Leave Requests
Attendance
Working Hours
```

This allows users to understand where the information came from.

### Fallback Handling
If the requested information cannot be found in the knowledge base, the assistant returns:

> Information not available in the knowledge base.

The assistant does not invent or guess a company policy.

### Conversation Memory
The assistant includes conversation memory. This allows the AI Agent to remember information from earlier messages within the same conversation.

Example:
```text
User: My name is Hatim.
Assistant: Hi Hatim! How can I help you?

User: What's my name?
Assistant: Your name is Hatim.
```

---

## Test Cases
The following test cases can be used to evaluate the assistant.

| # | Test Question | Expected Result |
|---|----------------|------------------|
| 1 | What is the company leave policy for interns? | Grounded answer + source |
| 2 | How should an intern request leave? | Grounded leave information + source |
| 3 | What are the working hours? | Working-hours information + source |
| 4 | What are the attendance requirements? | Attendance information + source |
| 5 | What are the internship guidelines? | Internship information + source |
| 6 | What is the company's code of conduct? | Code of conduct information + source |
| 7 | How should project tasks be submitted? | Project/task information + source |
| 8 | What are the security guidelines? | Security information + source |
| 9 | How should employees communicate professionally? | Communication information + source |
| 10 | What is the company's policy on scuba diving during weekends? | Information not available in the knowledge base. |

The final test results should be recorded using the actual responses returned by the workflow.

---

## Error Handling
The workflow includes several safeguards:

**Missing Knowledge**
If the required information cannot be found in the vector database, the assistant returns:
> Information not available in the knowledge base.

**Hallucination Prevention**
The AI Agent is instructed to use retrieved knowledge instead of relying on general model knowledge for company-specific questions.

**Vector Retrieval**
The assistant uses the Supabase vector database to retrieve relevant knowledge before generating company-specific answers.

**Credential Errors**
The workflow requires valid Google Sheets, Supabase, and OpenAI credentials. If credentials are invalid or unavailable, the corresponding workflow operation will fail.

**External Service Failures**
The system depends on external services such as Supabase, Google Sheets, and OpenAI. Temporary service failures or rate limits may prevent successful execution.

---

## Known Limitations
- AI-generated answers can still contain errors.
- Retrieval quality depends on the quality and completeness of the knowledge base.
- Poorly written or ambiguous knowledge entries may result in less accurate answers.
- The system depends on external services and valid credentials.
- Vector search may retrieve irrelevant information for ambiguous questions.
- The assistant cannot answer information that does not exist in the knowledge base.
- The system is intended as an internship demonstration and would require additional security, monitoring, authentication, and testing before production use.
- Knowledge changes require the ingestion workflow to be run again so that updated information becomes available to the assistant.

---

## Future Improvements
- Add automatic synchronization between Google Sheets and Supabase.
- Add duplicate-document detection during ingestion.
- Add document versioning.
- Add confidence scoring for retrieved answers.
- Add a human escalation system.
- Add authentication for the Chat interface.
- Add access control for different departments.
- Add multilingual support.
- Add analytics and query monitoring.
- Add more advanced metadata filtering.
- Add a dedicated document management interface.
- Add automatic re-indexing when company knowledge changes.
- Add Telegram or other chat interfaces.
- Add evaluation metrics for retrieval and answer quality.
- Add automated testing for hallucination and fallback behavior.

# Demo Link:- https://drive.google.com/file/d/1Qq1-So1Bt-lmNFDGipHO7x1eFRbgmqD0/view?usp=sharing 
