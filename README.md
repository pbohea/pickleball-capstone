# Dinkster: Computer Vision + LLM Pickleball Coaching

An end-to-end pickleball coaching application that ingests player video, analyzes technique with computer vision, and delivers personalized LLM feedback to help players improve faster.

![Project Banner](docs/images/banner.png)

---

## Table of Contents
1. Project Motivation
2. Overview of Dinkster
3. Project Workflow
4. System Architecture
5. Key Features
6. Tech Stack & Tools
7. Methodology & Implementation
8. Evaluation & Results
9. Technical Challenges
10. Market Potential & Differentiators
11. Future Work
12. Demo
13. Getting Started
14. Repository Structure
15. References
16. Acknowledgements & About Us

---

## Project Motivation
Pickleball is one of the fastest-growing sports in the U.S., but in-person coaching is limited, expensive, and inconsistent. Players frequently record themselves to self-diagnose issues, yet lack objective feedback or structured guidance.

Dinkster turns video into actionable coaching. By combining pose estimation, stroke segmentation, and LLM-based feedback, the system provides a personalized, scalable alternative to traditional coaching.

![Motivation Image](docs/images/motivation.png)

---

## Overview of Dinkster
Dinkster is a cloud-native coaching application that transforms unstructured video into structured performance insights.

The system:
- Accepts player video uploads from mobile or web
- Executes pose estimation and stroke analysis in the cloud
- Produces structured performance metrics
- Converts those metrics into personalized coaching feedback using an LLM
- Returns a simple, readable performance report to the user

At a high level, Dinkster bridges three domains:
1. Computer vision for extracting biomechanical signals
2. Structured analytics for quantifying technique
3. LLM interpretation for delivering human-readable coaching

The result is an end-to-end pipeline that feels intuitive to the user while running sophisticated analysis behind the scenes.

(include video or looped GIF of iphone screen here)

---

## Project Workflow
The workflow is intentionally designed to separate user experience from heavy computation.

1. User uploads a pickleball video
2. Video is stored in Google Cloud Storage
3. A Cloud Run job executes the YOLO Pose-26 based computer vision model and technique analysis
4. Structured output JSON is written back to cloud storage
5. The application parses the JSON and renders LLM-generated feedback

This architecture allows us to scale model execution independently from the application layer while maintaining responsiveness at the user interface.

![Workflow Diagram](docs/images/workflow.png)

---

## System Architecture
The system is divided into two primary services with clearly defined responsibilities.

### Rails Application

- Handles UI rendering
- Manages authentication and orchestration
- Initiates Cloud Run jobs
- Polls for job completion
- Retrieves and renders analysis results

### Python Model Runner

- Ingests video from cloud storage
- Executes YOLO Pose-26 keypoint detection
- Computes technique-specific metrics
- Generates structured JSON output
- Calls the OpenAI API to produce coaching feedback

This separation improves modularity, simplifies scaling, and enables independent iteration on modeling logic and application logic.


<img src="docs/images/erd_new.png" width="800" />

---

## Key Features
- **Direct-to-cloud uploads** for large video files
- **Automated pose + stroke analysis**
- **LLM feedback** with actionable coaching tips
- **Polling-based results page** with structured feedback
- **Cloud Run jobs** for scalable model execution

![Feature Highlights](docs/images/features.png)

---

## Tech Stack & Tools
- **Frontend/UI:** Rails views, iOS Swift for native Apple mobile capability
- **Backend:** Ruby on Rails, Cloud Run
- **Modeling:** Python, OpenAI API, YOLO Pose v26
- **Storage:** Google Cloud Storage
- **Deployment:** Cloud Build + Cloud Run

![Tech Stack](docs/images/tech_stack.png)

---

## Methodology & Implementation
### 1) Video Ingestion
Videos are uploaded directly to GCS using Active Storage direct uploads.

### 2) Pose + Technique Analysis
The Python model runner processes the video and extracts structured signals.

### 3) LLM Feedback
Generated analysis is converted into human-readable coaching feedback.

![Methodology Diagram](docs/images/methodology.png)

---

## Evaluation & Results
This section will include:
- Example output JSONs
- Sample coaching feedback
- Quantitative analysis of accuracy and runtime

![Results Graph](docs/images/results.png)

---

## Technical Challenges
- Large file uploads in Cloud Run
- Cross-service orchestration without background job workers
- Secure GCS access patterns
- Low-latency feedback generation

---

## Market Potential & Differentiators
Dinkster offers scalable coaching to a rapidly growing sport. Differentiators include:
- Automated, video-based coaching (not just tracking)
- LLM-generated personalized feedback
- End-to-end mobile-friendly workflow

---

## Future Work
- Multi-angle video support
- Skill benchmarking and progress tracking
- Coach marketplace integrations
- Expanded dataset for higher-quality technique classification

---

## Demo
![Demo GIF](docs/images/demo.gif)

---

## Getting Started
### Repositories
- UI Backend: https://github.com/pbohea/pickleball-rails
- iOS Frontend: https://github.com/pbohea/pickleball-ios

### Model Runner (Python)
```bash
python main.py \
  --video gs://<bucket>/path/to/video.mp4 \
  --desc "Forehand drive" \
  --start 0 \
  --output gs://<bucket>/pickleball/outputs/output.json
```

### Cloud Run Job
The model runs as a Cloud Run Job and writes output JSON to GCS:
```
gs://<bucket>/pickleball/outputs/video_<id>analysis<analysis_id>.json
```

---

## Repository Structure
```
pickleball-capstone/
  README.md
  docs/
    images/
  notebooks/
```

---

## References
- Cloud Run GPU docs: https://docs.cloud.google.com/run/docs/configuring/services/gpu
- Cloud Run CD docs: https://docs.cloud.google.com/run/docs/continuous-deployment-with-cloud-build

---

## Acknowledgements & About Us
Capstone team project. Placeholder for team names, roles, and advisor acknowledgements.

![Team Photo](docs/images/team.png)
