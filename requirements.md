# Requirements Document

## Setu (Sanjeevani) AI - Rural Health Bridge

---

## 1. Problem Statement

### Healthcare Challenges in Rural India

**Shortage of Specialists**
- Doctor-patient ratio exceeds 1:10,000+ in rural areas
- Limited access to specialized medical care
- Long wait times and travel distances for consultations

**Language Barriers**
- Patients communicate in local dialects (Bhojpuri, Tamil, Marathi, etc.)
- Medical professionals primarily use English and Hindi
- Critical information lost in translation during consultations
- Reduced quality of care due to miscommunication

**Overburdened ASHA Workers**
- Frontline health workers struggle with manual data entry
- Time-consuming documentation reduces patient interaction
- Lack of technical tools for efficient record-keeping
- High workload leads to burnout and errors

**Lost Medical History**
- Paper-based records are frequently lost or damaged
- No continuity of care across visits
- Difficulty in tracking patient progress
- Inability to share records with specialists

---

## 2. Solution Overview

Setu (Sanjeevani) AI is an Offline-First, Multilingual Voice AI Assistant designed to bridge the gap between rural patients and formal healthcare systems.

### Core Capabilities

**Voice-to-Record System**
- Listens to patient symptoms in local dialects
- Converts spoken language to structured English Medical Records
- Generates SOAP (Subjective, Objective, Assessment, Plan) format notes
- Eliminates manual data entry burden

**Smart Triage (Offline)**
- Uses on-device Small Language Models (SLMs) for analysis
- Flags "Red Alert" emergencies without internet connectivity
- Identifies critical symptoms: chest pain, severe bleeding, difficulty breathing
- Provides immediate guidance for emergency cases

**Prescription Translator**
- Scans English prescriptions using OCR technology
- Converts written instructions to voice explanations
- Delivers information in patient's native language
- Ensures proper medication adherence

---

## 3. User Stories

### ASHA Worker (Meena)
**As an ASHA worker**, I want to record patient symptoms by speaking naturally so that I can save time and focus on patient care instead of paperwork.

**Acceptance Criteria:**
- Can record consultation in local dialect
- Receives structured medical note automatically
- Can review and edit generated records
- Can flag urgent cases for immediate attention

### Patient (Raju)
**As a rural patient**, I want to understand medicine instructions in my own language so that I can take medications correctly and improve my health outcomes.

**Acceptance Criteria:**
- Can hear prescription instructions in native dialect
- Can replay instructions multiple times
- Receives clear dosage and timing information
- Understands warnings and side effects

### Doctor (Dr. Priya)
**As a consulting doctor**, I want structured patient history in standard format so that I can make quick and accurate diagnoses.

**Acceptance Criteria:**
- Receives SOAP-formatted clinical notes
- Can access patient history across visits
- Gets flagged emergency cases prioritized
- Can review original voice recordings if needed

---

## 4. Technical Stack

### Frontend
- **Flutter**: Cross-platform mobile application development
- **SQLite**: Local database for offline data storage
- **Dart**: Primary programming language

### AI/ML Components
- **OpenAI Whisper**: Automatic Speech Recognition (ASR) for voice-to-text
- **Llama-2-7b (Quantized)**: On-device Small Language Model for clinical note generation and triage
- **Meta NLLB (No Language Left Behind)**: Neural machine translation for multilingual support
- **Google ML Kit**: Optical Character Recognition (OCR) for prescription scanning

### Backend
- **Supabase**: Backend-as-a-Service platform
- **PostgreSQL**: Cloud database for data synchronization
- **RESTful APIs**: Communication between mobile app and cloud services

### Infrastructure
- **Edge Computing**: Models run locally on mobile devices
- **Sync Engine**: Opportunistic cloud synchronization when connectivity available
- **Offline-First Architecture**: Full functionality without internet access

---

## 5. Functional Requirements

### FR1: Voice Recording and Transcription
- Support for 10+ Indian languages and dialects
- Real-time voice transcription
- Noise cancellation for rural environments
- Ability to pause and resume recording

### FR2: Clinical Note Generation
- Automatic SOAP note creation from transcribed text
- Extraction of symptoms, duration, severity
- Structured data format for interoperability
- Editable output for corrections

### FR3: Offline Triage System
- Rule-based emergency detection
- SLM-powered symptom analysis
- Red/Yellow/Green alert classification
- Actionable recommendations for each category

### FR4: Prescription Translation
- Camera-based prescription capture
- OCR text extraction
- Translation to patient's language
- Text-to-speech output

### FR5: Data Synchronization
- Automatic sync when internet available
- Conflict resolution for offline edits
- Secure data transmission
- Sync status indicators

---

## 6. Non-Functional Requirements

### NFR1: Performance
- Voice transcription latency < 2 seconds
- App launch time < 3 seconds
- Triage analysis < 5 seconds
- Smooth UI with 60fps

### NFR2: Offline Capability
- 100% core functionality available offline
- Local storage for 1000+ patient records
- Model inference without internet
- Queue-based sync mechanism

### NFR3: Privacy and Security
- End-to-end encryption for synced data
- Local data encryption at rest
- Explicit consent for voice recording
- HIPAA-compliant data handling
- Data minimization principles

### NFR4: Usability
- Simple, intuitive interface for low-literacy users
- Voice-first interaction design
- Large touch targets (minimum 48x48dp)
- High contrast UI for outdoor visibility

### NFR5: Scalability
- Support for 10,000+ concurrent users
- Efficient model quantization for low-end devices
- Optimized battery consumption
- Minimal storage footprint (< 500MB)

---

## 7. Constraints and Assumptions

### Constraints
- Target devices: Android phones with 2GB+ RAM
- Model size: < 200MB for on-device SLM
- Battery usage: < 10% per hour of active use
- Network: Intermittent 2G/3G connectivity

### Assumptions
- ASHA workers have basic smartphone literacy
- Patients consent to voice recording
- Internet available at least once per week for sync
- Prescriptions are in English or Hindi

---

## 8. Success Metrics

### Adoption Metrics
- 500+ ASHA workers onboarded in pilot phase
- 5,000+ patient consultations recorded
- 80%+ daily active usage rate

### Quality Metrics
- 90%+ transcription accuracy
- 95%+ emergency detection accuracy
- 85%+ user satisfaction score

### Impact Metrics
- 50% reduction in documentation time
- 40% improvement in medication adherence
- 30% faster specialist referrals
- 70% reduction in lost medical records
