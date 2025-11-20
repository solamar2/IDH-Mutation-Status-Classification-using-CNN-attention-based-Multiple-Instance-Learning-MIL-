# IDH-Mutation-Status-Classification-using-CNN-attention-based-Multiple-Instance-Learning-MIL-
This project aims to determine the IDH mutation status (IDH+/IDH−) of glioma cases using Whole Slide Images (WSIs). WSIs are extremely large, heterogeneous images that contain both biological tissue and substantial non-informative regions. Therefore, a structured preprocessing pipeline is essential before meaningful feature extraction and classification can be performed.
## Pipeline Summary
1. WSI Segmentation (Trident)
Segmentation is a critical initial step, as a raw WSI typically contains extensive background areas, scanner artifacts, pen markings, and other non-biological elements. The purpose of segmentation is to isolate true tissue regions and exclude irrelevant content, thereby ensuring that downstream models operate exclusively on biologically meaningful data.
2. Patch Extraction (Trident)
Because WSIs cannot be processed directly due to their size, segmented tissues are decomposed into smaller image patches.
The WSIs in the dataset were scanned at varying physical resolutions (20×–40×), so patches were extracted uniformly at 20× magnification with a size of 256×256 pixels.
This choice ensures:
•	consistent physical scale across all cases
•	adequate morphological detail within each patch
•	computational efficiency.
3. Embedding Extraction
Each patch is converted into a high-dimensional feature vector (“embedding”). Two embedding models were employed:
•	UNI2: a self-supervised model trained specifically on large histopathology datasets, designed to capture fine-grained morphological structures.
•	ResNet50: a widely used convolutional neural network that serves as a classical baseline.
4. IDH Classification
Two classification strategies were implemented:
• SVM on mean-pooled embeddings
A baseline method in which patch embeddings are averaged per slide and classified using a Support Vector Machine.
• Attention-based Multiple Instance Learning (MIL)
A more refined approach in which the model learns to assign greater weight to patches that carry discriminative morphological signals relevant to IDH status.
The MIL architecture included an attention mechanism with a hidden dimension of 128, followed by a lightweight classification module.
Training incorporated Adam optimization, L2 regularization, and early stopping to mitigate overfitting.

Performance evaluation:
•	UNI2 + SVM: Accuracy 0.90
•	UNI2 + MIL: Accuracy 0.95, AUC 0.96 (highest performance)
•	ResNet50 + MIL: Accuracy 0.90

Additional Directions
Future extensions may include incorporating color-normalization or threshold-based preprocessing to reduce artifacts prior to segmentation, as well as integrating clinical metadata, which has been shown to enhance predictive accuracy in related studies.

Links
Dataset (Kaggle): TCGA Glioma Molecular Classification
Trident Framework: https://github.com/mahmoodlab/trident
Attention-based Deep MIL: https://arxiv.org/abs/1802.04712
