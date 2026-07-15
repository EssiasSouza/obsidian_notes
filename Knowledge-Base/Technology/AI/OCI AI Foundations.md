Source: #source/internet_resources
Project: #project/personal
Areas: #area/work
Subject: #subject/ai
Type: #type/learning
Learning priority: #priority/P1
Status: #status/learning
Related: [[oci-ai-foundations-study-plan]]
[[phase1-2026-mon2-july]]

---

# OCI AI Foundations

Reference notes for the Oracle Cloud Infrastructure (OCI) AI Foundations curriculum. Focuses on AI/ML/DL core principles, OCI AI services, Generative AI models, and infrastructure.

---

## 1. Fundamentals of AI, ML, and DL

Understanding the hierarchal relationship: **AI ⊃ ML ⊃ DL**.

### Artificial Intelligence (AI)
Simulating human intelligence processes by machines, especially computer systems (learning, reasoning, self-correction).

### Machine Learning (ML)
A subset of AI that allows systems to learn from data and improve from experience without being explicitly programmed.
*   **Supervised Learning**: Algorithms trained on labeled data.
    *   *Classification*: Output is a discrete category (e.g., Spam vs. Not Spam, fraud detection).
    *   *Regression*: Output is a continuous numerical value (e.g., house price forecasting).
*   **Unsupervised Learning**: Algorithms find hidden patterns or structures in unlabeled data.
    *   *Clustering*: Grouping data points with similar features (e.g., customer segmentation).
    *   *Dimensionality Reduction*: Simplifying data without losing critical information.
*   **Reinforcement Learning**: Learning by trial and error through reward/punishment feedback loop.

### Deep Learning (DL)
A specialized subset of ML based on **Artificial Neural Networks (ANNs)** with multiple layers (hence "deep") that mimic human brain structure to process complex data (images, sound, unstructured text).
*   **Transformers**: Neural network architectures designed for sequential processing, utilizing **Self-Attention mechanisms** to weight the importance of different parts of input data. This is the foundation of modern Large Language Models (LLMs).

---

## 2. OCI Cognitive AI Services

Out-of-the-box pre-trained models accessible via REST APIs without requiring data science expertise:

*   **OCI Language**: Performs text analytics (sentiment analysis, key phrase extraction, language detection, Named Entity Recognition / NER, text classification).
*   **OCI Document Understanding**: Extracts text (OCR), tables, and key-value fields from documents (invoices, receipts, PDF reports).
*   **OCI Vision**: Image analysis (object detection, classification, OCR in images).
*   **OCI Speech**: Converts audio files containing speech into high-fidelity text transcriptions.
*   **OCI Digital Assistant**: Platform for building conversational interfaces (chatbots) using natural language understanding (NLU).

---

## 3. OCI Generative AI Service

A fully managed service providing access to customizable LLMs (e.g., Cohere and Llama models) with enterprise-grade security.

*   **Generation**: Summarizing text, writing emails, copy editing.
*   **Summarization**: Condensing long documents into short, actionable summaries.
*   **Embedding**: Converting text into high-dimensional vector representations for semantic search or Retrieval-Augmented Generation (RAG).
*   **Fine-Tuning**: Customizing models using your own data to perform specific tasks.
*   **Dedicated AI Clusters**: Provisioned compute resources (GPUs) reserved exclusively for running or fine-tuning your generative AI models, ensuring predictable performance and security.

---

## 4. OCI AI Infrastructure

The compute and networking foundation that powers deep learning and heavy AI workloads:

*   **Bare Metal GPUs**: High-performance compute shapes equipped with enterprise NVIDIA GPUs (e.g., A100, H100) bypassing hypervisor overhead.
*   **OCI Superclusters**: Thousands of bare metal instances clustered together.
*   **RDMA Network**: Remote Direct Memory Access over Converged Ethernet (RoCE) networking, providing microsecond latencies and high bandwidth to avoid performance bottlenecks in cluster communication.
