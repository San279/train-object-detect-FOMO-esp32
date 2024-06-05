
## วิธีการฝึก FOMO AI ตรวจจับวัตถุโดยใช้ Edge Impulse
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) คือ AI ตรวจจับวัตถุที่ถูกพัฒนาสำหรับไมโครคอนโทรลเลอร์มที่มีเสป็คค่อนข้างจำกัด อย่างเช่น Esp32-S3 เป็นต้น โดยให้ข้อแนะนำต่างๆ สำหรับการรวบรวมรูปภาพ การฝึก AI และการนำโมเดลไปรันบน Esp32-S3.  
<br/>
## สิงที่ต้องมีสำหรับโปรเจ็ค
 - AIOT บอร์ด Esp32-S3 หรือ Esp32 ที่มี PSRAM.
 - กล้อง OV 2640.
 - กล้องเว็ปแคม (ไม่จำเป็น).
 - บัญชีผู้ใช้ Edge Impulse account.
<br/> <br/>
## ก่อนเริ่ม
  <strong> สร้างบัญชีใน Edge Impulse และกด create new project เพื่อสร้างโปรเจ็คใหม่. </strong> 
  <br/> <br/>
  ![alt text](/Images_for_readme/create_new_project.PNG)
<br/>
## การรวบรวมรูปภาพ
  <strong> 1. เรามีสองวิธีให้เลือกในการเก็บและรวบรวมรูปภาพสำหรับฝึก AI บน Edge Impulse วิธีแรกคือเก็บรูปภาพจากกล้อง Esp32 โดยตรง โดยใช้ [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) การรวบรวมรูปภาพแบบวิธีนี้จะทำให้ AI มีความแม่นยำมากกว่า หรือวิธีที่สองคือใช้กล้องโทรศัพย์หรือกล้องเว็ปแคมจากคอมพิวเตอร์ของเรา. </strong>
     <br/>
  - เราควรกำหนดรูปภาพให้มีอย่างน้อย 70 รูปต่อวัตถุ และ 10% ของรูปภาพทั้งหมดให้เป็นภาพพื้นหลัง หรือวัตถุอื่นๆ ที่มีความคล้ายเคลียงกับวัตถุของเรา ยกตัวอย่างเช่น การฝึกโมเดล ให้นับนิ้วมือนั้น เราจะกำหนด 2 วัตถุ คือหนึ่งนิ้ว กับ สองนิ้ว โดยนิ้วเดี่ยวจะต้องมี 70 รูป และสองนิ้วจะต้องมีอีก 70 รูปภาพ และในส่วนของภาพพิ้นหลังนั้น เราจะรวบรวมภาพรูปภาพอื่นๆ เช่น ภาพพื้นหลัง สามนิ้ว สี่นิ้ว ปากกา เป็นต้น โดยใช้ 20-30 รูปภาพ
  - เพื่อให้โมเดลของเรามีความแม่นยำสูง รูปของวัตถุที่ให้ AI ตรวจจับนั้นควรมีพื้นหลัง หรือ การจัดไฟที่ต่างกันอย่างน้อย 2 รูปแบบ
  - แต่ละรูปควรมีมิติ สูง X ยาวที่เหมือนกัน เนื่องจาก Edge Impulse จะตัดส่วนความยาวของรุปให้เท่ากับความสูง ซึ่งอาจจะตัดส่วนสำคัญต่างๆ ของวัตถุนั้นออกไป สำหรับโมเดลตัวนับนิ้วมือ ผมใช้รูปที่มีมิติ 96 X 96 โดยปรับกล้อง Esp32 ใน [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3)
<br/> <br/>   
  ![alt text](/Images_for_readme/webserver.PNG)
<br/> <br/> <br/>
 <strong>2. ในช่องด้านซ้าย กดไปตรง data aquisition และเลือก upload images เพื่ออัพโหลดรูปภาพขึ้นบน Edge Impulse</strong>
 <br/> <br/> 
 ![alt text](/Images_for_readme/add_data.PNG)
  <br/> <br/>
