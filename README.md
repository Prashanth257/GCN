**Title**

A Novel Graph Convolution Network-based framework for classification of performance in routine squat exercises to prevent training injuries

**Description**

This study presents a deep learning–based framework for the classification of squat exercise quality using monocular video data. The proposed approach integrates pose estimation, automated labeling, and spatial–temporal modeling to enable robust and efficient assessment of human movement without the need for specialized hardware. Human skeletal keypoints are extracted using a pose estimation framework to represent joint movements across video frames. A symmetry index is applied to generate frame-wise labels, identifying deviations that indicate improper form or injury risk. Spatial–temporal dependencies in motion are modeled using graph convolutional networks. A baseline ST-GCN captures joint relationships over time, while CTR-GCN enhances representation through adaptive topology learning. Together, these components enable accurate classification of squat performance in real-world scenarios.

**Dataset Information:**

The Workout/Exercises Video (https://www.kaggle.com/datasets/hasyimabdillah/workoutfitness-video) on Kaggle consists of videos of individuals performing various gym exercises, organized by exercise type in separate folders. It is primarily designed for video classification and human activity recognition tasks. For this study, only the squat exercise videos were utilized to focus on analyzing squat performance. The dataset includes short video clips captured in diverse real-world settings with variations in camera angles, backgrounds, and motion patterns. Such diversity makes it suitable for training and evaluating models for exercise recognition and movement quality assessment.

**Code Information:**

This README ensures reproducibility and transparency per the specified format. Reproducibility Algorithms and Code – the pipeline includes five main notebooks for squat exercise quality classification, leveraging MediaPipe pose estimation, symmetry index auto labelling, and two graph convolutional network architectures (ST GCN and CTR GCN):

**Module	Purpose	Description**

**Module:** Feature Extraction (GitHub_Angular.ipynb) **Purpose:** Extracts kinematic statistics (angular velocity, acceleration, jerk) from joint angle data.
	**Description:** Computes kinematic statistics such as velocity, acceleration, and jerk from joint angle data to capture motion dynamics.
**Module:** ST-GCN Model (GitHub_ST_GCN.ipynb)	**Purpose:** Classifies squat quality (Safe vs. Risk) using a baseline Spatial Temporal Graph Convolutional Network.
	**Description:** Implements a baseline Spatial-Temporal Graph Convolutional Network using a fixed skeleton adjacency matrix.
**Module:** CTR-GCN Model (GitHub_CTR_GCN.ipynb) **Purpose:**	Enhances squat classification using Channel wise Topology Refinement Graph Convolutional Network (CTR GCN) with learnable channel specific adjacency matrices.
	**Description:** Extends the baseline with channel-wise topology refinement for adaptive graph structure learning.
**Module:** Ablation Study (Ablation_Study.ipynb) **Purpose:**	Optimizes CTR GCN hyperparameters via learning rate ablation study.
	**Description:** Evaluates the impact of different learning rates on the performance of the CTR-GCN model.
**Module:** Cross-Validation (5&10_Fold_Cross_Validation.ipynb) **Purpose:**	Performs robust evaluation of CTR GCN using stratified k fold cross validation.
	**Description:** Assesses model generalizability using 5-fold and 10-fold cross-validation techniques.

**Output:**

1.	GitHub_Angular.ipynb (Kinematic Analysis):
Outputs: Biomechanics_Full_Report.xlsx (with sheets: TimeWise_Data, Angle_Descriptive_Stats, Kinematic_Stats), console preview of kinematic statistics table.
2.	GitHub_ST_GCN.ipynb (ST GCN Model):
Outputs: squat_stgcn.pt, squat_model_metrics.xlsx (with sheets: Training_History, Confusion_Matrix), plots (loss_curve.png, accuracy_curve.png, confusion_matrix.png).
3.	GitHub_CTR_GCN.ipynb (CTR GCN Model):
Outputs: best_squat_ctrgcn.pt, Squat_Model_Metrics.xlsx (with sheets: Training_Curves, Confusion_Matrix), plots (loss_curves.png, accuracy_curves.png, confusion_matrix.png).
4.	Ablation_Study.ipynb (Learning Rate Ablation for CTR GCN):
Outputs: Squat_LR_Ablation.xlsx (with sheet: LR_Ablation_Summary), best model files, console output of best LR and results table.
5.	5&10_Fold_Cross_Validation.ipynb (k Fold Cross Validation):
Outputs: Console output of per fold test accuracy, average accuracy, and standard deviation (5 fold and 10 fold results). 


**Usage Information and requirement details:**

**1. Clone the Repository:**
Access the project repository by cloning it to your local system using the command git clone Prashanth257/GCN, and then navigate into the project directory using cd GCN.
**2. Install Dependencies:**
Use Python 3.9 or later and install the required packages using:
pip install -r requirements.txt
Example requirements.txt includes:
numpy==1.23.5, pandas==1.5.3, torch==2.0.1, torch-geometric==2.3.1, scikit-learn==1.2.2, mediapipe==0.10.7, opencv-python==4.8.0, matplotlib==3.7.1, seaborn==0.12.2, tabulate==0.9.0, openpyxl==3.1.2, tqdm==4.65.0
**3. Download Dataset:**
Download the dataset from Kaggle and place the video files in a local directory (e.g., ./videos/). Ensure the folder structure matches the paths used in the code.
**4. Update File Paths:**
Modify dataset paths in the scripts or notebooks by setting:
VIDEO_DIR = "./videos/"
**5. Run the Pipeline:**
•	Execute each module sequentially:
•	Kinematic Analysis: python GitHub_Angular.py
•	ST-GCN Model: python GitHub_ST_GCN.py
•	CTR-GCN Model: python GitHub_CTR_GCN.py
•	Learning Rate Ablation: python Ablation_Study.py
•	Cross-Validation: python 5&10_Fold_Cross_Validation.py
•	Each step generates corresponding outputs such as trained models, evaluation metrics, and performance plots.
**6. Verify Outputs:**
•	Model files (.pt) in the working directory
•	Excel files containing performance metrics
•	Training curves and confusion matrices
•	Console output for accuracy and evaluation results

**Materials and Methods:
Computing Infrastructure:**
All experiments were conducted in a controlled computational environment to ensure consistency and reproducibility. The implementation was carried out on a system running Windows 11. The hardware configuration consisted of 8 GB RAM and an Intel Core i5 processor, providing sufficient computational capability for model development and evaluation.
**Dataset:**
The dataset used in this study comprises squat exercise videos obtained from the publicly available Workout/ExercisesVideo on Kaggle (https://www.kaggle.com/datasets/hasyimabdillah/workoutfitness-video). It contains MP4 video recordings captured under diverse real-world conditions. Access to the dataset requires a valid Kaggle account.

**Evaluation Method are evaluated on test sets using:**
The performance of the proposed framework is evaluated using a stratified data splitting strategy to ensure balanced class representation across subsets. The dataset is divided into training (70%), validation (15%), and testing (15%) sets. Classification performance is assessed using standard evaluation metrics. Primary metrics include accuracy, precision, recall, and F1-score, which provide a comprehensive measure of model effectiveness in binary classification (Safe vs. Risk). Secondary evaluation is performed through confusion matrices and detailed classification reports to further analyze model predictions. Both the ST-GCN and CTR-GCN models are evaluated on the held-out test set comprising 15% of the skeleton sequence samples. During training, early stopping is employed based on validation loss with a patience of eight epochs, and the model achieving the lowest validation loss is selected for final evaluation. To ensure robustness, cross-validation is additionally performed using a stratified k-fold strategy, allowing assessment of model generalization across multiple data splits. Furthermore, an ablation study is conducted to systematically evaluate the contribution of each component of the proposed framework, providing insights into the relative importance of model modules and architectural choices. All evaluation metrics are computed using standard functions from the scikit-learn library, and results are visualized through confusion matrix heatmaps, as well as training and validation loss and accuracy curves. These visualizations facilitate monitoring of the learning process and provide insights into model convergence, generalization performance, and the impact of individual components on overall effectiveness.
**Limitations:**
During the auto-labelling process, it was identified that the class was imbalanced, it was considered a significant limitation. The class was particularly underserved by "Bad Form", whereas the dataset was majorly controlled by "Safe Form" samples. The imblanceness might present a risk of class-bias, that effectively restricts the model's sensitivity towards minor errors. As a result, the evaluation metrics provide a deeper understanding.
Due to the dependency on the orientation provided by the camera, various videos exhibit misalignment. The limitations include the misdetection or frontal view; dependency on the sagittal plane restrict the framework in detecting various errors. The symmetry index provides a dedicated estimation, that may not exclusively capture all the movements associated with injury. 


