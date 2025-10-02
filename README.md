# CMPE272-Team10-Project-Ideas

# Idea 1: # Agentic AI Skin Health Assistant

## Problem
People often deal with everyday skin concerns like new moles, pimples, rashes, sunburns, or small growths, but find it hard to track them, understand if they’re normal, or decide what simple steps (hydration, sunscreen, OTC creams) could help. Existing apps are either static galleries or symptom checkers, without personalized, proactive insights or product guidance.

---

## Project Idea
Build a platform that acts as a personal skin tracking and coaching assistant. It analyzes real skin condition images from open datasets to detect and classify common conditions, tracks changes over time, and produces educational reports with safe, consumer-friendly product recommendations (e.g., moisturizers, sunscreens, acne spot treatments).

> Disclaimer: Educational use only, not a diagnostic tool.  
> All recommendations are general wellness/product suggestions, not medical treatment.

---

## Data Source
Use real dermatology and skin condition datasets:
- SCIN (Skin Condition Image Network) – ~10,000+ crowd-sourced images across many conditions (rashes, acne, etc.) with self-reported metadata. [Google Research](https://github.com/google-research-datasets/scin)  
- Derm7pt / DermNet Atlas – dermatology image sets including acne, eczema, psoriasis, sunburn-like erythema, etc.  
- Diverse Dermatology Images (DDI) – ensures fairness across skin tones.  
- SkinCAP – skin images with captions/annotations, useful for linking visual → description.  

---

## Apache Kafka
- Images are ingested into Kafka topics as events.  
- Streams images and metadata to agents in real time.  
- Decouples ingestion (upload or dataset feed) from processing pipeline.  

---

## Agentic Pipeline

### Intake & Quality Agent
- Ensures the photos aren't blurry, too dark/bright.  
- Provides retake suggestions (“move closer”, “avoid shadows”, "add light").  
- Images can also include personal context → apply face/background blurring for privacy.  

### Detection & Classification Agent
- Train CV models on SCIN/DDI/SkinCAP datasets.  
- Classifies common conditions: pimple, rash, sunburn, mole, benign growth.  

### Feature Extraction & Tracking Agent
- Extracts measurable features (size, redness intensity, border spread).  
- Matches across timepoints → tracks whether a pimple reduced, rash expanded, or mole grew.  

### Coach/Recommendation Agent (LLM)
- Generates explainable reports in plain English.  
- Uses transparent thresholds (e.g., redness intensity ↑ over 5 days = “Monitor closely”).  
- Outputs categories: No concerning change, Monitor progression, Seek dermatologist if persistent.  
- Suggests safe, OTC-friendly product categories:
  - Pimples: salicylic acid spot treatments, gentle cleansers.  
  - Sunburn: aloe vera gel, mineral vs. chemical sunscreens.  
  - Acne: benzoyl peroxide or non-comedogenic moisturizers.  
  - Rashes: soothing moisturizers, fragrance-free lotions.  
  - Moles/Growths: recommend monitoring only + dermatologist consult if growth/irregularity is seen.  
- Provides lifestyle tips: hydration, sunscreen use, gentle cleansing.  

---

## Data Lake
- AWS S3 to store raw images, classification labels, features, and longitudinal reports.  
- Use Parquet tables for structured features (e.g., redness %, size mm, trend history).  

---

## Containerization & Load Balancing
- Each agent = Using an Agentic framework like Strands Agents, LangChain etc as a microservice in Docker.  
- Deploy on Kubernetes or Docker Compose.  
- FastAPI gateway provides REST endpoints for upload/report retrieval.  

---

## IBM Tools (Optional)
- IBM Watsonx.ai: LLM for generating natural-language reports with safe product suggestions.  
- IBM MQ: for reliable product alert notifications (e.g., “rash worsening for 5 days—try soothing lotion”).

---

---

# Idea 2: AI-Powered Personal Learning & Skill Development Coach

## Problem
Many people want to learn new skills but struggle with motivation, finding the right resources, and creating a structured learning path.

---

## Project Idea
Build a platform that acts as a personal AI tutor and coach.  
The user defines a goal (e.g., "learn Python for data analysis"), and the AI agents create a personalized learning plan, provide resources, and adapt based on the user's progress.

---

## How it Works with Tools

### Data Source
The primary data source would be a user's interactions with the platform (simulated "quiz" results, "lessons completed," and user-entered notes). This data is streamed to a central system.

### Apache Kafka
All user-interaction events are streamed to Kafka.  
This event-driven architecture allows for real-time feedback and dynamic plan adjustments.

### Agentic Pipeline
- Learning Plan Agent: When a user sets a goal, this agent uses an LLM to generate a structured curriculum from a vast knowledge base (the data lake).  
- Progress Tracking Agent: This agent consumes user events from Kafka. It tracks progress, identifies areas where the user is struggling, and provides personalized feedback.  
- Resource & Motivation Agent: If a user is stuck, this agent recommends alternative resources (e.g., a specific YouTube video or a blog post). It also sends automated motivational messages based on the user's progress.  

### Data Lake
The data lake (AWS S3) would store a massive collection of learning resources (links to articles, video tutorials, practice problems) and all historical user progress data.

### Containerization & Load Balancing
Docker and Kubernetes would be used to manage the various agents and the user-facing web application ans the agents will be using an Agentic framework like Strands Agents, LangChain.  
The platform would be accessible via a web browser, with a load balancer managing traffic.

### IBM Tools
IBM Watsonx could be integrated to fine-tune a model on educational content, allowing for more domain-specific and accurate coaching responses.

---

## Complexity
High.  
The biggest challenges are building a truly intelligent and adaptive learning model, as well as managing and organizing a vast amount of learning resources in the data lake.

