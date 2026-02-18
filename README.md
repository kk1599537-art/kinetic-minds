Off-Road Autonomy: Semantic Scene SegmentationGHR 2.0 Hackathon | Desert Environment ChallengeTeam: KINETIC MINDS
1. The Mission: Navigating the Unforgiving SandsAutonomous navigation in a desert isn't just about avoiding obstacles; it’s about understanding the texture of the world. For an Unmanned Ground Vehicle (UGV), a "flower" is a harmless decorative element, but a "log" or a "rock" is a mission-ending hazard.Our project leverages the Falcon Cloud digital twin platform to provide the UGV with "semantic eyes." By classifying every single pixel in its field of vision, we enable the vehicle to distinguish between drivable landscape and treacherous terrain.
2. Our Technical ApproachWe didn't just want a model that was accurate; we wanted one that was fast. In a real-world deployment, a UGV cannot wait seconds for a frame to process.Architecture: Why SegFormer (B0)?We chose SegFormer-B0 because it represents the "sweet spot" between Transformer-based global context and real-time efficiency. Unlike traditional CNNs that struggle with long-range dependencies, SegFormer’s positional-encoding-free design allows it to adapt to various image resolutions without losing performance.The Loss Function StrategyStandard Cross-Entropy often ignores the "little things." In a desert, the Sky (ID: 10000) takes up 50% of the image, while Flowers (ID: 600) might take up 0.1%. To fix this, we used a hybrid approach:Dice Loss: Focuses on the overlap between predicted and ground truth masks, great for small objects.Cross-Entropy: Ensures overall pixel-wise accuracy.Class Weighting: We told the model that missing a "Flower" or "Log" is 10x more "painful" than missing a patch of sky.
3. The Digital Twin DatasetInstead of spending weeks in the sun collecting data, we utilized FalconEditor. This allowed us to generate "Perfect Ground Truth"—synthetic data where every pixel's identity is known with 100% certainty.CategoryTarget Classes (IDs)VegetationTrees (100), Lush Bushes (200), Dry Grass (300), Dry Bushes (500), Flowers (600)InanimateLogs (700), Rocks (800), Ground Clutter (550)BackgroundLandscape (7100), Sky (10000)
4. Overcoming "Desert Blindness" (Challenges)Training an AI to see in the desert comes with unique hurdles:The "Same-ish" Problem: Everything in the desert is beige, brown, or grey.Solution: We used Color Jittering and Strong Geometric Augmentations. By artificially changing the lighting and flipping the images, we forced the model to learn shapes and textures rather than just memorizing "brown = ground."Shadow Ambiguity: High-noon sun creates harsh shadows that look like holes or rocks.Solution: We fine-tuned the network depth to capture higher-level features that help the model understand that a shadow on the ground is still part of the "Landscape."
5. Lessons from the "Mirage" (Failure Analysis)No AI is perfect. During our validation phase, we spotted three key areas for future growth:Geometric Mimicry: A fallen log and a long rock have very similar edges. Our model occasionally swaps these labels.Low-Light Confusion: At "sunset" (low-light simulations), the vibrant colors of flowers disappear, making them indistinguishable from dry bushes.The "Borderlands": The transition between a bush and the ground is often blurry in synthetic textures, leading to "noisy" edges in our segmentation masks.
6. Get Running (Reproducibility)Ready to see what KINETIC MINDS built? Follow these steps:🛠 Environment SetupWe use the EDU Conda setup for a "plug-and-play" experience.Bash# Activate your environment
conda activate edu_env

# Run the automated setup script
setup_env.bat
🚀 Training & ValidationTo retrain the model or test it against your own holdout set:Train: python train.py --config configs/segformer_desert.yamlEvaluate: python test.py --weights best_model.pthNote: The raw dataset is restricted per Hackathon rules and is not included in this repo. You must have access to the Falcon Cloud data directory.

## 7. Methodology & Training Workflow
* *Architecture:* We implemented a SegFormer (B0) architecture with a lightweight backbone optimized for rapid inference on synthetic desert textures.
* *Training Objective:* Our model minimizes Cross-Entropy + Dice Loss to effectively handle class imbalance between dominant classes (Sky) and rare classes (Flowers/Logs).
* *Environment:* Development leveraged the EDU Conda setup provided by Falcon Cloud for reproducible training workflows.
* [15:52, 18/02/2026] .:
*  * *Optimization:* Hyperparameters including learning rates, batch sizes, and network depth were fine-tuned while maintaining inference efficiency.

## 8. Data Overview & Class Breakdown
We utilized high-quality semantic labels generated via FalconEditor's digital twin platform, enabling rapid iteration without physical data collection.
* *Format:* RGB color + segmented masks.
* *Target Classes:*
    * *Natural Vegetation:* Trees (100), Lush Bushes (200), Dry Grass (300), Dry Bushes (500), Flowers (600).
    * *Inanimate Objects:* Logs (700), Rocks (800), Ground Clutter (550).
    * *Background:* Landscape (7100), Sky (10000).
[15:53, 18/02/2026] .:
## 9. Challenges & Strategic Solutions
* *Data Scarcity:* Leveraged synthetic environments to eliminate costly real-world data collection.
* *Class Imbalance:* Implemented a Weighted Loss Function assigning a 10x higher penalty for misclassifying rare classes like Flowers (ID: 600).
* *Context Shifts:* Applied Strong Geometric Augmentation (random flips/rotations) and Color Jittering to force the network to learn object textures rather than relying on lighting or position.



