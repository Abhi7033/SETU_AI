# Setu (Sanjeevani) AI
**The Rural Health Bridge** | *Empowering ASHA Workers with Voice & AI*

Submitted to: AI for Bharat Hackathon

## The Problem
India faces a critical healthcare shortage in rural areas:
*   **1:10,000+** Doctor-to-Patient ratio in villages.
*   **Language Barriers:** Patients speak local dialects; Doctors speak English/Hindi.
*   **Lost History:** Paper-based records are often lost or illegible.
*   **Overburdened ASHAs:** Frontline workers spend more time on paperwork than patient care.

## The Solution: Setu AI
Setu (Bridge) is an **Offline-First, Multilingual Voice AI Assistant** designed for ASHA workers. It bridges the gap between rural patients and the formal healthcare ecosystem.

### Key Features
*   **Voice-to-Record:** Converts local dialect speech (Bhojpuri, Tamil, etc.) into structured **English Medical Records (SOAP)**.
*   **Smart Triage (Offline):** Uses on-device Small Language Models (SLMs) to **instantly flag emergencies** (Red/Yellow/Green) without internet.
*   **Prescription Translator:** Scans English prescriptions (OCR) and explains dosage in the patient's native language via voice.
*   **Offline-First:** Fully functional without connectivity; syncs when online.

## Tech Stack
*   **Frontend:** Flutter (Mobile App)
*   **ASR (Voice):** OpenAI Whisper (Fine-tuned on Indic Languages) / Bhashini
*   **Edge AI:** Llama 2 (7B Quantized) / Google Gemma (TensorFlow Lite)
*   **Translation:** Meta NLLB-200 (No Language Left Behind)
*   **Backend:** Supabase (PostgreSQL) + Python (FastAPI)

## Usage Workflow
1.  **Consultation:** ASHA worker taps "Listen" and speaks in local dialect.
2.  **Processing:** AI Transcribes -> Translates -> Extracts Symptoms.
3.  **Triage:** App displays: **"High Fever + Stiff Neck = Possible Meningitis (RED FLAG)"**.
4.  **Action:** ASHA refers patient to hospital; Digital note is synced to District Doctor.

## Impact
*   **Access:** Bringing specialist-level triage to the remotest village.
*   **Speed:** Reducing diagnosis time by **50%**.
*   **Adherence:** Increasing medication compliance by **40%** through native-language explanations.
