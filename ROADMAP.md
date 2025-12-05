# **Clin-NSVR – Detailed Project Roadmap**

This roadmap provides a structured and sequential development plan for building **Clin-NSVR**, a clinically grounded neuro-symbolic visual reasoning system for medical image diagnosis. It outlines the major phases of the project from dataset preparation and baseline modeling to neuro-symbolic integration, evaluation, and deployment.

---
## **Phase 0 — Project Initialization**

The project begins by setting up the development environment and repository structure. This includes creating directories for source code, data, experiments, and documentation. A Python environment with all required dependencies (PyTorch, MONAI, HuggingFace Transformers, PyTorch Geometric, Prolog/PyReason, visualization tools, etc.) should be established.  
Experiment tracking tools such as Weights & Biases and a configuration manager like Hydra will also be integrated.  
By the end of this phase, the repository should be fully organized, version-controlled, and ready to receive data and model code.

---

## **Phase 1 — Dataset Acquisition and Preprocessing**

In this phase, large-scale chest X-ray datasets such as MIMIC-CXR and CheXpert are downloaded and prepared. Images undergo standardized preprocessing such as resizing, normalization, consistent orientation handling, DICOM-to-image conversion (if required), and patient-level data splitting into train/validation/test sets.  
Exploratory data analysis is conducted to understand class imbalance, image quality variability, label distributions, and any dataset-specific properties. If needed, weak supervision or NLP-based tools can be used to convert radiology reports into structured labels.  
By completion, the project should have a clean, organized, and uniformly preprocessed dataset pipeline.

---

## **Phase 2 — Baseline Deep Learning Models**

Before developing the neuro-symbolic components, strong neural baselines are trained for benchmarking purposes. Popular architectures such as ResNet, ViT, Swin Transformer, or ConvNeXt are trained for classification of the primary target conditions (cardiomegaly, edema, effusion, consolidation, etc.).  
The focus here is on achieving reliable performance with metrics such as AUROC, AUPRC, F1 score, and calibration. Baseline interpretability methods like Grad-CAM and attention rollout will help establish how conventional models behave.  
These models form the reference point against which the advantages of the neuro-symbolic system will later be compared.

---

## **Phase 3 — Neural Perception Module**

This phase builds a detailed perception pipeline capable of extracting anatomical and pathological structures from the images.  
Segmentation models (UNet, nnUNet, UNETR, MedSAM) will identify anatomical regions such as lungs and heart, while detection models (YOLOv8/10, Detectron2, or transformer-based detectors) will identify lesions such as opacities or effusions.  
The perception pipeline will also compute measurements like cardiothoracic ratio, spatial zone classification (upper/middle/lower lung fields), symmetry differences, and patch-level texture features.  
Uncertainty estimation techniques will be introduced so that downstream reasoning modules can consider confidence levels.  
The final output of this stage includes structured JSON files containing all extracted anatomical structures, lesions, attributes, and confidence scores.

---

## **Phase 4 — Scene Graph Construction**

Using the output of the perception module, each image is transformed into an **anatomical–pathological scene graph**.  
Nodes represent structures (heart, lungs, regions) and findings (effusion, opacity, nodule), while edges encode relationships such as **left_of**, **overlaps**, **contains**, and **symmetrical_to**.  
A well-defined graph schema and consistent construction rules ensure that graphs are reproducible, clinically meaningful, and structurally sound. Tools like NetworkX and GraphViz will be used for graph creation and visualization.  
This graph becomes the foundation for symbolic reasoning and serves as the project's central intermediate representation.

---

## **Phase 5 — Clinical Knowledge Base and Rule Encoding**

In this phase, clinically meaningful diagnostic rules are formalized and encoded. These rules represent standard radiological criteria for different conditions. For example, cardiomegaly may be defined using cardiothoracic ratio thresholds, and pulmonary edema may be associated with bilateral perihilar opacities or vascular redistribution.  
The rules will be written in a machine-readable symbolic form (Prolog/PySWIP, Problog, or custom logic engines). A companion Python interface will allow the reasoning engine to access and evaluate these rules.  
Careful testing ensures that rules activate correctly when given well-designed example scene graphs.

---

## **Phase 6 — Diagnostic DSL (Domain-Specific Language)**

A domain-specific language is created to express diagnostic reasoning steps programmatically. The DSL includes primitive operations such as finding specific structures, counting lesions, comparing measurements, checking spatial relations, and aggregating evidence.  
The DSL is defined through a formal grammar, and an interpreter or runtime engine is implemented to parse and execute DSL programs on the scene graph.  
This language enables flexible querying of the image in a structured and interpretable way, forming a bridge between natural language questions and symbolic reasoning procedures.

---

## **Phase 7 — Program Generator (Neural Program Synthesis)**

A transformer-based model (such as T5 or BART) is trained to convert natural-language diagnostic questions or reporting prompts into DSL programs.  
A synthetic and template-based dataset of question–program pairs is generated from the scene graph schema and clinical knowledge base.  
Training involves supervised sequence-to-sequence learning, with optional enhancements such as execution-guided decoding or reinforcement learning to maximize correctness.  
The program generator ensures that users can interact with the system naturally while the underlying reasoning remains structured and symbolic.

---

## **Phase 8 — Neuro-Symbolic Integration**

With perception, scene graphs, rules, and program generation complete, the full system is integrated.  
Given an input X-ray and a query, the system performs the entire pipeline:  
**image → perception → scene graph → program generation → symbolic execution → diagnosis**.  
The symbolic executor combines structured graph evidence and clinical rules to derive a diagnosis. It also produces reasoning traces, highlighting which rules fired and which graph nodes supported the decision.  
This phase ensures a smoothly functioning, interpretable, end-to-end pipeline.

---

## **Phase 9 — Evaluation and Robustness Analysis**

The system undergoes rigorous evaluation. Diagnostic accuracy is assessed using AUROC, AUPRC, sensitivity, specificity, and calibration.  
External validation using a separate dataset ensures the model generalizes beyond its training distribution. Stress testing with noise, occlusions, and perturbations evaluates robustness.  
Explainability quality is examined using faithfulness tests, ablation studies, and qualitative case studies comparing neural-only and neuro-symbolic outputs.  
This phase ensures the system performs reliably and interpretably across varied scenarios.

---

## **Phase 10 — Deployment and User Interface**

A lightweight, interactive demonstration is built. A backend API (FastAPI) will process incoming images and queries, execute the entire pipeline, and return outputs. A front-end interface using Streamlit or Gradio will enable users to visualize the chest X-ray, scene graph, symbolic reasoning steps, and final diagnosis.  
The whole system is containerized using Docker, enabling reproducible execution across different machines.

---

## **Phase 11 — System Finalization and Documentation**

The final phase involves polishing the codebase, preparing complete documentation, and organizing results and visualizations.  
Well-structured documentation will describe system components, usage instructions, model architectures, sample outputs, and configuration details.  
This ensures that the repository remains maintainable, deployable, and easy for others to understand or extend.