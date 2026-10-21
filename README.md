<h2 align="center"><b>multimodal-traffic-inference</b></h2>

<h4 align="center">Distributed multi-agent visual reasoning and automated transit enforcement platform</h4>

<p align="center">
    <a href="https://github.com/yuvanvishnupandi/multimodal-traffic-inference"><img src="https://img.shields.io/badge/GitHub-multimodal--traffic--inference-181717?logo=github&style=flat-square" alt="GitHub Repo"></a>
    <a href="https://margarakshak-xi.vercel.app/"><img src="https://img.shields.io/badge/Live%20Demo-Vercel-success?logo=vercel&style=flat-square" alt="Live Demo"></a>
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" alt="MIT License"></a>
    <img src="https://img.shields.io/badge/Python-3.11+-3776AB.svg?logo=python&logoColor=white&style=flat-square" alt="Python 3.11+">
    <img src="https://img.shields.io/badge/FastAPI-0.110+-009688.svg?logo=fastapi&logoColor=white&style=flat-square" alt="FastAPI">
    <img src="https://img.shields.io/badge/LangGraph-Multi--Agent-FF4F00.svg?logo=langchain&logoColor=white&style=flat-square" alt="LangGraph">
    <img src="https://img.shields.io/badge/Google-Gemini_2.5_Flash-8E75B2.svg?logo=google&logoColor=white&style=flat-square" alt="Gemini AI">
    <img src="https://img.shields.io/badge/Node.js-18+-339933.svg?logo=nodedotjs&logoColor=white&style=flat-square" alt="Node.js">
    <img src="https://img.shields.io/badge/Express-4.19+-000000.svg?logo=express&logoColor=white&style=flat-square" alt="Express">
    <img src="https://img.shields.io/badge/MySQL-8.0-4479A1.svg?logo=mysql&logoColor=white&style=flat-square" alt="MySQL 8.0">
    <img src="https://img.shields.io/badge/React-18.2-61DAFB.svg?logo=react&logoColor=black&style=flat-square" alt="React 18">
    <img src="https://img.shields.io/badge/TailwindCSS-3.3-38BDF8.svg?logo=tailwindcss&logoColor=white&style=flat-square" alt="Tailwind CSS">
</p>

---

Implementation of a distributed multi-agent vision-language architecture for automated traffic violation detection, spatial fraud mitigation, and statutory penalty adjudication. Powered by LangGraph, Google Gemini Vision, and FastAPI, coupled with a Node.js Express backend, MySQL 8.0 relational schema, and React frontend.

The platform processes crowdsourced incident evidence, triangulates spatial authenticity, performs optical recognition on vehicle license plates, identifies regulatory infractions under the Motor Vehicles Act, and provides police officers with an authoritative adjudication console for rapid 1-click verification.

---

## Architecture

The system decouples user interaction, core business logic, relational storage, and multi-agent artificial intelligence into modular, independently scalable tiers:

```mermaid
flowchart TD
    subgraph Client["Client Presentation Layer"]
        C1["Citizen Reporting Portal<br/>(React 18 + Vite + Tailwind)"]
        C2["Police Adjudication Console<br/>(React 18 + Leaflet Heatmaps)"]
    end

    subgraph Gateway["Application Services Layer (Node.js)"]
        API["Express.js REST Gateway"]
        AUTH["JWT Authentication & RBAC Engine"]
        QUEUE["Evidence Ingestion & Report Router"]
    end

    subgraph AgenticEngine["AI Microservice (FastAPI + LangGraph)"]
        ORCH["StateGraph Orchestrator"]
        AF["Geospatial Anti-Fraud Agent<br/>(EXIF vs GPS Triangulation)"]
        VIS["Vision & OCR Triage Agent<br/>(Gemini 2.5 Flash Multimodal)"]
        RULE["Statutory Penalty Agent<br/>(Motor Vehicles Act 1988)"]
        HOT["Spatial Hotspot Agent<br/>(Density Clustering Engine)"]
        CHAT["AskRakshak Assistant<br/>(Conversational Legal Copilot)"]
    end

    subgraph Storage["Data & External Services Layer"]
        DB[("MySQL 8.0 Relational DB")]
        NOM["Nominatim Reverse Geocoding API"]
        GEM["Google Gemini API Cluster"]
    end

    C1 & C2 --> Gateway
    Gateway --> AUTH
    AUTH --> QUEUE
    QUEUE --> DB
    QUEUE --> ORCH

    ORCH --> AF
    AF --> NOM
    AF --> VIS
    VIS --> GEM
    VIS --> RULE
    RULE --> QUEUE

    C1 --> CHAT
    CHAT --> GEM
    QUEUE --> HOT
```

---

## Multi-Agent Execution Graph

The core inference engine executes as a stateful, cyclic directed graph using LangGraph. Each specialized agent inspects, modifies, and enriches the shared `AgentState` before routing to subsequent nodes:

