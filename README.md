# BirdBase: AI-Powered Avian Expert & Detection System

<div align="center">
  <img src="https://img.shields.io/badge/TÜBİTAK-2209--A-19328?style=for-the-badge&color=blue" alt="TÜBİTAK 2209-A">
  <img src="https://img.shields.io/badge/Architecture-Monorepo-brightgreen?style=for-the-badge" alt="Monorepo">
  <img src="https://img.shields.io/badge/AI-YOLOv8%20%7C%20Gemini-orange?style=for-the-badge" alt="YOLOv8 and Gemini">
  <img src="https://img.shields.io/badge/Platform-Flutter%20%7C%20FastAPI%20%7C%20Web-lightgrey?style=for-the-badge" alt="Flutter, FastAPI, Web">
</div>

## Project Overview & Live Access

**BirdBase** is an AI-assisted bird species identification and educational platform developed as a two-person graduation project at Ankara Science University. It brings together computer vision, a web interface, a Flutter mobile application, species information, and a conversational assistant in one application workflow.

Users can submit a bird image, receive an identification result, explore information about the predicted species, and ask follow-up questions. The system supports two inference approaches: an online API-backed model and a lightweight model designed for on-device/offline use.

**Live web application:** [Try BirdBase on Hugging Face](https://huggingface.co/spaces/Pianabol/BirdBase)

> **Project scope:** This repository documents the application and the research/development process. Bird identification accuracy depends on image conditions and the species involved; this is an educational prototype, not a professional ornithological verification tool.

---

## Interface Previews

<div align="center">
  <img src="Assets/App1.png" alt="BirdBase mobile app - screen 1" width="24%">
  <img src="Assets/App2.png" alt="BirdBase mobile app - screen 2" width="24%">
  <img src="Assets/App3.png" alt="BirdBase mobile app - screen 3" width="24%">
  <img src="Assets/App4.png" alt="BirdBase mobile app - screen 4" width="24%">
</div>

<br>

<div align="center">
  <img src="Assets/Web1.png" alt="BirdBase web interface - screen 1" width="48.5%">
  <img src="Assets/Web2.png" alt="BirdBase web interface - screen 2" width="48.5%">
</div>

---

## Vision & Mission

**Mission:** Make bird species identification more accessible by connecting image-based detection with biological information and an interactive learning experience.

**Vision:** Build a modular system that can be extended with improved datasets, additional species, refined mobile inference, and richer educational content. The project explores the practical trade-offs between cloud-based analysis and offline usability in the field.

---

## Key Features

### Offline Mode: On-Device Bird Recognition

- **Local inference:** The Flutter application includes a YOLOv8-compatible **TensorFlow Lite** model, loaded through `flutter_vision` for on-device analysis.
- **Connectivity independence:** A subset of the application's recognition and locally stored species-information features can work without a continuous network connection.
- **Deployment trade-off:** A smaller model reduces device-side resource requirements, but challenging species may be harder to identify than through the online pipeline.

### Online Mode: FastAPI + YOLOv8

- **Image-based detection:** The web/mobile workflow can send an image to the FastAPI backend for YOLO-based inference.
- **Structured responses:** The API prepares detected species names, annotated image output, and matching species information when available.
- **Model development:** The project explored YOLOv8n and YOLOv8s, with an emphasis on evaluating lightweight/mobile inference against stronger server-side inference.

### AI Bird Assistant & Species Information

- **Conversational learning:** Google Gemini powers a bird-related assistant for questions about species, characteristics, habitats, and behavior.
- **Species context:** The application is designed to connect recognition results with follow-up information and questions.
- **Educational content:** BirdBase combines prediction with structured information instead of presenting only a class label.

---

## From Initial Prototype to Integrated Application

BirdBase was developed across two graduation-project phases. The progression is primarily **functional and architectural**; it should not be mistaken for a numerically verified increase in model accuracy.

| Area | Initial phase — Dec 2025 | Later phase — Jun 2026 |
|---|---|---|
| Detection | Initial YOLOv8 experiments and feasibility prototype | YOLOv8n/YOLOv8s exploration and application integration |
| Data | Dataset collection, annotation, and initial cleaning | Review of visually similar species and targeted dataset refinement/fine-tuning |
| Mobile | Early mobile UI prototype | Flutter application with image selection and cloud/offline analysis workflow |
| Backend and web | Prototype / planned integration | FastAPI-based API and browser interface for image analysis |
| Learning experience | Primary focus on detecting bird species | Added species information and Gemini-assisted conversations |
| Evaluation | Initial model testing | Confusion matrices, validation samples, model-training curves, and limitations review |

### Research challenge: visually similar bird species

Fine-grained recognition became a major focus during development. The team identified **19 particularly challenging species** for targeted review and fine-tuning. This is a *difficult-species subset*, not a claim that the entire model supports only 19 species.

Examples of difficult groups included:

| Group | Example species | Main challenge |
|---|---|---|
| Corvids | Common Raven / American Crow | Similar dark plumage and body shape |
| Terns | Caspian / Forster's / Common Tern | Closely related appearance and flight profiles |
| Cormorants | Great / Double-crested Cormorant | Similar silhouettes and coloration |
| Ibises | White / Glossy / White-faced Ibis | Species-level visual distinctions |
| Aerial birds | Purple Martin / Barn Swallow / Common Swift | Similar flight poses and small subjects |
| Raptors | Golden Eagle / Bald Eagle / Harris's Hawk / Peregrine Falcon / Northern Harrier | Different species with overlapping visual cues |

The approach combined reviewing annotations, improving training examples, inspecting validation predictions, and prioritizing common confusion patterns over simply adding more images.

---

## Model Evaluation & Experimental Evidence

The original graduation report includes training curves, confusion matrices, and visual validation batches. The figures below are **extracted from Section 9 of the project report** and show what was examined during development.

### YOLOv8n: training and validation curves

![YOLOv8n training curves from the graduation report](docs/results/yolov8n_training_curves.png)

This figure tracks box/classification/distribution-focal losses alongside precision, recall, mAP@0.50, and mAP@0.50–0.95 across the recorded training run. The chart visually suggests improving detection metrics during that run. **Exact final metrics should be taken from the original `results.csv`, not estimated from this screenshot.**

<details>
<summary><strong>YOLOv8n normalized confusion matrix</strong></summary>

![YOLOv8n normalized confusion matrix](docs/results/yolov8n_confusion_matrix_normalized.png)

This figure contains a large set of class IDs and should be interpreted with the exact label mapping and evaluation split used in the corresponding run.

</details>

<details>
<summary><strong>Validation examples: ground-truth labels vs. predictions</strong></summary>

**Ground-truth annotations:**

![Validation batch 0 ground truth](docs/results/validation_batch0_labels.jpg)

**Model predictions:**

![Validation batch 0 predictions](docs/results/validation_batch0_predictions.jpg)

These examples expose both successful localization and **species-level misclassifications**. They are provided as error-analysis evidence, not as a showcase of perfect predictions.

</details>

### How to interpret the comparison

- YOLOv8n was explored for lighter device-side inference; YOLOv8s was explored for the backend-oriented pipeline.
- The available report does **not** provide a complete, like-for-like numeric benchmark for both models on a verified identical evaluation split.
- Some YOLOv8s evaluation plots in the project report show near-zero aggregate results. They should be rechecked against the relevant checkpoint, dataset configuration, label mapping, and raw validation logs before publishing a strong quantitative claim.
- We therefore **do not claim an exact percentage improvement or a validated final accuracy advantage** for one model over the other in this README.

### Known limitations

- Fine-grained species recognition is sensitive to visually similar classes, occlusion, unusual camera angles, blur, and image quality.
- Dataset imbalance and changing class definitions can make comparisons between training runs misleading.
- Offline inference offers portability, but a compact model has different speed/accuracy trade-offs from a backend model.
- Conversational answers can be incorrect; the assistant should not be treated as a verified biological reference.

---

## System Architecture (Monorepo)

BirdBase separates its **mobile client**, **browser interface**, and **Python API/inference backend** in a single repository. The codebase is organized into these top-level components:

| Directory | Responsibility |
|---|---|
| [`BirdBaseApp/`](BirdBaseApp/) | Flutter/Dart mobile app, navigation screens, local TFLite model usage, and local bird data |
| [`BirdBase_Backend/`](BirdBase_Backend/) | FastAPI endpoints, model inference, image processing, and Gemini API calls |
| [`WEB/`](WEB/) | HTML/CSS/JavaScript browser interface for image upload, analysis, and exploration |
| [`Assets/`](Assets/) | Mobile and web screenshots used in the project documentation |
| [`docs/results/`](docs/results/) | Selected experimental figures extracted from the graduation report (added for portfolio documentation) |

**Online flow:** Image upload → Web/Flutter client → FastAPI endpoint → YOLO inference → Species result and optional biological data → Client display.

**Offline flow:** Local image → Flutter app → TFLite inference → Local labels/species information → Client display.

The codebase is a graduation-project implementation rather than a production-scale, independently deployed microservice platform. The main API routes currently share a Python application module.

---

## Technology Stack

- **Computer vision:** Python, PyTorch, Ultralytics YOLOv8n/YOLOv8s, TensorFlow Lite
- **Backend:** Python, FastAPI, Uvicorn, Pillow
- **Mobile:** Flutter, Dart, `flutter_vision`
- **Web:** HTML, CSS, JavaScript
- **Conversational AI:** Google Gemini API
- **Data / experiments:** YOLO annotations, JSON-based species information, validation batches, confusion matrices, training metrics
- **Workflow:** Git/GitHub, dataset preprocessing, iterative experiments, model evaluation

---

## Future Work

- Validate YOLOv8n and YOLOv8s checkpoints using the **same documented data split** and retain each run's `results.csv`.
- Expand and balance difficult-species samples, then perform per-class error analysis.
- Improve mobile/offline evaluation on real devices and test more challenging field images.
- Strengthen chatbot grounding with reviewed biological sources.
- Improve exception handling, test coverage, and release documentation.

---

## Contributors & Acknowledgments

BirdBase was developed as a **two-person graduation project** by:

- **Furkan Tuç**
- **Nisa Doğa Yücel**

The project involved shared planning and technical decisions covering data preparation, model training, evaluation, backend integration, user interfaces, and application testing. The Git commit history reflects how the shared repository was uploaded; it is not a complete record of individual contributions.

**Academic advisor:** Assist. Prof. Yüksel Arslan, Ankara Science University.

**Project support:** TÜBİTAK 2209-A University Students Research Projects Support Program, as acknowledged in the original project repository.

---

## Further Information

- **Live demo:** [BirdBase on Hugging Face](https://huggingface.co/spaces/Pianabol/BirdBase)
- **Experimental sources:** Graduation Project II report, Section 9 (Implementation and Results, June 2026)

*BirdBase is an academic prototype intended for experimentation and education. Results shown here should be read with their stated evaluation limitations.*
