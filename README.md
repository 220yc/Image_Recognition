# Construction-Site-Safety  
Ensuring a safe working environment is crucial in the construction industry, given the high level of inherent dangers and the potential for accidents. To minimize the occurrence of accidents, proactive measures for early prevention are of utmost importance.  
## What this project is about?  
The objective of this research is to reduce occupational hazards in construction sites by focusing on the inspection of safety guardrails, particularly those installed along the edges of steel decks. According to occupational safety, workplaces with edge heights exceeding two meters must have appropriate strength guardrails, covers, or other protective devices to prevent workers from falling hazards. In construction operations, steel decking is typically installed first, followed by the erection of safety guardrails. To enhance the effectiveness of safety guardrail inspections, this study proposes independent inspection models for vertical and horizontal guardrails. The inspection process involves first examining the vertical guardrails, then inspecting the edges of the steel decking, and finally assessing the horizontal guardrails.  
  
The main contribution of this research lies in proposing independent inspection models, innovatively applying image preprocessing techniques, and introducing perspective transformation correction for the detection of steel deck edges. These approaches address the challenges of complex backgrounds and similar objects, thereby enhancing the accuracy of safety guardrail detection.

Keywords: construction engineering, safety guardrail detection system, vertical guardrail inspection, steel deck edge detection, horizontal guardrail inspection.  

## Contributions
1. Innovative application of image preprocessing: This research innovatively applies image preprocessing techniques by converting RGB original images to YUV images and enhancing the grayscale image of the Y channel. This preprocessing approach helps highlight the features of safety guardrails and steel decking edges, improving the accuracy of the detection model, especially in environments with low light conditions or backlighting issues.  

2. Introduction of independent detection models for vertical and horizontal directions: Considering that safety guardrails occupy a wide range in the image but have a relatively small area, and construction sites pose complex environmental challenges, this research proposes independent detection models for vertical and horizontal directions. This approach allows for more effective handling of complex scenes in the images and improves the precision of the detection.  

3. Perspective transformation correction for steel decking edges: To address the impact of height and angle variations in construction sites on image detection, this research introduces a perspective transformation correction method for steel deck edges. This helps correct the guardrail images and ensures that the detection model focuses on the target area during the horizontal safety guardrail detection, thereby improving the overall detection accuracy.  

## Why use YOLOv4 as our object detection model?
YOLOv4 is an object detection model that utilizes the CSPDarknet-53 architecture as its backbone for feature extraction. It integrates various techniques to enhance prediction capabilities and improve accuracy, including SPP (Spatial Pyramid Pooling) and PAN (Path Aggregation Network). The prediction layer from YOLOv3 is used for object localization and classification. Throughout the architecture, YOLOv4 tests and incorporates multiple modules and methods from other researchers to improve recognition accuracy.    

To balance computational efficiency and high recognition accuracy, YOLOv4 applies methods such as CSPDarknet-53, SPP, and PANet. Techniques like CutMix, Mosaic data augmentation, and Mish activation are employed to assist in learning. Due to its ability to rapidly and accurately locate and classify objects in images, this research selects YOLOv4 for the preprocessing localization of safety guardrail images. This helps improve the recognition performance of safety guardrail images.  

## Research Methods and Steps
The research focuses on images taken at construction sites of steel structures, which possess unique characteristics in terms of background complexity and challenges. Factors such as occlusion during photography, light and weather variations, and different heights or angles of buildings make the detection of safety guardrails more complex. When observing outdoor construction sites, light and weather conditions have a profound impact on image capture. Therefore, before entering the model detection stage, this research conducts image preprocessing. This includes converting the RGB original image to the YUV color space and enhancing the grayscale image of the Y channel to highlight the features of safety guardrails and steel deck edges, followed by the detection of safety guardrails.

To improve the detection performance of safety guardrails, this research divides the guardrails into two directions: vertical and horizontal. The research first performs vertical guardrail detection on the enhanced guardrail images. Then, it detects the vertical guardrails and subsequently detects the edges of the steel decks to obtain the direction parallel to the horizontal guardrails. The vertical guardrails and steel deck edges are then compared and the image is cropped accordingly. Perspective transformation is applied based on the angle of the steel deck edges to correct the guardrail image. Finally, the detection of horizontal safety guardrails is conducted to remove unnecessary background and focus on the target area, thereby improving the detection accuracy.

