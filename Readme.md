## Guide to training FOMO object detection model in Edge Impulse
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) is a object detection model designed for constrained device. Due to it's low foot print and memory requirement, this model is highly suitable for AIOT box or Esp32-S3. This repository will provide simple tips for building a FOMO model in Edge Impulse, including data collection, training, and deployment.   

## What you'll need
 - AIOT, Esp32S3 or any Esp32 series.
 - OV camera series.
 - Webcam (optional).
 - Edge Impulse account(free).

## Data collection
  Collecting data from Esp32 can be a tediuos. Luckily, you can download and run the scripted that I've created [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) or use Webcam interface in Edge impulse. It is highly recomended that the training dataset contains images from Esp32 camera rather than typical Web camera. The best results of this neral network is obtained atleast 60 images per class and 10% of background(other) images. To put in perspective, training a model to count 2 fingers requires 60 images one, another 60 images of two, and atleast 20 images of other fingers or object look alike. Images should has equal width and height otherwise it's width will be crop off when uploading to Edge Impulse. Here is picture of webserver used for data collections and a dataset containing total of 155 images. Each images is 96X96 in dimension.<br/> <br/>   
  ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/> 
  ![alt text](/Images_for_readme/folder_directory.PNG)

 Upload images to Edge Impulse <br/> <br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
  <br/> <br/>
## Traning
  Label each images into classes. Keep in mind that non square images will be crop off during this process. <br/> <br/>  
  - Images with non equal dimension 320 X 240. <br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
    <br/> 
  - Images with equal dimension 96 X 96.<br/> 
   ![alt text](/Images_for_readme/folder_directory.PNG)
<br/> <br/>
 After labeling all images, navigate to Impulse design on the left and click on Create impulse. This will take you to a page where you can choose   
