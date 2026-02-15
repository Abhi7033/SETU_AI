# Design Document

## Setu (Sanjeevani) AI - Rural Health Bridge

---

## 1. System Architecture

### Architecture Philosophy: Mobile-First, Edge-AI

The system is designed with an offline-first, edge-computing approach where AI models run locally on mobile devices, ensuring functionality in areas with limited or no internet connectivity.

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    EDGE CLIENT (Mobile)                  │
│  ┌────────────────────────────────────────────────────┐ │
│  │              Flutter Application                    │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐ │ │
│  │  │   UI     │  │ Business │  │   Local Models   │ │ │
│  │  │  Layer   │  │  Logic   │  │  - Whisper ASR   │ │ │
│  │  │          │  │          │  │  - Llama-2 SLM   │ │ │
│  │  │          │  │          │  │  - NLLB Trans.   │ │ │
│  │  └──────────┘  └──────────┘  └──────────────────┘ │ │
│  │                                                      │ │
│  │  ┌──────────────────────────────────────────────┐  │ │
│  │  │         Local Storage (SQLite)                │  │ │
│  │  │  - Patient Records  - Voice Files             │  │ │
│  │  │  - Clinical Notes   - Sync Queue              │  │ │
│  │  └──────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
                            │
                            │ Opportunistic Sync
                            │ (When Internet Available)
                            ▼
┌─────────────────────────────────────────────────────────┐
│                  CLOUD BRIDGE (Backend)                  │
│  ┌────────────────────────────────────────────────────┐ │
│  │                  Supabase Platform                  │ │
│  │  ┌──────────────┐  ┌──────────────┐               │ │
│  │  │  PostgreSQL  │  │  REST APIs   │               │ │
│  │  │   Database   │  │  Auth Service│               │ │
│  │  └──────────────┘  └──────────────┘               │ │
│  │  ┌──────────────┐  ┌──────────────┐               │ │
│  │  │ File Storage │  │ Sync Engine  │               │ │
│  │  │ (Voice/Docs) │  │              │               │ │
│  │  └──────────────┘  └──────────────┘               │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

### Component Breakdown

**Edge Client (Mobile Application)**
- Flutter-based cross-platform mobile app
- Embedded AI models for offline inference
- SQLite database for local persistence
- Sync queue for offline-to-online transitions

**Cloud Bridge (Sync Server)**
- Supabase backend for data synchronization
- PostgreSQL for structured data storage
- Object storage for voice recordings and documents
- Authentication and authorization services

---

## 2. Data Flow

### End-to-End Processing Pipeline

```
INPUT (Voice in Dialect)
    │
    ▼
┌─────────────────────────────────┐
│  Automatic Speech Recognition   │
│  (OpenAI Whisper)                │
│  Output: Raw Text in Dialect    │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Neural Machine Translation     │
│  (Meta NLLB)                     │
│  Output: English Text            │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Symptom Extraction & Analysis  │
│  (Llama-2-7b Quantized SLM)     │
│  Output: Structured Data         │
│  - Chief Complaint               │
│  - Symptoms List                 │
│  - Duration & Severity           │
│  - Relevant History              │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Triage Logic Engine            │
│  (Rule-Based + SLM)              │
│  Check for Red Flags:            │
│  - Chest pain                    │
│  - Severe bleeding               │
│  - Difficulty breathing          │
│  - Loss of consciousness         │
│  - High fever in children        │
└─────────────────────────────────┘
    │
    ▼
OUTPUT
├─ Alert Level (Red/Yellow/Green)
├─ Clinical Note (SOAP Format)
├─ Recommended Actions
└─ Referral Guidance
```

### Prescription Translation Flow

```
INPUT (Prescription Image)
    │
    ▼
┌─────────────────────────────────┐
│  Optical Character Recognition  │
│  (Google ML Kit)                 │
│  Output: Extracted Text          │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Text Parsing & Structuring     │
│  Extract:                        │
│  - Medicine Names                │
│  - Dosage                        │
│  - Frequency                     │
│  - Duration                      │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Translation to Local Language  │
│  (Meta NLLB)                     │
│  Output: Translated Instructions │
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│  Text-to-Speech Synthesis       │
│  Output: Audio in Patient's      │
│          Native Language         │
└─────────────────────────────────┘
    │
    ▼
OUTPUT (Translated Audio Player)
```

---

## 3. UI/UX Conceptualization

### Design Philosophy: "Thumb-Friendly, Voice-First, Visual"

The interface is designed for users with varying literacy levels, optimized for one-handed use, and prioritizes voice interaction over text input.

### Key Screens and Interactions

#### 3.1 Consultation Mode

**Layout:**
- Large circular microphone button (center, 120dp diameter)
- Live transcript display (scrollable, large font 18sp)
- Language selector (top-right corner)
- Patient info card (top, collapsible)

**Interaction Flow:**
1. Tap mic button to start recording
2. Speak naturally in local dialect
3. See live transcription appear
4. Tap again to stop recording
5. Review auto-generated summary card
6. Edit if needed, then save

**Visual Design:**
- High contrast colors (dark text on light background)
- Pulsing animation on mic button during recording
- Waveform visualization for audio feedback
- Clear visual indicators for recording state

#### 3.2 Auto-Summary Card

**Components:**
- Chief Complaint (bold, 20sp)
- Symptoms list (bullet points)
- Triage alert badge (color-coded)
- Action buttons: Edit, Save, Share

