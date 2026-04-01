# 🛠️ Utilities & Data Preparation

This directory contains the utility scripts requisite for preprocessing the data prior to initializing the primary training pipelines. 

---

## 1. Dataset Generation: `k_dataset.ipynb`

This notebook serves as the foundational component of the data preparation pipeline. It is responsible for acquiring the base trajectories, synthesizing visual demonstrations via motion planning algorithms, and formatting the data to adhere to the LeRobot framework specifications.

### 1.1 - 1.2 Library Installation and Authentication
The preliminary cells are dedicated to environment configuration. 
* They execute the installation of all necessary libraries and dependencies required to operate ManiSkill, LeRobot, and the motion planning scripts.
* Authentication with **Hugging Face (HF)** is performed to enable dataset uploading, alongside optional Google Drive integration.
* **Note:** Following the installation phase, a **session restart** is generally mandated to ensure the newly installed libraries are properly initialized within the runtime environment.

### 1.3 - 1.7 Environment Overrides
In this section, custom *overrides* are systematically applied to the standard simulation environments. This procedure is critical for altering specific parameters of the original environments, including:
* Adjusting the camera observation resolution.
* Modifying the dimensional and physical properties of the target objects manipulated by the robotic agent.

### 1.8 - 1.9 Motion Planning and Data Conversion
This segment defines and modifies the core functions requisite for data manipulation:
* Functions dictating the **motion planning** logic and the synthesis of actual demonstrations (transitioning from physical state arrays to comprehensive action sequences).
* Scripts designed to transpose the generated dataset into the standardized **LeRobot Format v3.0**.

### 1.10 - 1.14 Task Processing Framework
For each designated task, the notebook sequentially executes the following protocol:
1. **Data Acquisition:** Downloads the official trajectories (demos) provided by the ManiSkill repository.
2. **Simulation Replay and Video Synthesis:** Utilizing the `replay_force_panda` function, the motion planning scripts are executed. The acquired trajectories are replayed within the simulation, yielding comprehensive demonstrations paired with visual observations (video data).
3. **Format Conversion:** The `convert_maniskill_v3_native.py` script transforms the raw state-action data into the final dataset architecture. During this phase, critical parameters such as frames per second (FPS), image resolution, and robot kinematics type are specified, and a corresponding natural language instruction is appended.
4. **Repository Upload:** The processed dataset corresponding to the specific task is sequentially uploaded to the designated Hugging Face Hub.

### 1.15 Dataset Consolidation
Following the processing and uploading of individual task datasets, the notebook integrates them. The `lerobot-edit-dataset` utility is deployed to **merge** all distinct task datasets into a singular, unified dataset encompassing all recorded demonstrations.

### 1.16 Integrity Verification (Optional)
The concluding cells provide optional **CHECK** routines. It is highly recommended to execute these diagnostics to audit the resulting dataset, ensuring that:
* It encapsulates all requisite modalities (images, states, actions) accurately.
* It exhibits full computational compatibility with the LeRobot framework prior to advancing to the model training phase.

---

## 2. Language Embeddings Preparation: `k_embeddings.ipynb`

This notebook is a specialized script dedicated exclusively to the pre-computation of language embeddings, a strict computational prerequisite for the **RDT-1B** (Robotics Diffusion Transformer) model architecture.

### 2.1 Initialization and Repository Setup
The initial computational cells are designed to establish the operational environment. This sequence encompasses:
* Authorizing and mounting access to **Google Drive** for persistent data storage.
* Cloning the official RDT repository to access its native architectural configurations.
* Installing the specific software dependencies required for natural language processing and embedding extraction.

### 2.2 Embedding Generation via T5 Architecture
Following the setup phase, the script utilizes the **`T5Embedder`** function. This module is tasked with computing the high-dimensional language embeddings derived from the natural language prompts. These specific prompts are meticulously selected and defined within the designated `TASKS` dictionary.

### 2.3 Output Inspection and Storage
In the final phase, the generated embeddings undergo an inspection process to verify their dimensional accuracy and structural integrity. Upon successful validation, the resulting files are automatically uploaded and persistently stored within the mounted Google Drive directory. This protocol ensures they can be efficiently accessed and loaded during the primary RDT-1B training iterations.
