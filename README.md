# Customer Support Automation — Quantized LLM + Agentic Workflow

4-bit quantized LLM support pipeline that classifies, retrieves, and drafts responses to tier-1 customer support tickets — built to cut cost-per-ticket while keeping high-risk requests routed to a human.

## Problem

Tier-1 support tickets (order status, refund policy, account issues) are repetitive and high-volume, driving up support cost per ticket. Fully manual handling doesn't scale efficiently; fully automated handling risks mishandling sensitive requests (e.g., refund disputes).

## What this does

1. **Quantizes** a base open-source LLM to 4-bit (bitsandbytes / GPTQ) to reduce inference cost while preserving response quality.
2. **Agentic workflow** (LangChain/LangGraph): classify ticket → retrieve relevant policy/FAQ via RAG → draft response → escalate to human if low-confidence or high-risk.
3. **Benchmarks** quantized vs. full-precision model on accuracy, latency, and cost per 1,000 tickets.
4. **Deploys** on a serverless platform (Google Cloud Functions / Hugging Face Inference Endpoints) for cost-effective scaling.
5. **Guardrails**: prompt injection resistance, escalation rules for sensitive requests, response-quality monitoring and drift logging.

## Architecture

```
Incoming ticket
      │
      ▼
Ticket classification
      │
      ▼
RAG retrieval (policy/FAQ) ──► Draft response (quantized LLM)
      │                                  │
      ▼                                  ▼
Confidence + risk check ──► Escalate to human (if low-confidence
                              or sensitive: refunds, disputes, etc.)
```

## Tech stack

- **Model**: [base model name] quantized to 4-bit via bitsandbytes / GPTQ
- **Agent framework**: LangChain / LangGraph
- **Deployment**: Google Cloud Functions / AWS Lambda / Hugging Face Inference Endpoints
- **Monitoring**: response drift logging, escalation audit trail

## Setup

```bash
git clone [repo-url]
cd support-automation-quantized-llm
pip install -r requirements.txt
```

Set environment variables (`.env`):
```
HF_TOKEN=your_huggingface_token
CLOUD_FUNCTION_KEY=your_key
```

Run the notebook:
```bash
jupyter notebook support_automation_quantized.ipynb
```

## Results

| Metric | Full precision | 4-bit quantized |
|---|---|---|
| Accuracy | Not yet measured — requires a labeled eval set (see Guardrails & limitations below) | Not yet measured — requires a labeled eval set (see Guardrails & limitations below) |
| Avg. latency / ticket | 76,323.70 ms (~76.3s) | 8,465.89 ms (~8.5s) |
| Cost per 1,000 tickets | $31.8015 | $1.1758 |
| Estimated cost reduction | — | 96.30% |




## Guardrails & limitations

- Prompt injection tests included against common jailbreak patterns.
- Hard escalation rules for refund disputes, account security issues, and any low-confidence classification.
- Response drift monitored against a held-out validation set over time.
- Not intended to fully replace human support for sensitive or high-stakes tickets — routes those by design.

## Demo

[[Deployment link / video walkthrough]](https://colab.research.google.com/drive/1RIf9_bZAoqmB9rq6nNWciNmaSPZJSpOx?usp=sharing)

## License

MIT
