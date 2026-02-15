# Requirements: Setu (Sanjeevani) AI - The Rural Health Bridge

## 1. Problem Statement
**The Rural Healthcare Gap:**
- **Shortage of Specialists:** India has a massive shortage of doctors in rural areas. The doctor-to-patient ratio is often alarming (1:10,000+ in some regions).
- **Overburdened Frontline Workers:** ASHA (Accredited Social Health Activist) workers are the backbone of rural health but often lack advanced diagnostic support and are overburdened with manual data entry.
- **Language Barriers:** Patients often describe symptoms in local dialects (Bhojpuri, Maithili, Tamil, etc.), which may be lost or misinterpreted when communicated to remote doctors who speak English or Hindi.
- **Lost History:** Medical records are often paper-based and lost, leading to poor continuity of care.

## 2. Solution Overview
**Setu (Sanjeevani) AI** is an **Offline-First, Multilingual Voice AI Assistant** designed specifically for ASHA workers and rural clinics. It acts as a bridge between the rural patient and the formal healthcare system.

**Key Features:**
1.  **Voice-to-Record (Symptom Logger):** Listens to patient-ASHA conversations in local dialects and automatically generates a structured, clinical-grade medical note in English (SOAP format) for doctors.
2.  **Smart Triage (Red Flag Detection):** Uses on-device Small Language Models (SLMs) to instantly flag "Red Alert" symptoms (e.g., "chest pain radiating to arm") without needing internet, guiding the ASHA worker to recommend immediate hospitalization if necessary.
3.  **Prescription Translator:** Scans a doctor’s English prescription (OCR) and explains the dosage and instructions back to the patient in their own request language via synthesized voice.

## 3. User Stories
### User Story 1: The ASHA Worker (Meena)
> "As an ASHA worker in a remote village, I want to record patient symptoms by just speaking naturally in our local dialect, so that I don't have to struggle with typing English forms and can focus on the patient."

### User Story 2: The Rural Patient (Raju)
> "As a patient who cannot read English, I want to understand exactly when and how to take my medicine, so that I can recover faster without confusion."

### User Story 3: The Remote Doctor (Dr. Priya)
> "As a remote specialist, I want to receive structured, standardized patient history, so that I can make accurate diagnoses quickly without sifting through unstructured notes."

## 4. Technical Stack
- **Frontend:** Flutter (Mobile App) - for cross-platform, offline-capable UI.
- **AI/ML Core:**
    - **ASR (Speech-to-Text):** OpenAI Whisper (Fine-tuned on Indic languages) or Bhashini API.
    - **LLM/SLM:** Llama-2-7b (Quantized) or Google Gemma (2b/7b) for on-device processing.
    - **Translation:** Meta NLLB (No Language Left Behind) or Google Translate API.
    - **OCR:** Google ML Kit or Tesseract for prescription scanning.
- **Backend (Optional/Sync):** Supabase (PostgreSQL) for syncing data when internet is available.
- **Data Privacy:** All processing happens locally or on secure, compliant servers with consent.