![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/20868e2a-1169-40cb-ac84-dc654f6808c8)  
Safety guardrail Inspection Process Flowchart

## Image Preprocessing
In this study, steel structure images will be captured by mobile phones. However, the detection of these images may be challenging due to various factors. For example, the variation in building height can lead to changes in the background, and variations in climate and light direction can result in differences in light and shadows in each captured image. Additionally, changes in light conditions can also affect the images. Therefore, before conducting the guardrail detection, image preprocessing is necessary to enhance the features and make the target objects easier to detect.

To achieve better detection results, this study proposes three methods of image preprocessing, including Contrast Stretching, Histogram Equalization, and Contrast Limited Adaptive Histogram Equalization.

### Contrast Stretching 
First, the RGB image is transformed into the YUV channel, and then the contrast stretching is applied to the Y channel. The stretched Y channel is then merged with the UV channels to convert it back to RGB color image. In this study, the grayscale intensity of the Y channel at position (x,y) in the image is denoted as I_Y(x,y). The contrast-stretched representation is denoted as I_Y(x,y), and it is calculated as follows:
![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/a1642d29-a22a-4279-8924-6eb3b3ca3478)  

Here, min_Y(x,y) represents the minimum grayscale value in the Y channel of the image, and max_Y(x,y) represents the maximum grayscale value in the Y channel. This processing enhances the visibility of darker or brighter areas in the original image.

### Histogram Equalization (HE) 
Histogram equalization is a method that remaps the grayscale image based on the cumulative distribution function (CDF) to expand the range of the histogram and improve the contrast of the image. In this study, we first convert the RGB image to the YUV channel and then apply histogram equalization to the Y channel. This process helps to distribute the brightness more evenly in the original image. Finally, the equalized Y channel is merged with the UV channels and converted back to an RGB image. This step enhances the visual contrast of the image.

### Contrast Limited Adaptive Histogram Equalization (CLAHE) 
In this study, we first convert the RGB image to the YUV channel and then apply contrast-limited adaptive histogram equalization to the Y channel. The equalized Y channel is then merged with the UV channels and converted back to an RGB image.

Contrast Limited Adaptive Histogram Equalization (CLAHE) is achieved by controlling the slope of its cumulative distribution function to suppress noise caused by excessive enhancement of the image. In this method, the equalized value H̃(i) of the i-th gray level value in the original histogram is calculated as follows:
![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/b2eb2d9f-6a3c-462a-8fbb-cb995fc423cb)

Here, P is the threshold value and L is the value that is scaled proportionally above P. This processing helps to effectively control the contrast of the image while suppressing noise generation.
  
To demonstrate the effects of contrast stretching, histogram equalization, and contrast-limited adaptive histogram equalization, two construction site images were selected for experimentation in this study, as shown below.
![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/ac2bddf7-d47b-46fd-bf08-66a0587c8778)

It is evident that the images processed using these three preprocessing techniques exhibit clearer and higher contrast visually, which will aid in the subsequent detection tasks.

# Vertical Guardrail Detection Model
![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/dfff1807-c723-48de-be16-0d930690528b)  

The dataset used in this study consists of images captured by mobile phones at construction sites, specifically focusing on the edges of steel decks with safety guardrails. The dataset comprises a total of 162 images with safety guardrail along the edges. The dataset is divided into a 60% training set and a 40% testing set, following a 6:4 ratio, for the purpose of training and evaluating the guardrail detection model.

![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/39c979eb-277b-43de-b0e4-92c649f7dba8)  

Due to the typical orientation and horizontal positioning of cameras during image capture, vertical guardrail usually cause fewer errors in the images. In the images, vertical guardrail appear upright (as indicated by the red line in the figure) and their markings do not pose any issues. However, in YOLOv4, ground truth is marked with rectangular bounding boxes. Horizontal guardrail (as indicated by the blue diagonal lines in the figure) may appear slanted due to the camera angle or perspective. The marked data, as shown by the yellow boxes in Figure (6), may encompass excessive background information. Particularly, as the slant angle of horizontal guardrail increases, the background coverage widens (as seen in the triangular region) due to varying floors, heights, angles, and orientations. When thin horizontal guardrail only occupy a small portion of the ground truth, but cover a significant amount of background, it can lead to a decrease in detection accuracy. Therefore, in this study, it was decided to independently detect vertical and horizontal guardrail to improve accuracy.  

