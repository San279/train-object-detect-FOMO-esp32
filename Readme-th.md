
## วิธีการฝึก FOMO AI ตรวจจับวัตถุโดยใช้ Edge Impulse
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) คือ AI ตรวจจับวัตถุที่ถูกพัฒนาสำหรับไมโครคอนโทรลเลอร์มที่มีเสป็คค่อนข้างจำกัด อย่างเช่น Esp32-S3 เป็นต้น โดยให้ข้อแนะนำต่างๆ สำหรับการรวบรวมรูปภาพ การฝึก AI และการนำโมเดลไปรันบน Esp32-S3 ใน Arduino
<br/>
## สิงที่ต้องมีสำหรับโปรเจ็ค
 - AIOT บอร์ด Esp32-S3 หรือ Esp32 ที่มี PSRAM
 - กล้อง OV 2640
 - กล้องเว็ปแคมหรือมือถือ (ไม่จำเป็น)
 - บัญชีผู้ใช้ Edge Impulse
<br/> <br/>
## ก่อนเริ่ม
  <strong> สร้างบัญชีใน Edge Impulse และกด create new project เพื่อสร้างโปรเจ็คใหม่. </strong> 
  <br/> <br/>
  ![alt text](/Images_for_readme/create_new_project.PNG)
<br/>
## การรวบรวมรูปภาพ
  <strong> 1. เรามีสองวิธีให้เลือกในการเก็บและรวบรวมรูปภาพสำหรับฝึก AI บน Edge Impulse วิธีแรกคือเก็บรูปภาพจากกล้อง Esp32 โดยตรง โดยใช้ [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) การรวบรวมรูปภาพแบบวิธีนี้จะทำให้ AI มีความแม่นยำมากกว่า หรือวิธีที่สองคือใช้กล้องโทรศัพย์หรือกล้องเว็ปแคมจากคอมพิวเตอร์ของเรา. </strong>
     <br/>
  - เราควรกำหนดรูปภาพให้มีอย่างน้อย 70 รูปต่อวัตถุ และ 10% ของรูปภาพทั้งหมดให้เป็นภาพพื้นหลัง หรือวัตถุอื่นๆ ยกตัวอย่างเช่น การฝึกโมเดล ให้นับนิ้วมือนั้น เราจะกำหนด 2 วัตถุ คือหนึ่งนิ้ว กับ สองนิ้ว โดยนิ้วเดี่ยวจะต้องมี 70 รูป และสองนิ้วจะต้องมีอีก 70 รูปภาพ และในส่วนของภาพพิ้นหลังนั้น เราจะรวบรวมภาพรูปภาพอื่นๆ เช่น ภาพพื้นหลัง สามนิ้ว สี่นิ้ว ปากกา เป็นต้น โดยใช้ 20-30 รูปภาพ
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
  <strong> 1. หลังอัพโหลดรูปภาพเรียบร้อยแล้ว กดตรง labeling queue และให้วาดกล่องกับชื่อวัตถุที่จะกำหนดให้ AI ตรวจจับให้กับทุกๆรูป และกด save label สำหรับรูปพื้นหลังหรือวัตถุอืนๆ ให้เรากด save label โดยไม่ต้องวาด </strong>
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
หลังจากนั้นกดไป add learning block และเลือกตัวบนสุด จากนั้นให้กดไปที่ save impulse
 <br/> <br/>
 ![alt text](/Images_for_readme/learning_block.PNG)
<br/><br/> <br/>
<strong> 3. หลังจากสร้าง impulse ของเราแล้ว เราสามารถกำหนดการเทรน AI โดยจะใช้รูปแบบ ขาวดำ(Gray Scale) หรือ สี(RGB) สำหรับโมเดลนี้เราจะใช้ รูปแบบเป็น สี(RGB) พอเลือกเสร็จให้กดไปที่ save parameters </strong>
<br/>  <br/>
 ![alt text](/Images_for_readme/rgb.PNG)
<br/> <br/>
<strong> 4. กดไปตรง generate feature เพื่อโชว์กราฟของความใกล้เคลี่ยงของรูปภาพเราทั้งหมด ด้วย K-nearest neighbor Algorithm จุดสีแดงคือนิ้วเดี่ยว ส่วน จุดสีชมพูคือสองนิ้ว โดยถ้าจะให้โมเดลของเรามีความแม่นยำ วัตถุแต่ละประเภทจะต้องอยู่แยกกัน แต่ตรงที่ผมวงใว้คือรูปที่โมเดลไม่สามารถแยกความแตกต่างกันได้ เนื่องจากรูปของสองวัตถุคล้ายกันเกินไป ในเคสแบบนี่เราต้องลบรูปส่วนนั้นออกไปและอัพโหลดรูปใหม่โดยการปรับแสงไฟ หรือ พื้นหลังให้มีความต่างกันมากขึ้น </strong>
<br/><br/>
 ![alt text](/Images_for_readme/feature_unedit.PNG)
<br/> <br/>
- ตอนนี้โมเดลจะสามารถแยกแยะได้เพราะรูปแต่ละประเภทนั้นไม่ได้อยุใกล้กัน
 <br/> <br/>
 ![alt text](/Images_for_readme/feature_edited.PNG)