```mermaid
flowchart TD
    START([Incident Evidence Submitted]) --> AF["Geospatial Anti-Fraud & Routing Node<br/>• IP Geolocation vs Device EXIF GPS<br/>• Reverse Geocoding via Nominatim<br/>• Jurisdiction ID Assignment"]

    AF --> COND1{"Anti-Fraud Check"}

    COND1 -- "GPS Mismatch / Spoof Detected" --> FRAUD["Flag Report as Fraudulent<br/>• Record Spoof Reason<br/>• Terminate Pipeline"]
    COND1 -- "Location Authentic" --> VIS["Single-Plate OCR & Vision Triage Node<br/>• Multimodal Reasoning via Gemini 2.5 Flash<br/>• Offending Vehicle Classification<br/>• License Plate String Extraction<br/>• Confidence Metric Computation"]

    VIS --> COND2{"OCR & Violation Check"}

    COND2 -- "No Plate or Obscured" --> REJECT["Mark Inconclusive<br/>• Route to Manual Investigation"]
    COND2 -- "Valid Plate & Infraction" --> PEN["Entity Verification & Penalty Calculation Node<br/>• Motor Vehicles Act Violation Mapping<br/>• Statutory Penalty & Fine Calculation<br/>• Challan Payload Generation"]

    PEN --> END([Ready for Authoritative Police Review])
    FRAUD --> END
    REJECT --> END
```

### Agent Specialization Matrix

| Agent | Technology | Primary Function | Failure Mode / Fallback |
|:---|:---|:---|:---|
| **Geospatial Anti-Fraud** | Geopy / Nominatim | Cross-validates device hardware GPS coordinates against client IP geolocation with 50km tolerance | Bypasses network check if GPS metadata unavailable |
| **Vision & OCR Triage** | Google Gemini 2.5 Flash | Isolates single vehicle, reads license plate alphanumeric characters, detects violation type | Cascades: `gemini-2.5-flash` → `gemini-2.0-flash` → `gemini-flash-latest` |
| **Statutory Rule Engine** | Python / SQL Rules | Maps detected offense string to codified Indian Motor Vehicles Act sections and calculates standard fine amounts | Applies baseline general safety fine if specific section unmatched |
| **AskRakshak Assistant** | LangChain / Multi-LLM | Conversational copilot answering citizen legal inquiries and providing challan contest guidance | Fallback chain: Google Gemini → OpenAI GPT-4o-mini → Mistral AI |
| **Hotspot Prediction** | Scikit-Learn / Leaflet | Aggregates geographic report clusters to project traffic violation density heatmaps | Returns global regional hotspot coordinates if density under threshold |

---

## Traffic Violation Enforcement Workflow

The complete lifecycle from citizen crowdsourced evidence capture to official fine issuance and penalty settlement:

```mermaid
sequenceDiagram
    autonumber
    actor Citizen
    participant Client as Web Portal
    participant Server as Express Server
    participant DB as MySQL Database
    participant AI as LangGraph AI Engine
    actor Police as Police Officer

    Citizen->>Client: Capture & upload violation evidence
    Client->>Server: POST /api/reports (image + GPS coordinates)
    Server->>DB: Store pending report record
    Server->>AI: POST /api/ai/process-evidence
    AI->>AI: Anti-Fraud: Validate EXIF GPS vs IP
    AI->>AI: Vision: OCR plate & classify violation (Gemini)
    AI->>AI: Legal: Map penalty under Motor Vehicles Act
    AI-->>Server: Return validation results & draft challan
    Server->>DB: Update report with AI triage telemetry
    Police->>Client: Open Adjudication Review Queue
    Client->>Server: Fetch pending reports with AI confidence
    Police->>Client: 1-Click Approve / Reject
    Client->>Server: PUT /api/police/reports/:id/verify
    Server->>DB: Issue official e-challan & award trust score
    Server-->>Citizen: Dispatch fine notification & SMS receipt
```

---


## Install

### Prerequisites

- Python 3.11+
- Node.js 18+ and npm
- MySQL Server 8.0+
- Google Gemini API Key

### Repository Setup

```bash
# Clone the repository
$ git clone https://github.com/yuvanvishnupandi/multimodal-traffic-inference.git
$ cd multimodal-traffic-inference

# 1. AI Microservice Environment
$ cd ai_service
$ python -m venv venv
$ source venv/bin/activate  # On Windows: .\venv\Scripts\activate
$ pip install -r requirements.txt

# 2. Node.js Backend Gateway
$ cd ../backend
$ npm install

# 3. React Frontend Dashboard
$ cd ../frontend
$ npm install
```

### Database Initialization

```bash
$ mysql -u root -p -e "CREATE DATABASE traffic_violation_db;"
$ mysql -u root -p traffic_violation_db < backend/database_schema.sql
```

### Environment Configuration