![alt text](/Images_for_readme/upload_data.PNG)
  <br/> <br/> <br/> 
 <strong>3. ระหว่างการอัพโหลดนั้นจะมีตัวเลือกสำหรับการเทรน AI ตรวจจับวัตถุ ให้เรากด yes</strong>
  <br/> <br/> 
![alt text](/Images_for_readme/object_detection_tab..PNG)
  <br/> <br/>  <br/> <br/> 
## Training
  <strong> 1. หลังอัพโหลดรูปภาพเรียบร้อยแล้ว กดตรง labeling queue และให้วาดกล่องกับชื่อวัตถุที่จะกำหนดให้ AI ตรวจจับ รูปที่มีมิติความยาวกับความสูงที่ต่างกันจะถูกตัดออกให้มีมิติที่เท่ากัน </strong>
     <br/> <br/>
ตัวอย่างของรูปที่มีมิติที่ต่างกัน 320 X 240 เราจะเห็นสีทึบในทั้งสองฝั่งของรูป ซึ่งส่วนนั้นจะถูกตัดออกไป 
 <br/> <br/>
   ![alt text](/Images_for_readme/label_320.PNG)
    <br/> <br/>
ตัวอย่างของรูปที่ใช้ในโปรเจ็คนี่ มิติเท่ากัน 96 X 96 จะไม่ถูกปรับเปลี่ยน
  <br/> <br/>
   ![alt text](/Images_for_readme/label_96.PNG)
<br/> <br/> <br/>
 <strong> 2. หลังจากกำหนดวัตถุของเราเรียบร้อยแล้ว ให้กดไปที่ Create impulse ในช่องด้านซ้าย ในพาร์ทนี่เราสามารถกำหนดขนาดของโมเดลเรา โดยขนาดนั้นจะต้องอยู่ในผลคูณของ 8 </strong>
    <br/><br/>
    - เราสามารถกำหนดขนาดของโมเดลให้ใหญ่กว่าหรือเล็กกว่ารูปเราก้ได้ โดยจะมีข้อดีกับข้อเสียที่ตรงกันข้าม ในโมเดลใหญ่ ข้อดีคือความแม่นยำอาจจะสูงกว่า แต่จะใช้เวลาในการตรวจจับค่อนข้างนาน เมื่อเทียบกับโมเดลที่เล็กกว่า แต่ความแม่นยำน้อยกว่า สำหรับโมเดลนับนิ้วมือ เรากำหนดที่ 96 X 96 ขนาดเท่ารูปของเรา เพราะโมเดลนี่ไม่ได้มีอะไรที่ซับซ้อนมากนัก 
 <br/> <br/>
 ![alt text](/Images_for_readme/input_size.PNG)
<br/> <br/>
กดไปตรง add processing block และเลือกตัวบนสุด
<br/> <br/>
 ![alt text](/Images_for_readme/add_processing.PNG)
<br/><br/>
หลังจากนั้นกดไป add learning block and select the first option, then save the impulse.
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
  <strong> 3. Click on Build to start downloading the library, and you're done. I've created two libraries for testing the model in real time, please visit [FOMO-object-detect-stream-Esp32](https://github.com/San279/FOMO-object-detect-stream-Esp32) for streaming inference result or [FOMO-object-detect-TFT](https://github.com/San279/FOMO-object-detect-stream-Esp32) for displaying inference results to TFT screens. </strong>

## Credit
Thanks to [WIRELESS SOLUTION ASIA CO.,LTD](https://wirelesssolution.asia/) for providing AIOT board to support this project. Also thanks to [Bodmer / TFT_eSPI](https://github.com/Bodmer/TFT_eSPI/blob/master/README.md) for the TFT libraries. Scripted used for Esp32 FOMO object detection inferencing were provided by [Edge Impulse](https://edge-impulse.gitbook.io/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices). 
