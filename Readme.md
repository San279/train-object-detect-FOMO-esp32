## Guide to training FOMO object detection model in Edge Impulse
  [ภาษาไทย](https://github.com/San279/train-FOMO-object-detect-esp32/blob/main/Readme-th.md)
  <br/><br/>
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) is a object detection model designed for constrained device. Due to it's low foot print and memory requirement, this model is highly suitable for AIOT box or Esp32-S3. This repository will provide simple tips for building a FOMO model in Edge Impulse, including data collection, training, and deployment.   
<br/>
## What you'll need
 - AIOT, Esp32S3 or any Esp32 series.
 - OV2640.
 - Webcam (optional).
 - Edge Impulse account(free).
<br/> <br/>
## Before we begin
  <strong> Register a free account in Edge Impulse, and create a new project. </strong> 
  <br/> <br/>
  ![alt text](/Images_for_readme/create_new_project.PNG)
<br/>
## Data collection
  <strong> 1. Collecting data from Esp32 can be a tediuos. Luckily, you can download and run the scripted that I've created [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) or use Webcam interface in Edge impulse. </strong>
     <br/>
  - The best results of this network is obtained atleast 70 images per class and 10% of background(other) images. To put in perspective, training a model to count 2 fingers requires 70 images of one, another 70 images of two, and atleast 20-30 images of other fingers or object look alike.
  - Images should has equal width and height otherwise it's width will be crop off when uploading to Edge Impulse. Here is snapshot of [webserver](https://github.com/San279/camera-webserver-for-esp32S3) used for data collections. Each images is 96 X 96 in dimension. 
<br/> <br/>   
  ![alt text](/Images_for_readme/webserver.PNG)
<br/> <br/> <br/>
 <strong>2. On the left tab, go to data aquisition, and upload images to Edge Impulse</strong>
 <br/> <br/> 
 ![alt text](/Images_for_readme/add_data.PNG)
  <br/> <br/>
![alt text](/Images_for_readme/upload_data.PNG)
  <br/> <br/> <br/> 
 <strong>3. Click yes for object Detection </strong>
  <br/> <br/> 
![alt text](/Images_for_readme/object_detection_tab..PNG)
  <br/> <br/>  <br/> <br/> 
## Training
  <strong> 1. On the top of the page, navigate to labeling queue and add label to each images. Keep in mind that images with non equal dimension will be crop off during this process, which is why I've equal image dimension. </strong>
     <br/> <br/>
Images with non equal dimension 320 X 240, notice the black shade on each sides of the image indicates that those parts will be crop off.
 <br/> <br/>
   ![alt text](/Images_for_readme/label_320.PNG)
    <br/> <br/>
   Images with equal dimension 96 X 96.
  <br/> <br/>
   ![alt text](/Images_for_readme/label_96.PNG)
<br/> <br/> <br/>
 <strong> 2. After labeling all images, navigate to Impulse design on the left and click on Create impulse. This will take you to a page where you can choose the size of the input model and resizing mode. </strong>
    <br/><br/>
    - Edge Impulse reccomends the size of the model should be in multiple of 8. The higher the input size, the slower the network for inferencing. But higher size has advantage of detecting multiple objects if it's presented in the frame.
 <br/> <br/>
 ![alt text](/Images_for_readme/input_size.PNG)
<br/> <br/>
Click on add a processing block and select the only option.
<br/> <br/>
 ![alt text](/Images_for_readme/add_processing.PNG)
<br/><br/>
Click on add learning block and select the first option, then save the impulse.
 <br/> <br/>
 ![alt text](/Images_for_readme/learning_block.PNG)
<br/><br/> <br/>
<strong> 4. After saving the impulse you will be directed to a new section. In this section, you can choose whether images will be train in Grayscale or RGB feature. I've left it as RGB for this project. Click on save parameters to proceed. </strong>
<br/>  <br/>
 ![alt text](/Images_for_readme/rgb.PNG)
<br/> <br/>
 After selecting the features, the page will direct you to generate feature tab, <strong> click on generate feature and you will see the graph on the right side of the page. </strong>
<br/> <br/>
<strong> 5. This graph uses K-nearest neibors algorithm to represented the similarities between each images. Notice that red dot represent finger no.1 and pink represent finger no.2. If two classes are too close to each other like the ones I've circled, the object detection model will have problems distinguish between two classes which will greatly reduce the accuracy. Thus images that overlaped has to be deleted. </strong>
<br/><br/>
 ![alt text](/Images_for_readme/feature_unedit.PNG)
<br/> <br/>
- After deleting and adding more images, the two classes should be seperated like this.
 <br/> <br/>
 ![alt text](/Images_for_readme/feature_edited.PNG)
<br/><br/> <br/>
<strong> 6. On the left panel select Object detection. These are the settings that can be customized. </strong>
  - Traning cycles indicates the number of epoch the model will go through, I've found that it is trivial to set it more than 80. I will be using 25 cycles for this project.
  - Data augentation, multiplies amount of your dataset significantly. leave this on as default.
  - Learning rate, determines how fast the model learn the features, this is best leave as just it is.
  - Validation set size, also best to leave this as default as well.
  - batch size, determines samples that will be propagated through the traning process e.g. if it's set 8 then the model will train on 1-8 images, then on the next cycle it will go through 9-16 and so forth. Batch size should be in the power of 2^n, e.g. 4, 8, 16, 32, 64, and etc. I've found that on small datasets 8 and 16 yield the best result. The batch size of 8 will be used for this project. 
<br/><br/>
 ![alt text](/Images_for_readme/best_setting.PNG)
<br/><br/>
  - Choose the model, as of now, only two FOMO models are avaiable. I will be using FOMO 0.35 for this project.
<br/><br/>
   ![alt text](/Images_for_readme/model_choice.PNG)
<br/><br/>
  - Start traning the model, this process might takes up to 20 minutes.
     <br/><br/>
   ![alt text](/Images_for_readme/100.PNG)
  <br/><br/>
  <strong> Tips to improve mode's accuracy </strong>
  - Check if each class has overlapped features, go back to step no.5.
  - Increase the datasets.
  - decrease batch size.
  - Epoch should not be more than 80 for smaller datasets
  <br/><br/><br/><br/>
## Deployment
  <strong> 1. On the left tab, navigate to Deployment and change deployment option to Arduino library. </strong>
    <br/> <br/>
   ![alt text](/Images_for_readme/deployment1.PNG)
   <br/><br/><br/>
  <strong> 2. Change target option to Esp32. </strong>
   <br/> <br/>
   ![alt text](/Images_for_readme/deployment2.PNG)
   <br/> <br/><br/>
  <strong> 3. Click on Build to start downloading the library, and you're done. </strong>
  <br/> <br/>
  I've created two libraries for testing the model in real time, please visit [FOMO-object-detect-stream-Esp32](https://github.com/San279/FOMO-object-detect-stream-Esp32) for webserver platform or [FOMO-object-detect-TFT](https://github.com/San279/FOMO-object-detect-stream-Esp32) for display on TFT screens.

## Credit
Thanks to [WIRELESS SOLUTION ASIA CO.,LTD](https://wirelesssolution.asia/) for providing AIOT board to support this project. Also thanks to [Bodmer / TFT_eSPI](https://github.com/Bodmer/TFT_eSPI/blob/master/README.md) for the TFT libraries. Scripted used for Esp32 FOMO object detection inferencing were provided by [Edge Impulse](https://edge-impulse.gitbook.io/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices). 