# Detection of Steel Deck Edges
Steel deck Edge Annotation: When annotating the edges of steel decks, non-horizontal and non-vertical edges can pose challenges. When marking angled edges, it is necessary to annotate both the construction site surroundings and the steel deck itself to ensure that the marked edges represent areas of particular interest.  
To address this issue, this study adopts a multi-box annotation approach within smaller regions. This approach effectively reduces unnecessary background while detecting the edges. Such annotation technique helps ensure that the marked areas represent steel deck edges while minimizing the impact of the environment and other elements, thus improving the detection performance of the model.  
![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/adaadd71-5cbf-4a4a-a279-0650458d3775)  

Steel deck Edge Direction: The purpose of detecting the edges of steel decks is not only to locate them but also to determine the inclination angle of the horizontal guardrail. Therefore, among the predicted bounding boxes in the detected small regions, it is necessary to identify the line segments representing the edges of the steel decks. In this study, the method is based on drawing edge line segments using the coordinates of the predicted bounding box centers. Specifically, threshold values are set based on the angles and distances of line segments formed by pairs of adjacent points. Once a line segment has been drawn, it will no longer be considered for subsequent line segment drawing. When line segments that meet the threshold criteria are detected, they are drawn and recorded.  

<div align=center><img src="https://github.com/220yc/Construction-Site-Safety/assets/91858697/5db21872-ecb9-4250-9b3a-b482d592b59d" width="500" height="350"/></div>

# Perspective Transformation
To achieve perspective transformation of the horizontal safety guardrail, we use the detected results of the angles of the steel bearing deck edges and the vertical guardrail.   
Firstly, we determine the angles of the predicted steel bearing deck edges and then perform perspective transformation.   
Perspective transformation requires four points, so we select the vertical guardrail on both sides of the predicted steel bearing deck edges. We take the coordinates of the top and bottom of the leftmost vertical guardrail, and the coordinates of the top and bottom of the rightmost vertical guardrail (shown as red dots), and substitute them into the following formula:  

<div align=center><img src="https://github.com/220yc/Construction-Site-Safety/assets/91858697/807a6060-11cc-42c9-ad96-62926d526711" width="400" height="70"/></div>
<div align=center><img src="https://github.com/220yc/Construction-Site-Safety/assets/91858697/ab641ba8-806e-4ea6-bd82-d6221044697d" width="900" height="300"/></div>
<div align=center><img src="https://github.com/220yc/Construction-Site-Safety/assets/91858697/fc2daaa3-6a69-4495-8991-46dee8ff1884" width="400" height="300"/></div>
# Horizontal Guardrail Detection Model
This study utilizes the YOLOv4 model for detecting vertical and horizontal safety guardrail. Firstly, the vertical guardrail are detected, and then the steel bearing deck edges are detected to obtain the direction parallel to the horizontal guardrail. The vertical guardrail are compared with the steel bearing deck edges, and the image is cropped accordingly. Then, perspective transformation is applied based on the angle of the steel bearing deck edges to correct the guardrail image. Finally, the detection of horizontal safety guardrail is performed to remove unnecessary background and focus on the target area, thus improving the detection accuracy.  

Through this series of detections, if the horizontal guardrail between the vertical posts are detected, it ensures the proper installation of the guardrail along the steel bearing deck edges.  

![image](https://github.com/220yc/Construction-Site-Safety/assets/91858697/b8e568d5-e6a3-4d0f-8c81-607e387dca6b)  

# Expected results
1. The innovative application of image preprocessing can effectively highlight the features of safety guardrail and steel bearing deck edges, considering factors such as occlusion during photography, lighting variations, and different heights or angles of structures. This improves the accuracy of the detection model.

2. The introduction of separate detection models for vertical and horizontal guardrail allows for more effective handling of complex scenes, enhancing detection accuracy, and improving safety on construction sites.

3. The perspective transformation and correction method for steel bearing deck edges can correct images, focus on the target area, and more accurately rectify any angle and perspective distortions that may exist in the images. This improves the overall detection accuracy and plays a crucial role in adapting to variations in different construction site conditions.

4. Through a series of detections, the presence of horizontal guardrail between vertical posts is determined, ensuring the proper installation of guardrail along the steel bearing deck edges. This helps to ensure the completeness of the guardrail system on construction sites.

