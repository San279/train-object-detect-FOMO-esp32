## Guide to training FOMO object detection model in Edge Impulse
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) is a object detection model designed for constrained device. Due to it's low foot print and memory requirement, this model is highly suitable for AIOT box or Esp32-S3. This repository will provide simple tips for building a FOMO model in Edge Impulse, including data collection, training, and deployment.   
<br/> <br/><br/> <br/>
## What you'll need
 - AIOT, Esp32S3 or any Esp32 series.
 - OV camera series.
 - Webcam (optional).
 - Edge Impulse account(free).
<br/> <br/><br/> <br/>
## Before we begin
  Register a free account in Edge Impulse, and create a new project. 
  <br/> <br/>
  ![alt text](/Images_for_readme/folder_directory.PNG)
  <br/> <br/><br/> <br/>
## Data collection
  Collecting data from Esp32 can be a tediuos. Luckily, you can download and run the scripted that I've created [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) or use Webcam interface in Edge impulse. It is highly recomended that the training dataset contains images from Esp32 camera rather than typical Web camera. The best results of this neral network is obtained atleast 60 images per class and 10% of background(other) images. To put in perspective, training a model to count 2 fingers requires 60 images one, another 60 images of two, and atleast 20 images of other fingers or object look alike. Images should has equal width and height otherwise it's width will be crop off when uploading to Edge Impulse. Here is snapshot of [webserver](https://github.com/San279/camera-webserver-for-esp32S3) used for data collections. Each images is 96X96 in dimension.
  <br/> <br/>   
  ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/>
  ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/>
 Upload images to Edge Impulse <br/> <br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
  <br/> <br/> <br/> <br/>
## Traning
  Label each images into classes. Keep in mind that non square images will be crop off during this process. <br/> <br/>  
  - Images with non equal dimension 320 X 240. <br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
    <br/> <br/>
  - Images with equal dimension 96 X 96.<br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/> <br/>
 After labeling all images, navigate to Impulse design on the left and click on Create impulse. This will take you to a page where you can choose the size of the input model and resizing mode. FOMO reccomends the size of the model should be in multiple of 8. The higher the input size, the slower the network for inferencing. But higher size has advantage of detecting multiple objects if it's presented in the frame. Experiment with this feature as you will, as a demonstration of process we will be traning two models with different input sizes, 46X46(half the dimension of the dataset) and 96X96(equal to dataset's dimension) to showcase it's difference in accuracy and speed.<br/> <br/> <br/> <br/>
 ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/><br/> 
 Click on Add a processing block and select the only option <br/> <br/>
 ![alt text](/Images_for_readme/folder_directory.PNG)
<br/><br/> <br/>
 Click on Add learning block and select the first option. <br/> <br/>
 ![alt text](/Images_for_readme/folder_directory.PNG)
<br/><br/> <br/>
Save the impulse
