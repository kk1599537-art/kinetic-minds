# Off-road Autonomy: Semantic Scene Segmentation
*GHR 2.0 Hackathon - Desert Environment Challenge*
*Team Name:* KINETIC MINDS

## 1. Project Overview
This project focuses on navigating Unmanned Ground Vehicles (UGVs) through pixel-level understanding of desert environments using the Falcon Cloud digital twin platform. Our objective is to accurately segment 10 distinct terrain categories to facilitate autonomous off-road navigation.

## 2. Methodology & Training Workflow
* *Architecture:* We implemented a SegFormer (B0) architecture with a lightweight backbone optimized for rapid inference on synthetic desert textures.
* *Training Objective:* Our model minimizes Cross-Entropy + Dice Loss to effectively handle class imbalance between dominant classes (Sky) and rare classes (Flowers/Logs).
* *Environment:* Development leveraged the EDU Conda setup provided by Falcon Cloud for reproducible training workflows.
* [15:52, 18/02/2026] .:
*  * *Optimization:* Hyperparameters including learning rates, batch sizes, and network depth were fine-tuned while maintaining inference efficiency.

## 3. Data Overview & Class Breakdown
We utilized high-quality semantic labels generated via FalconEditor's digital twin platform, enabling rapid iteration without physical data collection.
* *Format:* RGB color + segmented masks.
* *Target Classes:*
    * *Natural Vegetation:* Trees (100), Lush Bushes (200), Dry Grass (300), Dry Bushes (500), Flowers (600).
    * *Inanimate Objects:* Logs (700), Rocks (800), Ground Clutter (550).
    * *Background:* Landscape (7100), Sky (10000).
[15:53, 18/02/2026] .:
## 4. Challenges & Strategic Solutions
* *Data Scarcity:* Leveraged synthetic environments to eliminate costly real-world data collection.
* *Class Imbalance:* Implemented a Weighted Loss Function assigning a 10x higher penalty for misclassifying rare classes like Flowers (ID: 600).
* *Context Shifts:* Applied Strong Geometric Augmentation (random flips/rotations) and Color Jittering to force the network to learn object textures rather than relying on lighting or position.

## 5. Failure Analysis
Through visual diagnosis, we identified common error patterns for future refinement:
* Logs misclassified as Rocks due to similar geometric profiles.
* Flowers confused with Dry Bushes under low-light conditions.
* Shadow regions causing boundary ambiguity in segmentation masks.
[15:53, 18/02/2026] .:
 ## 6. Reproducibility & Instructions
1. *Environment Setup:* Activate the EDU Conda environment and execute setup_env.bat to configure dependencies.
2. *Training:* Run train.py with the provided configuration file specifying model architecture and hyperparameters.
3. *Validation:* Execute test.py on the holdout dataset to reproduce final IoU results and generate confusion matrices.

---
Note: The dataset itself is not included in this repository per hackathon rules.
