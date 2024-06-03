## Guide to training FOMO object detection model in Edge Impulse
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) is a object detection model designed for constrained device. Due to it's low foot print and memory requirement, this model is highly suitable for AIOT box or Esp32-S3. This repository will provide simple tips for building a FOMO model in Edge Impulse, including data collection, training, and deployment.   
<br/> <br/>
## What you'll need
 - AIOT, Esp32S3 or any Esp32 series.
 - OV camera series.
 - Webcam (optional).
 - Edge Impulse account(free).
<br/> <br/>
## Before we begin
  Register a free account in Edge Impulse, and create a new project. 
  <br/> <br/>
  ![alt text](/Images_for_readme/create_new_project.PNG)
  <br/> <br/><br/> <br/>
## Data collection
  1. Collecting data from Esp32 can be a tediuos. Luckily, you can download and run the scripted that I've created [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) or use Webcam interface in Edge impulse. It is highly recomended that the training dataset contains images from Esp32 camera rather than typical Web camera. The best results of this neral network is obtained atleast 60 images per class and 10% of background(other) images. To put in perspective, training a model to count 2 fingers requires 60 images one, another 60 images of two, and atleast 20 images of other fingers or object look alike. Images should has equal width and height otherwise it's width will be crop off when uploading to Edge Impulse. Here is snapshot of [webserver](https://github.com/San279/camera-webserver-for-esp32S3) used for data collections. Each images is 96X96 in dimension.
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
  1. On the top of the page, navigate to labeling queue and add label to each images. Keep in mind that images with non equal dimension will be crop off during this process, which is why I've equal image dimension.
     <br/> <br/>
<strong> Images with non equal dimension 320 X 240, notice the black shade on each sides of the image indicates that the shaded part will be crop off.</strong>
 <br/> <br/>
   ![alt text](/Images_for_readme/label_320.PNG)
    <br/> <br/>
   <strong> Images with equal dimension 96 X 96. </strong>
  <br/> <br/>
   ![alt text](/Images_for_readme/label_96.PNG)
<br/> <br/> <br/>
 3. After labeling all images, navigate to Impulse design on the left and click on Create impulse. This will take you to a page where you can choose the size of the input model and resizing mode. FOMO reccomends the size of the model should be in multiple of 8. The higher the input size, the slower the network for inferencing. But higher size has advantage of detecting multiple objects if it's presented in the frame. Experiment with this feature as you will, as a demonstration of process we will be traning two models with different input sizes, 46X46(half the dimension of the dataset) and 96X96(equal to dataset's dimension) to showcase it's difference in accuracy and speed.<br/> <br/> <br/> <br/>
 ![alt text](/Images_for_readme/input_size.PNG)
<br/> <br/><br/> 
 <strong> Click on add a processing block and select the only option. </strong> <br/> <br/>
 ![alt text](/Images_for_readme/add_processing.PNG)
<br/><br/> <br/>
 <strong> Click on add learning block and select the first option. </strong><br/> <br/>
 ![alt text](/Images_for_readme/learning_block.PNG)
<br/><br/> <br/>
Save the impulse

<br/><br/> <br/>
## Deployment
  <strong> 1. On the left tab, navigate to Deployment and change deployment option to Arduino library. </strong>
    <br/> <br/><br/>
   ![alt text](/Images_for_readme/deployment1.PNG)
  <strong> 2. change target option to Esp32. </strong>
   <br/> <br/><br/>
   ![alt text](/Images_for_readme/deployment2.PNG)
  <strong> 3. Click on Build to start downloading the library, and you're done. I've created two libraries for testing the model, please visit [FOMO-object-detect-stream-Esp32](https://github.com/San279/FOMO-object-detect-stream-Esp32) for streaming inference result or [FOMO-object-detect-TFT](https://github.com/San279/FOMO-object-detect-stream-Esp32) for displaying inference results to TFT screens. 
