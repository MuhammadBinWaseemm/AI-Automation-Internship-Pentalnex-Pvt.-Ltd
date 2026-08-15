# Company / HR Policy Assistant — RAG Q&A

A simple RAG-based assistant that answers HR and company policy questions using a predefined knowledge base.

## Objective

Build an assistant that:

* Answers questions only from the provided knowledge base.
* Does not invent or assume policies.
* Gives a clear fallback when information is unavailable.
* Mentions the source when possible.

## Knowledge Base

The knowledge base contains:

* Leave Policy
* Working Hours
* Attendance
* Internship Guidelines
* Code of Conduct

## How It Works

```text
User Question
     ↓
Retriever
     ↓
Relevant Policy Information
     ↓
Chat Model
     ↓
Answer + Source
```

The retriever finds relevant information from the knowledge base, and the chat model generates an answer using only that information.

## Example

**Question:**

> How many casual leaves are allowed?

**Answer:**

> Employees are allowed the number of casual leaves specified in the Leave Policy.
> **Source:** Leave Policy

## Unknown Information

If the answer is not present in the knowledge base:

> Information not available in the company knowledge base.

The assistant does not guess or create policies.

## Tech Stack

* RAG
* Embeddings
* Vector Store
* Chat/LLM Model

## Key Features

* Knowledge-base-based Q&A
* Grounded responses
* Safe fallback
* Source/reference citation
