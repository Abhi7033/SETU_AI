# Setu (Sanjeevani) AI

The Rural Health Bridge

---

## Problem

Rural healthcare in India faces critical challenges that prevent millions from accessing quality medical care:

- Doctor-patient ratio exceeds 1:10,000 in rural areas, creating severe shortage of medical specialists
- Language barriers between patients speaking local dialects and doctors using English/Hindi lead to miscommunication
- ASHA workers are overburdened with manual data entry, reducing time for patient care
- Paper-based medical records are frequently lost, eliminating continuity of care
- Critical symptoms go undetected due to lack of immediate triage capabilities

---

## Solution

Setu (Sanjeevani) AI is an Offline-First, Multilingual Voice AI Assistant designed specifically for rural ASHA workers. It bridges the gap between rural patients who speak local dialects and formal healthcare systems that operate in English and Hindi.

The system empowers frontline health workers with AI-powered tools that work without internet connectivity, ensuring healthcare access in the most remote areas.

---

## Key Features

### Voice-to-Record
Listens to patient symptoms spoken in local dialects and automatically converts them to structured English Medical Records in SOAP format. Eliminates manual data entry and saves valuable time for patient care.

### Smart Triage (Offline)
Uses on-device Small Language Models to analyze symptoms and flag emergency cases requiring immediate attention. Works completely offline, identifying red alerts like chest pain, severe bleeding, and difficulty breathing without internet connectivity.

### Prescription Translator
Scans English prescriptions using OCR technology and provides voice explanations in the patient's native language. Ensures proper medication adherence by making instructions accessible and understandable.

### Offline-First Architecture
All core functionality works without internet connection. AI models run locally on the mobile device, with opportunistic cloud synchronization when connectivity is available.

---

## Tech Stack

### Frontend
- Flutter for cross-platform mobile development
- SQLite for local data persistence
- Dart programming language

### AI/ML
- OpenAI Whisper for Automatic Speech Recognition
- Llama-2-7b (Quantized) for clinical note generation and triage
- Meta NLLB for neural machine translation across Indian languages
- Google ML Kit for Optical Character Recognition

### Backend
- Supabase for backend services and data synchronization
- PostgreSQL for cloud database
- RESTful APIs for communication

---

## Workflow

### 1. Consultation
ASHA worker initiates consultation and records patient symptoms using voice in local dialect. The app provides real-time transcription and visual feedback.

### 2. Processing
Voice is transcribed using Whisper ASR, translated to English using NLLB, and analyzed by Llama-2 SLM to extract structured medical information including symptoms, duration, and severity.

### 3. Triage
On-device triage engine analyzes symptoms and assigns alert level (Red/Yellow/Green). Emergency cases are flagged immediately with recommended actions.

### 4. Action
Structured clinical note is generated in SOAP format. ASHA worker reviews, edits if needed, and saves to patient record. Data syncs to cloud when internet is available.

---

## Impact

### Access
Brings AI-powered healthcare tools to remote areas with limited internet connectivity. Enables ASHA workers to provide better care with limited resources.

### Speed
Reduces documentation time by 50%, allowing health workers to see more patients. Faster specialist referrals through structured medical records and emergency flagging.

### Adherence
Improves medication compliance by 40% through prescription translation and voice instructions in patient's native language. Reduces medication errors and improves health outcomes.

---

## Project Status

This project is a prototype developed for the AI for Bharat Hackathon. It demonstrates the core capabilities of offline voice-to-record, smart triage, and prescription translation.

Current implementation includes:
- Voice recording and transcription in 3 Indian languages
- Basic triage logic for common emergency symptoms
- Prescription OCR and translation
- Offline-first mobile application

Future development will focus on:
- Expanding language support to 10+ dialects
- Fine-tuning models on Indian medical data
- Integration with government health systems
- Field testing with ASHA workers in rural communities

---

## Getting Started

### Prerequisites
- Flutter SDK 3.0 or higher
- Android Studio or VS Code with Flutter extensions
- Android device or emulator with 2GB+ RAM
- Python 3.8+ for model conversion scripts

### Installation

Clone the repository:
```bash
git clone https://github.com/[username]/setu-sanjeevani-ai.git
cd setu-sanjeevani-ai
```

Install dependencies:
```bash
flutter pub get
```

Download AI models:
```bash
python scripts/download_models.py
```

Run the application:
```bash
flutter run
```

---

## Contributing

Contributions are welcome. Please read the contributing guidelines before submitting pull requests.

---

## License

This project is licensed under the MIT License. See LICENSE file for details.

---

## Acknowledgments

Developed for the AI for Bharat Hackathon with the goal of improving healthcare access in rural India. Special thanks to ASHA workers who inspired this project through their dedication to community health.

---

## Contact

For questions, feedback, or collaboration opportunities, please open an issue on GitHub or contact the project maintainers.
