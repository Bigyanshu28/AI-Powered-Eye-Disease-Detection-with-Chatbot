# AI-Powered-Eye-Disease-Detection-with-Chatbot

## 📖 Project Overview
This project is an advanced, AI-powered medical orchestration system built around a graph-based workflow (inspired by LangGraph). Instead of routing every user request to a single, expensive, monolithic Large Language Model (LLM), this system represents every stage of the patient journey as an independent node. 

The architecture intelligently understands user intent, routes conversations, invokes specialized AI or deterministic components only when necessary, and ultimately delivers medical guidance, doctor recommendations, and appointment bookings. 

By separating onboarding, triage, symptom collection, disease-specific workflows, knowledge retrieval, and booking into distinct modular layers, the system is highly scalable, cost-efficient, and easy to maintain.

## 🧠 Core Design Philosophy
**"The Right Model for the Right Task"**

The architecture follows a strict engineering principle: **Every node performs only one responsibility and uses the smallest model capable of performing that task.**

Instead of defaulting to an expensive reasoning model for every interaction, the workflow strategically allocates model tiers based on computational and clinical complexity. For example:
* Greeting users requires minimal reasoning (Tier-3).
* Extracting structured patient demographics requires moderate capability (Tier-2).
* Medical triage and symptom analysis require high-level clinical reasoning (Tier-1).
* Database lookups and specialist routing require **zero** LLM invocation (Deterministic/Python).

This tiered approach dramatically reduces operational inference costs while preserving high diagnostic safety and quality.

---

## 🗺️ Overall User Flow

```text
[ User Input ]
      │
      ▼
[ 1. Intake Node ] ── (Extracts Demographics -> Supabase)
      │
      ▼
[ 2. Intelligent Triage ] ── (Emergency Keyword Scan ──> 🚨 EXIT if Emergency)
      │
      ▼
[ 3. Dynamic Symptom Collector ]
      │
      ▼
[ 4. Main Router ] (Deterministic)
      │
      ├──> [ 5. General Chat ] (Non-medical intent)
      │
      ├──> [ 6. Eye-Specific Workflow ] (Vision Model -> Diagnosis -> Protocol)
      │
      ├──> [ 7. Medical Advice ] (Self-RAG Pipeline -> BioBERT / pgvector)
      │
      └──> [ 8. Doctor Search & 9. Booking ] (Direct Supabase Query)
                  │
                  ▼
          [ Patient Report ]
