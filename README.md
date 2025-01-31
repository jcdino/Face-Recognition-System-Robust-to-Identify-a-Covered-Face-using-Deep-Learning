# Face-Recognition-System-Robust-to-Identify-a-Covered-Face-using-Deep-Learning
## Content 
1. [Teammates](#teammates)
2. [Abstract](#abstract)
3. [Algorithm Diagram](#algorithm-diagram)
4. [Process](#process)
5. [Code Instructions](#code-instructions)
6. [Result](#result)
7. [Citation](#citation)
## Teammates
- [Jae Yoon Chung](https://github.com/jcdino)
- [Kong Minsik](https://github.com/Minsik96)
## Abstract
As an influence of the COVID 19 pandemic, everyone is required to wear a facial mask. This made it hard to identify the user’s face using the present facial recognition systems with portable devices. Using deep learning, our project resolves this problem by creating a system that identifies the user accurately and efficiently, even when the face is covered by an accessory.
## Algorithm Diagram
![그림1](https://user-images.githubusercontent.com/90415099/147800943-46cd4f1e-f71e-4ce0-9a83-a62ea35dea7f.png)
## Process
### 1. Accessory Detector
The model identifies the accessory the user is wearing. The model used the MobileNet V2 architecture with the weights of ImageNet. Fine tunined the model so that it only classifies the wanted accessories.
![그림2](https://user-images.githubusercontent.com/90415099/147801268-ea5968d2-75d4-419a-9ac4-b3ad9731214f.png)
### 2. Face Recognizer 
A pretrained ResNet10 is used to detect the ROI of the face and FaceNet is used for facial recognition. FaceNet is trained with the images of the registered users. The 128-D embeddings extracted from the face on each image which are used to determine the features of each user. This is done by using the triplet loss function. The model determines the identifies the person in the input image by comparing their embeddings and the features of the registered users, 
![그림3](https://user-images.githubusercontent.com/90415099/147801667-e689abcc-f5a4-4cfb-91bb-0e22ffbc7960.png)
### 3. Image Composition
If the face of the input image is covered by an accessory, the accuracy of the facial recognition is returned low. The image composition process resolves this problem by recreating the face of the input image into an uncovered face. The given graphs compares two situations: compositing images with the same user’s image and a different user’s image. The graphs show that compositing the image with the same user gives a higher accuracy.
![image](https://user-images.githubusercontent.com/90415099/147802038-62c9a6b8-3dac-4618-8a05-8feae03ce515.png)
![image](https://user-images.githubusercontent.com/90415099/147802052-d8fa1675-2219-4ee8-86a9-0b48fa783b8c.png)

## Code Instructions
### 1. Run accessory_detector/train_accessory_detector.py
Excecution code : python train_accessory_detector.py --dataset dataset_accessory<br />
The directory dataset_accessory has the datasets of default faces and faces with masks. You may add more images into each directories.<br />
If you want to add the type of accessories to detect, create a new directory and add images of faces wearing it.<br />
=> Output : accessory_detector.model
### 2. Go to dir output_creator
### 3. Download nn4.small2.v1.t7
[nn4.small2.v1.t7](http://cmusatyalab.github.io/openface/) is copyright Carnegie Mellon University and licensed under the Apache 2.0 License.
### 4. Run codes
- extract_embeddings_default.py<br />
Excecution Code : python codes/default/extract_embeddings_default.py --dataset dataset --embeddings output/embeddings_default.pickle --detector face_detection_model --embedding-model openface_nn4.small2.v1.t7
- train_model_default.py<br />
Excecution Code : python codes/default/train_model_default.py --embeddings output/embeddings_default.pickle --recognizer output/recognizer_default.pickle --le output/le_default.pickle
### 4-1. Repeat step4 with mask
### 4-2. Test result
Test the outputs of step4 and step4-1 with cameras.<br />
Excecution Code : python codes/mask/recognize_video_mask.py --detector face_detection_model --embedding-model openface_nn4.small2.v1.t7 --recognizer output/recognizer_mask.pickle --le output/le_mask.pickle
### 5. Go to dir main_module
### 6. Download shape_predictor_68_face_landmarks.dat
[shape_predictor_68_face_landmarks.dat](https://sourceforge.net/projects/dclib/files/dlib/v18.10/ ) is licensed under the Boost Software License - Version 1.0.
### 7. Download nn4.small2.v1.t7
[nn4.small2.v1.t7](http://cmusatyalab.github.io/openface/) is copyright Carnegie Mellon University and licensed under the Apache 2.0 License.
### 8. Move accessory_detector.model 
From ../accessory_detector/accessory_detector.model to ../main_module/
### 9. Move outputs
From ../output_creator/output/ to ../main_module/output/
### 10. Run zzampong.py
Excecution Code : python zzampong.py --embedding-model openface_nn4.small2.v1.t7 --recognizer output/recognizer_default.pickle --le output/le_default.pickle --recognizer_mask output/recognizer_mask.pickle --le_mask output/le_mask.pickle

## Result
### - Primary Fcae Detector
![image](https://user-images.githubusercontent.com/90415099/147802102-633ba4e2-ba3b-42e9-9c5b-a54c8bc76150.png)

### - Secondary Face Detector 
![image](https://user-images.githubusercontent.com/90415099/147802087-f39ebebd-3e12-4eca-8f07-3d52540630dd.png)
If the marked score is higher than the pre-decided threshold, the system prints out the decided user’s name and device’s status(lock/unlock) to unlock.

## Citation
1. https://www.pyimagesearch.com/2020/05/04/covid-19-face-mask-detector-with-opencv-keras-tensorflow-and-deep-learning/
2. https://www.pyimagesearch.com/2018/09/24/opencv-face-recognition/
