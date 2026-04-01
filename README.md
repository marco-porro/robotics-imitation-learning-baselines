# Thesis
[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Model%20%26%20Data-FFD21E?style=flat&logoColor=white)](https://huggingface.co/mimirmimir) [![Thesis PDF](https://img.shields.io/badge/%F0%9F%93%9C%20Thesis-2ea44f?style=flat)](thesis.pdf)
[![Diffusion Policy](https://img.shields.io/badge/Diffusion%20Policy-525252?logo=googlecolab)](https://colab.research.google.com/drive/1Y7WWHbDcby_Tj6Lr2pvNwursgn2f9B1A) [![SmolVLA](https://img.shields.io/badge/SmolVLA-525252?logo=googlecolab)](https://colab.research.google.com/drive/1wf0hqzjLR6yL-vlTbdkqXctZP2GqUC3R) [![Pi0](https://img.shields.io/badge/Pi0-525252?logo=googlecolab)](https://colab.research.google.com/drive/1BVCzvt2PfhSahik43obpyKKV_AB7xxAZ) [![RDT-1B](https://img.shields.io/badge/RDT--1B-525252?logo=googlecolab)](https://colab.research.google.com/drive/1dT5BuTBwXS1d3Sf5lcwipcJDJLhjJFDz)

This repository contains the scripts used to implement, train and evaluate the following models: "Diffusion Policy, SmolVLA, Pi0 and RDT-1B" on ManiSkill 3 simulation environment, together with the one used to obtain everythinh needed for the pipeline, such as dataset creation.

These scripts are designed to be run on Google Colab (or a similar Jupyter environment with GPU support) and serve as a practical working pipeline for future students and researchers working on robotic manipulation.

# Imitation Learning in Virtual Environments: Usage Guide

This repository provides standalone Jupyter Notebooks and utility scripts to set up, train, and evaluate different Imitation Learning and Vision-Language-Action (VLA) models for robotic manipulation. The pipelines are optimized for **Google Colab** and utilize [ManiSkill](https://github.com/haosulab/ManiSkill) and [LeRobot](https://github.com/huggingface/lerobot).

## 📁 Repository Structure

The project is organized into the main `scripts/` directory, which contains the core training pipelines and supplementary folders for data processing and extra experiments:

* **`scripts/`**:
  * `dp_84.ipynb`: Pipeline for Diffusion Policy (84x84 resolution). Requires a T4 GPU.
  * `pi0.ipynb`: Pipeline for Physical Intelligence Zero (Pi0). Requires a T4 GPU.
  * `rdt1b.ipynb`: Pipeline for Robotics Diffusion Transformer (1B). Requires an A100 GPU and Google Drive mounting.
  * `smolvla.ipynb`: Pipeline for SmolVLA. Requires a T4 GPU.
  
  * **`utils/`** (Data preparation and processing scripts):
    * `k_dataset.ipynb`: Used to download the original demonstrations, create the dataset, and convert it into the required training formats. Optimized for a T4 GPU.
    * `k_embeddings.ipynb`: Dedicated script to pre-compute the language embeddings specifically required for the RDT-1B model. Requires an A100 GPU and saves outputs directly to your Google Drive.
  
  * **`eval exp/`** (Ablation studies and experiments):
    * Contains alternative evaluation loops compared to the standard ones. These scripts were used for ablation studies and allow you to conduct extra experiments to test the trained policies under modified environmental conditions.

## 🛠️ Prerequisites

Before running any notebook, ensure you have:
1.  **Google Colab**: A Google account to run the environments.
2.  **Hugging Face Account**: A Write-access Token is required to download datasets and upload model weights.
3.  **Weights & Biases (W&B)**: Necessary for monitoring and tracking metrics during training.

---

## 🚀 How to Run the Pipelines (Step-by-Step)

### Phase 1: Data Preparation (Initial Setup)
If you are starting from scratch and need to generate or process the datasets yourself, use the scripts in the `utils/` folder:
1. Run **`k_dataset.ipynb`** to gather the demonstrations, compile them, and convert them into the correct formats.
2. If you intend to train the **RDT-1B** model, you must first run **`k_embeddings.ipynb`** to pre-compute the textual embeddings. This will save significant compute time during the main training phase. The results will be automatically copied to your Google Drive space.

### Phase 2: Training and Standard Evaluation
Open your chosen model's notebook (e.g., `smolvla.ipynb` or `rdt1b.ipynb`) in Google Colab:
1. **Hardware Check**: Go to `Runtime` > `Change runtime type` and ensure the Hardware accelerator is set to **GPU** (Select T4 or A100 depending on the model).
2. **Authentication**: Execute the initial setup cells. You will be prompted to enter your W&B API key and Hugging Face token. *(For RDT-1B, you must also authorize access to Google Drive).*
3. **Execution**: Run the cells sequentially. The script will autonomously install dependencies, register customized simulation environments, download the processed dataset, execute the training loop, and finally perform an evaluation, saving demonstration videos in MP4 format.

### Phase 3: Ablation Studies and Extra Experiments
To test the trained policies in different scenarios or reproduce the ablation studies presented in the thesis:
* Navigate to the **`eval exp/`** folder.
* Use the scripts provided to launch alternative evaluation loops.
* Simply load the weights (checkpoints) of the models you previously trained and saved to observe their behavior under the new experimental conditions.

## ⚙️ Customizing the Execution

Inside the main training notebooks, look for the configuration variables located before the training loop to modify the basic settings:
* **`TASKS_TO_RUN` / `env_id`**: Change the simulation task (e.g., from `PickCube-v1` to `StackCube-v1`).
* **`training_steps`**: Modify the duration of the training (you can lower it for a quick test run).
* **`num_episodes`**: Adjust the number of evaluation videos generated at the end of the process.

## 📥 Accessing the Results
* **Training Metrics**: Tracked live via the W&B link generated in the Colab output.
* **Model Checkpoints**: Saved in the `outputs/` folder in Colab and automatically pushed to your Hugging Face Hub.
* **Videos**: Standard evaluation `.mp4` files are saved in the Colab file explorer (usually under `eval_videos/` or `episodes/`).