<br/><br/> <br/>
<strong> 5. กดไปที่ Object detection ในช้องด้านซ้าย เราจะเห็นค่าต่างๆ ที่ใช้ในการเทรน AI </strong>
  - Traning cycles คือจำนวน Step ที่โมเดลทำการเทรนนิ่ง ในส่วนนี่ อย่าปรับเกิน 70 เนื่องจากไม่ค่อยมีผล ส่วนในโมเดลนับนิ้ว เราจะใช้เพียงแค่ 25 ครั้ง
  - Data augmentation คือการคูณรูปภาพของเราให้มีจำนวนมากกขึ้น โดนแต่ละรูปจะมีการปรับแสง พลิก เปลี่ยนมุม ๆลๆ ส่วนนี่จำเป็นต้องเปิดใว้เนื่องจากเรามีแค่ 170 รูป
  - Learning rate ควบุคมความเร็วที่โมเดลเราจะเรียน feature ต่างๆ ในทุก step ของ training cycles สามารถอ่านเพื่มเติมได้ใน [mindphp](https://www.mindphp.com/%E0%B8%9A%E0%B8%97%E0%B9%80%E0%B8%A3%E0%B8%B5%E0%B8%A2%E0%B8%99%E0%B8%AD%E0%B8%AD%E0%B8%99%E0%B9%84%E0%B8%A5%E0%B8%99%E0%B9%8C/python-tensorflow/8491-what-is-the-learning-rate.html) สำหรับโปรเจ็คนี่เราจะทิ้งค่าเดิมของมันใว้ที่ 0.001
  - Validation set size แบ่งเป็นเปอร์เซ็น ในส่วนนี่หลังจากทุกๆ traning cycle โมเดลของเราจะทำการทดสอบความแม่นยำของแต่ละวัตถุ ควรปล่อยใว้ที่ค่าเดิมของมัน
  - batch size คือจำนวนรูปที่โมเดลของเราจะทำการเทรนในทุกๆ traning cycle ยกตัวอย่างเช่น เราเซ็ทค่าใว้ที่ 8 โมเดลของเราจะเทรนรูปที่ 1 - 8 และใน cycle ต่อไป จะทำการเทรน รูปที 9 - 16 โดยค่าของ Batch size ควรอยู่ในผลคูณของ 2^n เช่น 2, 4, 8, 16, 32, 64, และ ๆลๆ ยิ่งเราเซ็ทค่า batch size สูง เวลาการใช้เทรนนิ่งก้จะมากขึ้นเช่นกัน ส่วนตัวแล้วสำหรับ dataset เพียงแค่ 170 รูป batch size ที่แนะนำคือ 8 16 หรือ 32 สำหรับโมเดลนี่เราจะเซ็ทใว้ที่ 8 
<br/><br/>
 ![alt text](/Images_for_readme/best_setting.PNG)
<br/><br/>
  - เรามีสองตัวเลือกของ FOMO ในส่วน alpha 0.35 หรือ 0.1 เราจะใช้โมเดล FOMO 0.35
<br/><br/>
   ![alt text](/Images_for_readme/model_choice.PNG)
<br/><br/>
  - กด start training เพื่อเริ่มการเทรนโมเดลเรา
     <br/><br/>
   ![alt text](/Images_for_readme/100.PNG)
  <br/><br/>
  <strong> เทคนอคในการปรับความแม่นยำของโมเดลเรา </strong>
  - เช็คว่าแต่ละรูปนั้นมีความคล้ายคลึงกันเกินไปไหม ในเสต็ปที่ 4
  - เพิ่มจำนวนรูป
  - ลดจำนวน batch size
  - เพิ่ม traning cycles ไม่ควรเกิน 80
  <br/><br/><br/><br/>
## Deployment
  <strong> 1. ในช่องก้านซ่ายให้กดไปที่ deployment และเลือก change deployment เป็น Arduino library. </strong>
    <br/> <br/>
   ![alt text](/Images_for_readme/deployment1.PNG)
   <br/><br/><br/>
  <strong> 2. กดไปที่ change target option และเลือก Esp32. </strong>
   <br/> <br/>
   ![alt text](/Images_for_readme/deployment2.PNG)
   <br/> <br/><br/>
  <strong> 3. กด Build เพื่อโหลดโมเดล เราจะได้มาเป็น zip file เพื่อไปใช้บน Arduino ในส่วนของการเทสผมมีสอง library ให้เลือก [FOMO-object-detect-stream-Esp32](https://github.com/San279/FOMO-object-detect-stream-Esp32) สำหรับการ Stream โมเดลเราขึ้นเว็ป หรือ [FOMO-object-detect-TFT](https://github.com/San279/FOMO-object-detect-stream-Esp32) สำหรับการแสดงผลของโมเดลเราบนจอ TFT ของ AIOT </strong>

## Credit
ต้องขอขอบคุณ [WIRELESS SOLUTION ASIA CO.,LTD](https://wirelesssolution.asia/) สำหรับ AIOT board และ support ในโปรเจ็คนี่ และ [Bodmer / TFT_eSPI](https://github.com/Bodmer/TFT_eSPI/blob/master/README.md) สำหรับ library จอ TFT. Scripted used for Esp32 FOMO object detection inferencing were provided by [Edge Impulse](https://edge-impulse.gitbook.io/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices). 
