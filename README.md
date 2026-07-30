**Title**

A Novel Graph Convolution Network-based framework for classification of performance in routine squat exercises to prevent training injuries

**Description**

This study presents a deep learning-based framework for classifying squat exercise quality from monocular video. The approach combines pose estimation, automated labeling, and spatial-temporal modeling to enable robust assessment of human movement without specialized hardware. Human skeletal keypoints are extracted using a pose estimation framework, with Sports2D and Mediapipe employed to enhance 2D joint tracking and representation. A symmetry index generates frame-wise labels, highlighting deviations that indicate improper form or potential injury risk. Spatial-temporal dependencies are captured using graph convolutional networks: a baseline ST-GCN models joint relationships over time, while CTR-GCN further refines representations through adaptive topology learning. Together, these components allow accurate, real-world classification of squat performance.

**Dataset Information:**

Two publicly available datasets were used in this study for exercise and posture analysis. The first “A Multi-View Raw Video Dataset of Seven Fitness Exercises with Good/Bad Form Labels” dataset contains raw MP4 videos of seven exercises, including standing dumbbell side bend, bent-over dumbbell row, alternating dumbbell bicep curl, standard push-up, standing dumbbell shoulder press, bodyweight squat, and overhead dumbbell triceps extension. Videos were recorded in a gym using smartphones from three views (front, side, and diagonal) and include 26 subjects. Each video is labeled by subject ID, exercise type, posture quality (good/bad), and camera view. The dataset is available on Mendeley Data: https://data.mendeley.com/datasets/kgbb3yn47p/2. The second dataset, “Workout/Exercises Video” from Kaggle (https://www.kaggle.com/datasets/hasyimabdillah/workoutfitness-video), contains videos of various gym exercises organized by exercise type. For this study, only the squat videos were used. The videos were captured in diverse real-world environments with different camera angles, backgrounds, and movement patterns, making the dataset suitable for training and evaluating computer vision models for exercise recognition and posture assessment.

**Code Information:**

This README ensures reproducibility and transparency per the specified format. Reproducibility Algorithms and Code - the pipeline includes five main notebooks for squat exercise quality classification, leveraging pose estimation, symmetry index auto labelling, and two graph convolutional network architectures (ST GCN and CTR GCN):


**Module:** Main Model (main.ipynb) **Purpose:** Performs skeleton-based feature extraction and trains ST-GCN and CTR-GCN models for squat posture classification.
	**Description:** Processes squat videos using MediaPipe Pose to extract 3D skeletal joint coordinates. The extracted skeletons are normalized and converted into graph-based representations with position, velocity, acceleration, and jerk features (12 input channels). The notebook automatically labels samples using the Symmetry Index (SI) threshold, trains both Spatial-Temporal Graph Convolutional Network (ST-GCN) and Channel-wise Topology Refinement Graph Convolutional Network (CTR-GCN), evaluates their performance, and generates classification metrics, confusion matrices, and training history.
**Module:** Ablation Study (Ablationstudy.ipynb) **Purpose:** Evaluates the contribution of different model configurations and training settings.
	**Description:** Performs an Ablation study by modifying selected model components and hyperparameters to investigate their influence on squat classification performance. Experimental results are summarized to compare different configurations and identify the contribution of each component.
**Module:** Cross-Validation (crossvalidation.ipynb) **Purpose:** Evaluates the robustness and generalization capability of the proposed graph convolutional models.
	**Description:** Implements k-fold cross-validation to assess the consistency and reliability of the ST-GCN and CTR-GCN models across multiple training and testing splits. Performance metrics from each fold are summarized to evaluate model stability.

	
**Output:**

1.	Angular.ipynb (Kinematic Analysis):
Outputs: Subject-wise Excel analysis files, Time series of joint angle, velocity, acceleration, and jerk, Angle statistics (maximum, minimum, ROM, mean, median, standard deviation, and mode), Derivative statistics (velocity, acceleration, and jerk),	Consolidated summary workbook (All_Subjects_Summary.xlsx).
2.	main.ipynb (ST-GCN and CTR-GCN Model):
Outputs: Cached extracted skeleton features (.pt), Trained ST-GCN and CTR-GCN models (if model saving is enabled), Classification reports,	Training history (training_history.csv), Results summary (results_summary.csv),	Confusion matrices,	Accuracy plots (Comparison_Accuracy.png), Confusion matrix plots (Comparison_Confusion.png),	Classification report text file (classification_reports.txt).
3.	Ablationstudy.ipynb (Ablation study):
Outputs: Performance comparison of different model configurations, Experimental summary and evaluation metrics.
4.	crossvalidation.ipynb (Cross-validation):
Outputs: •	Cross-validation accuracy and F1-score for each fold,	Average performance metrics,	Fold-wise evaluation summaries,	Performance comparison across folds.


**Usage Information and requirement details:**

**1. Clone the Repository:**
Access the project repository by cloning it to your local system using the command git clone Prashanth257/GCN, and then navigate into the project directory using cd GCN.