Configure `ai_service/.env`:

```env
GEMINI_API_KEY=your_gemini_api_key
OPENAI_API_KEY=your_openai_api_key
MISTRAL_API_KEY=your_mistral_api_key
```

Configure `backend/.env`:

```env
PORT=5000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=traffic_violation_db
JWT_SECRET=your_jwt_secret_hash
AI_SERVICE_URL=http://localhost:8000
```

---

## Usage

### Programmatic Multi-Agent Pipeline

Invoke the LangGraph state machine directly from Python:

```python
from ai_service.models import EvidencePayload, LocationData
from ai_service.agents import build_graph

# Compile the multi-agent graph
graph = build_graph()

# Construct report evidence payload with spatial telemetry
payload = EvidencePayload(
    user_id = "citizen_4021",
    image_url = "https://example.com/incident_evidence.jpg",
    device_gps = LocationData(lat = 12.9716, lng = 77.5946),
    ip_address = "106.51.72.10"
)

# Run end-to-end multi-agent evaluation
state = graph.invoke({
    "agent_state": {
        "payload": payload
    }
})

result = state["agent_state"]

print(f"Anti-Fraud Flag:    {result.is_fraud}")
print(f"Jurisdiction:       {result.jurisdiction_id}")
print(f"Detected Plate:     {result.vision_validation.extracted_plate}")
print(f"Violation:          {result.vision_validation.violation_detected}")
print(f"Vision Confidence:  {result.vision_validation.confidence_score}%")
print(f"Assessed Penalty:   ₹{result.final_challan.fine_amount}")
```

### Launching System Services

Run all services simultaneously using the provided Windows script:

```cmd
start.bat
```

Or launch services independently:

```bash
# Terminal 1 — AI Microservice (FastAPI)
$ cd ai_service
$ uvicorn main:app --host 0.0.0.0 --port 8000 --reload

# Terminal 2 — Application Gateway (Express)
$ cd backend
$ npm start

# Terminal 3 — Interactive Client (Vite)
$ cd frontend
$ npm run dev
```

### API Ingestion Endpoints

#### 1. Submit Evidence for Autonomous Multi-Agent Triage

```bash
$ curl -X POST "http://localhost:8000/api/ai/process-evidence" \
    -H "Content-Type: application/json" \
    -d '{
        "user_id": "citizen_4021",
        "image_url": "https://example.com/evidence.jpg",
        "device_gps": {"lat": 12.9716, "lng": 77.5946},
        "ip_address": "106.51.72.10"
    }'
```

```json
{
  "status": "success",
  "is_fraud": false,
  "fraud_reason": null,
  "vision_validation": {
    "is_valid_submission": true,
    "license_plate_found": true,
    "extracted_plate": "TN01AB1234",
    "violation_detected": "Driving without a Helmet",
    "confidence_score": 96,
    "auto_approve": true
  },
  "challan": {
    "challan_id": "CH-928174",
    "plate_number": "TN01AB1234",
    "violation_type": "Driving without a Helmet",
    "fine_amount": 1000.0,
    "currency": "INR",
    "jurisdiction_id": "JURISDICTION_BANGALORE_URBAN",
    "status": "ISSUED"
  },
  "jurisdiction_id": "JURISDICTION_BANGALORE_URBAN"
}
```

#### 2. AskRakshak Legal Copilot Query

```bash
$ curl -X POST "http://localhost:8000/api/ai/chat" \
    -H "Content-Type: application/json" \
    -d '{
        "message": "I received a red light challan at 10:14 AM on Anna Salai but my vehicle was parked.",
        "mode": "citizen",
        "current_path": "/dispute"
    }'
```

#### 3. Spatial Hazard & Hotspot Density Extraction

```bash
$ curl -X GET "http://localhost:8000/api/ai/heatmap"
```

---

## Citations

```bibtex
@article{team2023gemini,
    title   = {Gemini: A Family of Highly Capable Multimodal Models},
    author  = {Gemini Team and Google},
    journal = {arXiv preprint arXiv:2312.11805},
    year    = {2023}
}
```

```bibtex
@misc{langgraph2024,
    title   = {LangGraph: Building Stateful Multi-Agent Applications with LLMs},
    author  = {LangChain Team},
    year    = {2024},
    url     = {https://github.com/langchain-ai/langgraph}
}
```

```bibtex
@article{dosovitskiy2020image,
    title   = {An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale},
    author  = {Alexey Dosovitskiy and Lucas Beyer and Alexander Kolesnikov and Dirk Weissenborn and Xiaohua Zhai and Thomas Unterthiner and Mostafa Dehghani and Matthias Minderer and Georg Heigold and Sylvain Gelly and Jakob Uszkoreit and Neil Houlsby},
    journal = {Proceedings of the International Conference on Learning Representations (ICLR)},
    year    = {2021}
}
```

---

## License

[MIT](LICENSE)

