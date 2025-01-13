# Project Overview

This project is aimed on exploration of various approaches for recognition of apples on sample images and provision of list of them apples with coordinates of their centers (in pixels). Several methodologies were tested, ranging from traditional image processing techniques to deep learning models. Below is a summary of the approaches taken and their respective outcomes.

13/01/2025 update:
In this version, in addition to changing the saturation of images 4-6, the following steps were taken:

- The ultralytics library has been updated to version 8.3.59, which includes enhancements to the auto_annotate method, allowing for manual adjustment of confidence and IoU values. More specifically, changing IoU allowed to battle false positive detections (on images 1-3).
- Separate parameters were applied to JPG and PNG images to account for their differing characteristics and the types of objects they represent.
- The 11th iteration of YOLO was employed, specifically YOLOv11x, due to its superior performance in detecting apples in images 4-6. The decision to use this "heavier" model is justified by the fact that auto_annotate still depends on object detection for identifying apples.
- The SAM2 model was employed, although it yielded better results, the gain in accuracy was minimal.

To further improve quality of this solution it is recommended to fine-tune OD model on the custom dataset close to the sample images.

## Files and their contents

- **solution.ipynb**: Solution for this task
- **train_YOLO_detector.ipynb**:  Check original YOLOv8 and then train it on publicly avaliable dataset
- **test_YOLO_detector.ipynb**: Test fine-tuned models with SAHI
- **nonDL_approach.ipynb**: Non-Deep Learning Approach

## Approaches and Results

### 1. Non-Deep Learning Approach
- **Description**: This approach utilized traditional image processing techniques, including edge detection and contour finding, to identify objects in images.
- **Results**: The non-deep learning method showed no significant results in detecting apples. The performance was inadequate for the task, highlighting the limitations of traditional techniques in complex image scenarios.

### 2. YOLOv8 (You Only Look Once) - Not Fine-Tuned
- **Description**: The YOLO model was employed without any fine-tuning on the dataset. The model was used to predict apple locations in sample images.
- **Results**: The initial results from the not fine-tuned YOLO model were below average. The model struggled to accurately detect apples, indicating that further training and adjustments were necessary.

### 3. SAHI (Slicing and Hierarchical Inference)
- **Description**: The SAHI framework was implemented to perform inference using a sliding window technique on the images since on the images 1-3 imgsz is 7500x3500 pxls. This method aimed to improve detection accuracy by processing images in smaller segments.
- **Results**: While SAHI showed better results compared to the previous approaches, the detections were still not plausible. The model's performance was improved but remained insufficient for reliable apple detection.

### 4. Fine-Tuning YOLO on Multiple Datasets
- **Description**: The YOLO model was fine-tuned on several datasets, incorporating data augmentation techniques to enhance the training process. The goal was to improve the model's ability to generalize and accurately detect apples.
- **Results**: Despite the efforts to fine-tune the model and apply augmentations, there were no significant improvements in detection performance. The results remained unsatisfactory, even when using SAHI for inference.
- **Augmentations**: Mixup, HSV changes, Mosaic, Scale
- **Examples**: You can check some of the results in the *demo* folder

### 5. Solution
- **Description**: The final solution involved the use of the SAM model to create annotations for the images. These annotations were in YOLO segmentation format which were then converted to COCO annotation format. After that masks were extracted and applied to the corresponding images. Finally, the centers for each apple were calculated, providing the output of apple locations with their centers. 
- **Results**: The results obtained from this approach were far better than those achieved through object detection without additional fine-tuning on more data from this domain.

## Conclusion
The exploration of various approaches for apple detection revealed the challenges associated with both traditional and deep learning methods. The non-deep learning techniques were ineffective, while the YOLO model did not yield satisfactory results. However, the solution demonstrated significantly better performance in detecting apples and providing their center coordinates. This approach showcased the importance of data preprocessing and the potential of combining different techniques to achieve better results in object detection tasks.

## Note
There is a possibility of another soultions that would require more data from this domain. This data would be later annotated and used for fine-tuning either Object Detection or Segmentation model.