**2. Install Dependencies:**
Use Python 3.9 or later and install the required packages using:
pip install -r requirements.txt
Example requirements.txt includes:
numpy==1.23.5, pandas==1.5.3, torch==2.0.1, torch-geometric==2.3.1, scikit-learn==1.2.2, mediapipe==0.10.7, opencv-python==4.8.0, matplotlib==3.7.1, seaborn==0.12.2, tabulate==0.9.0, openpyxl==3.1.2, tqdm==4.65.0

**3. Download Dataset:**
Download the dataset from Kaggle and Mendeley, place the video files in a local directory (e.g., ./videos/). Ensure the folder structure matches the paths used in the code.

**4. Update File Paths:**
Modify dataset paths in the scripts or notebooks by setting:
VIDEO_DIR = "./videos/"

**5. Run the Pipeline:**
•	Execute each module sequentially:
•	Kinematic Analysis: python angular.py
•	ST-GCN & CTR-GCN: python main.py
•	Ablation: python Ablationstudy.py
•	Cross-Validation: python Cross validation.py
•	Each step generates corresponding outputs such as trained models, evaluation metrics, and performance plots.

**6. Verify Outputs:**
After execution, verify that the following files are generated:
•	Subject-wise and summary Excel files containing kinematic statistics. 
•	Cached skeleton feature file (.pt). 
•	Training history (training_history.csv). 
•	Results summary (results_summary.csv). 
•	Classification reports (classification_reports.txt). 
•	Accuracy plots (Comparison_Accuracy.png). 
•	Confusion matrix plots (Comparison_Confusion.png). 
•	Console output showing training progress, classification accuracy, weighted F1-score, and evaluation metrics for both ST-GCN and CTR-GCN models.


**Materials and Methods:
Computing Infrastructure:**
Experiments were conducted in a controlled computational environment to ensure consistency and reproducibility. The implementation was carried out on a system running Windows 11. The hardware configuration consisted of 128 GB RAM and an Intel Core Ultra 9 processor, providing sufficient computational capability for model development and evaluation.

**Dataset:**
The datasets used in this study comprises squat exercise videos obtained from the publicly available Workout/ExercisesVideo on Kaggle (https://www.kaggle.com/datasets/hasyimabdillah/workoutfitness-video) and Mendeley (https://data.mendeley.com/datasets/kgbb3yn47p/2). It contains MP4 video recordings captured under diverse real-world conditions. Access to the dataset requires a valid Kaggle account where as in Mendeley the dataset is publicly available.

**Evaluation Method-train-test split sets, Ablation study, 5 fold cross validation:**
The performance of the proposed framework is evaluated using a stratified data splitting strategy to ensure balanced class representation across subsets. The dataset is divided into training (70%), validation (15%), and testing (15%) sets. To facilitate a direct comparison, all models were trained and evaluated under identical conditions, with the same data splits, input features, hyperparameters, and evaluation metrics applied uniformly across both architectures. Models are trained using the Adam optimizer with a learning rate of 3×10⁻⁴ and weight decay of 1×10⁻³ with a batch size of 32 for a maximum of 20 epochs on an NVIDIA RTX 4500 GPU. Both the ST-GCN and CTR-GCN models are evaluated on the held-out test set comprising 15% of the videos. During training, early stopping is employed based on validation loss with a patience of eight epochs. The model achieving the lowest validation loss is selected for final evaluation. Classification performance is assessed using standard evaluation metrics. Primary metrics include accuracy, precision, recall, and F1-score, which provide a comprehensive measure of model effectiveness in binary classification (Safe vs. Risk). Secondary evaluation is performed through confusion matrices and detailed classification reports to further analyze model predictions. To ensure robustness, stratified k-fold cross-validation is applied on the training set only for hyperparameter tuning, while the fixed 15% test split is reserved for final unbiased evaluation, allowing assessment of model generalization across multiple data splits. Ablation study is conducted to systematically evaluate the contribution of each component. Furthermore, an ablation study is conducted to systematically evaluate the contribution of each component of the proposed framework, providing insights into the relative importance of model modules and architectural choices. All evaluation metrics are computed using standard functions from the scikit-learn library, and results are visualized through confusion matrix heatmaps, as well as training and validation loss and accuracy curves. These visualizations facilitate monitoring of the learning process and provide insights into model convergence, generalization performance, and the impact of individual components on overall effectiveness.

**Limitations:**
The proposed framework shows that GCNs are able to effectively classify the human squat posture in a graph-based setting, but there are a few drawbacks to be noted. The performance of the system is dependent on the input videos and the accuracy of the MediaPipe Pose landmark extraction. Skeletal coordinates extracted may vary depending on lighting conditions, camera position, occlusions, subject appearance and the speed of movement. The current method uses a predefined Symmetry Index (SI) threshold for squat quality evaluation, which may not fully capture the variations in human movement patterns or the comprehensive assessment criteria used by experts. The trained ST-GCN and CTR-GCN may not be able to be generalized to other subjects and exercise styles, as well as different environments, because of the size and diversity of the data set. Also, the framework is currently limited to the analysis of the squat posture and further adaptations need to be done for other exercises or rehabilitation movements. In addition, using graph-based deep learning models and feature extraction using MediaPipe may be limiting in low-resource devices for real-time implementation. These limitations can be overcome with future work that involves using larger and more diverse datasets, creating adaptive labelling strategies, using multiple views of the input, using expert annotated ground truth and lightweight models optimized for real-time applications.

