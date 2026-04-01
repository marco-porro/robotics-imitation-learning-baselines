# 🛠️ Utilities & Data Preparation

This directory contains the utility scripts requisite for preprocessing the data prior to initializing the primary training pipelines. 

---

## 1. Dataset Generation: `k_dataset.ipynb`

This notebook serves as the foundational component of the data preparation pipeline. It is responsible for acquiring the base trajectories, synthesizing visual demonstrations via motion planning algorithms, and formatting the data to adhere to the LeRobot framework specifications.

Below is the step-by-step workflow:

### 1 and 2. Library Installation and Authentication
The first cells of the notebook are dedicated to environment setup.
* They install all the required libraries and dependencies to run ManiSkill, LeRobot, and the motion planning scripts.
* You are prompted to log in to **Hugging Face (HF)** (necessary for uploading the datasets) or Google Drive.
* **Important:** After executing the installation cells, it is generally necessary to **restart the session** in Colab or your notebook environment, as indicated within the code itself. This ensures that the newly installed libraries are loaded correctly.

### 2.1 - 2.5 Environment Overrides
In this section, custom *overrides* are applied to the standard simulation environments. This step is crucial for modifying certain characteristics of the original environments, such as:
* Changing the camera resolution.
* Altering the size or physical properties of the objects manipulated by the robot.

### 2.6 - 2.71 Motion Planning and Conversion Scripts
Here, the core functions needed for data manipulation are created or modified:
* Functions for the **motion planning** scripts and the actual generation of demonstrations (transitioning from physical states to complete action sequences).
* Functions and scripts required to convert the generated dataset into the specific **LeRobot Format v3.0**.

### 3 - 7. Processing Individual Tasks
This is the main operational phase. For each chosen task, processing one at a time, the notebook sequentially executes the following operations:
1. **Download:** Downloads the official trajectories (demos) provided by ManiSkill.
2. **Replay and Video Generation:** Using the `replay_force_panda` function, the motion planning scripts are executed. The downloaded trajectories are "replayed" in the simulation, transforming them into complete demonstrations with videos (visual observations).
3. **Conversion:** Through the `convert_maniskill_v3_native.py` script, the raw data is converted into the final dataset format. During this phase, you select key parameters such as FPS, image resolution, and robot type, and append a natural language instruction.
4. **Upload:** The specific dataset just converted for the individual task is uploaded to your Hugging Face Hub.

### Dataset Merging
After processing and uploading the datasets for each single task to Hugging Face, the notebook unifies them. The `lerobot-edit-dataset` command is used to **merge** all the individual task datasets into one large, comprehensive dataset containing all demonstrations.

### Final Check (Optional)
At the end of the notebook, there are optional **CHECK** cells. It is highly recommended to run them to inspect the resulting dataset and ensure that:
* It contains all the correct information (images, states, actions).
* It is actually and fully compatible with the LeRobot framework before proceeding to the model training phase.

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