**Alert Color Coding:**
- Red: Emergency (requires immediate attention)
- Yellow: Urgent (needs same-day care)
- Green: Routine (standard follow-up)

#### 3.3 Prescription Reader

**Layout:**
- Camera viewfinder (full screen)
- Capture button (bottom center)
- Flash toggle (top-right)
- Gallery import option (bottom-left)

**Post-Capture Flow:**
1. Image preview with crop/rotate tools
2. OCR processing indicator
3. Extracted text display (editable)
4. Translation preview
5. Audio player with playback controls
6. Save to patient record option

**Audio Player Features:**
- Large play/pause button
- Progress bar with time indicators
- Playback speed control (0.5x, 1x, 1.5x)
- Repeat button for medication instructions

#### 3.4 Patient Dashboard

**Layout:**
- Search bar (top)
- Patient list (scrollable cards)
- Quick action FAB (bottom-right)
- Filter/sort options (top-right)

**Patient Card:**
- Name and age (large text)
- Last visit date
- Alert indicator (if any)
- Quick actions: Call, View History, New Consultation

#### 3.5 Settings and Sync

**Options:**
- Language preferences
- Voice recording quality
- Auto-sync settings
- Storage management
- Privacy controls

**Sync Status Indicator:**
- Green: All synced
- Orange: Pending sync (with count)
- Red: Sync error
- Gray: Offline mode

### Accessibility Features

- Voice navigation support
- High contrast mode
- Large text option (up to 24sp)
- Haptic feedback for key actions
- Screen reader compatibility

---

## 4. Privacy & Ethics

### Data Privacy Principles

**Explicit Consent**
- Voice recording consent obtained before each consultation
- Clear explanation of data usage in local language
- Option to delete recordings after transcription
- Consent logged and auditable

**Data Minimization**
- Collect only essential medical information
- No collection of caste, religion, or socioeconomic data
- Automatic deletion of voice files after 30 days (configurable)
- Anonymization for analytics and research

**Security Measures**
- AES-256 encryption for data at rest
- TLS 1.3 for data in transit
- Biometric authentication for app access
- Automatic session timeout after 15 minutes
- Secure key storage using platform keychain

### Ethical Considerations

**Transparency**
- Clear disclosure that AI is assisting, not replacing doctors
- Explanation of triage logic and limitations
- No false promises about diagnostic accuracy
- Open about data sharing with healthcare providers

**Fairness and Bias**
- Models tested across diverse dialects and accents
- Regular audits for demographic bias
- Inclusive training data from multiple regions
- Feedback mechanism for incorrect translations

**Accountability**
- Audit logs for all clinical decisions
- Human oversight for emergency cases
- Clear escalation paths for AI failures
- Incident reporting and review process

**Patient Rights**
- Right to access their data
- Right to correct inaccurate information
- Right to delete their records
- Right to opt-out of data sharing

### Compliance

- Adherence to India's Digital Personal Data Protection Act
- HIPAA-compliant data handling practices
- Regular security audits and penetration testing
- Data residency in India for sensitive health information

---

## 5. Technical Design Decisions

### Model Selection Rationale

**Whisper (ASR)**
- Multilingual support out-of-the-box
- Robust to background noise
- Quantized version fits on mobile devices
- Open-source and customizable

**Llama-2-7b (SLM)**
- Efficient 4-bit quantization for mobile
- Strong reasoning capabilities
- Can be fine-tuned on medical data
- Runs offline on 2GB+ RAM devices

**NLLB (Translation)**
- Supports 200+ languages including Indian dialects
- Better than Google Translate for low-resource languages
- Optimized mobile version available
- Preserves medical terminology

### Database Schema Design

**Patients Table**
- patient_id (UUID, primary key)
- name, age, gender
- village, district
- contact_number
- created_at, updated_at

**Consultations Table**
- consultation_id (UUID, primary key)
- patient_id (foreign key)
- asha_worker_id (foreign key)
- voice_file_path
- transcript_text
- translated_text
- clinical_note (JSONB)
- triage_level (enum: red/yellow/green)
- consultation_date
- synced_at

**Prescriptions Table**
- prescription_id (UUID, primary key)
- consultation_id (foreign key)
- image_path
- extracted_text
- translated_audio_path
- created_at

### Offline-First Sync Strategy

**Conflict Resolution:**
- Last-write-wins for patient demographics
- Append-only for consultations (no conflicts)
- Manual review for prescription edits

**Sync Priority:**
- Emergency cases (red alerts) synced first
- Recent consultations next
- Historical data last
- Voice files synced on WiFi only (optional)

---

## 6. Performance Optimization

### Model Optimization
- 4-bit quantization for Llama-2 (reduces size by 75%)
- Pruning and distillation for Whisper
- Caching of common translations
- Lazy loading of models (load on first use)

### App Performance
- Image compression for prescriptions
- Pagination for patient lists
- Background sync using WorkManager
- Efficient SQLite queries with indexing

### Battery Optimization
- Batch processing of voice recordings
- Reduce model inference frequency
- Use low-power sensors for activity detection
- Optimize wake locks and background tasks

---

## 7. Future Enhancements

- Integration with government health portals (ABDM)
- Telemedicine video consultation support
- Predictive analytics for disease outbreaks
- Medication inventory management
- Multi-device sync for tablets and desktops
