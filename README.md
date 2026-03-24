# TEFA-UNet
[2026]The codes for the work "TEFA-UNet: A Texture-aware and Efficient Fusion-Attention U-shaped Network". Our paper has been submitted to the International Journal of Imaging Systems and Technology. We updated the Reproducibility. I hope this will help you to reproduce the results.
# Prepare data
The datasets we used are the public BraTS 2018 and BraTS 2019 brain glioma MRI datasets.
Extract all raw files into data/unprocessed/.
Run the following scripts to preprocess the 3D MRI data into 2D slices (uniformly cropped and resized to 160x160):
# For Training and Validation sets
python data/BraTS2Dpreprocessing-master/GetTrainingSets.py

# For Testing set
python data/BraTS2Dpreprocessing-master/GetTestingSetsFrom2019.py
The processed .npy data will be automatically saved in the data/processed/2D/ directory.
# Environment
Please prepare an environment with python=3.x, and then install the dependencies.
Required packages: torch, torchvision, numpy, scikit-image, scikit-learn, medpy (required for 95% Hausdorff Distance calculation ), pandas, tqdm, imageio, joblib
# Train/Test
Run the train script on the BraTS dataset. The batch size we used is 32. If you do not have enough GPU memory, the batch size can be reduced to save memory.
python train.py --arch TEFAUNet --batch-size 32 --epochs 200 --loss BCEDiceLoss --lr 0.0001
python test1.py --mode GetPicture
python test1.py --mode Calculate
# Reproducibility
Regarding how to reproduce the segmentation results presented in the paper, we carefully set the random seed, so the results should be consistent when trained multiple times. To comprehensively evaluate the model's performance, we employed Test Time Augmentation (TTA) during the testing phase by applying dihedral group transformations and averaging the multiple predictions. If the training does not give the same segmentation results as in the paper, it is recommended to adjust the learning rate. The model is optimized using AdamW with a Cosine Annealing learning rate adjustment strategy (minimum learning rate set to 1e-5). And, the type of GPU we used in this work is an NVIDIA GeForce RTX 5090 (32GB).
