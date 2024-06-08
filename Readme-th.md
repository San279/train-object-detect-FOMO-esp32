
## วิธีการฝึก FOMO AI ตรวจจับวัตถุโดยใช้ Edge Impulse
 [For English version](https://github.com/San279/train-FOMO-object-detect-esp32/blob/main/Readme.md)
 <br/>
 <br/>
  [FOMO](https://docs.edgeimpulse.com/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) คือ AI ตรวจจับวัตถุที่เหมาะสำหรับไมโครคอนโทรลเลอร์มที่มีเสป็คที่จำกัด ในโปรเจ็คนี่เราจะทำการเทรน AI ให้นับสองนิ้ว โดยให้ข้อแนะนำต่างๆ ในการรวบรวมรูปภาพจากกล้อง การฝึก AI และการนำโมเดลไปรันบน Esp32-S3 ใน Arduino
<br/>
## สิงที่ต้องมีสำหรับโปรเจ็ค
 - AIoT บอร์ด Esp32-S3 หรือ Esp32 ที่มี PSRAM
 - กล้อง OV 2640
 - กล้องเว็ปแคมหรือมือถือ (ไม่จำเป็น)
 - บัญชีผู้ใช้ Edge Impulse
## ก่อนเริ่ม
  <strong> สร้างบัญชีใน Edge Impulse และกด create new project เพื่อสร้างโปรเจ็คใหม่. </strong> 
  <br/> <br/>
  ![alt text](/Images_for_readme/create_new_project.PNG)
<br/>
## การรวบรวมรูปภาพ
  <strong> 1. การรวบรวมรูปภาพเพื่อฝึก AI ประเภทนี้ควรใช้กล้องจาก AIoT หรือ Esp32 โดยตรง โดยการใช้ไลบรารี่ [camera-webserver-for-esp32S3](https://github.com/San279/camera-webserver-for-esp32S3) เพื่อทำให้วิธีนี้นั้นง่ายขึ้น </strong>
     <br/>
  - เราควรกำหนดให้มีอย่างน้อย 70 รูปต่อวัตถุ(class) และอีก 10% เป็นรูปของพื้นหลังยกตัวอย่างเช่น การฝึกโมเดลให้นับนิ้วมือนั้นเราจะกำหนด 2 class คือหนึ่งนิ้วกับสองนิ้ว โดยนิ้วที่หนึ่งจะต้องมี 70 รูป ส่วนสองนิ้วจะต้องมีอีก 70 รูปภาพ และสุดท้ายควรมีภาพพื้นหลังหรือวัตถุอื่นๆ อีก สัก 30 รูปภาพ โดยถ้ารวมทั้งหมดเราจะมีประมาณ 170 รูปเพื่อใช้ในการฝึก AI
  - แต่ละรูปควรมีมิติ สูง X ยาวที่เหมือนกัน เนื่องจากทางเว็ปไซค์ของ Edge Impulse จะตัดส่วนความยาวให้เท่ากับความสูง ซึ่งอาจจะตัดส่วนสำคัญต่างๆ ของรูปนั้นออกไป สำหรับโปรเจ็คนี้เราใช้รูปมิติ 96 X 96
<br/> <br/>   
  ![alt text](/Images_for_readme/webserver.PNG)
<br/> <br/> <br/><br/>
 <strong>2. ในช่องด้านซ้าย กดเลือก data aquisition และกดที่ upload images เพื่ออัพโหลดรูปภาพที่เราเซฟใว้บนคอมพิวเตอร์ขึ้นบน Edge Impulse</strong>
 <br/> <br/> 
 ![alt text](/Images_for_readme/add_data.PNG)
  <br/> <br/>
![alt text](/Images_for_readme/upload_data.PNG)
  <br/> <br/> <br/> <br/> 
 <strong>3. ระหว่างการอัพโหลดจะมีตัวเลือกให้เช็คว่ารูปถ่ายนี้จะนำไปใช้ในการเทรน AI ตรวจจับวัตถุ ให้เรากด yes</strong>
  <br/> <br/> 
![alt text](/Images_for_readme/object_detection_tab..PNG)
  <br/> <br/>  <br/> <br/> <br/> 
## การฝึก AI 
  <strong> 1. กดตรง labeling queue และให้วาดกล่องกับชื่อของ class ให้หมดทุกรูปที่มีวัตถุชนิดนั้นๆ </strong>
     <br/> <br/>
ตัวอย่างของรูปที่มีมิติที่ต่างกัน 320 X 240 เราจะเห็นสีทึบในทั้งสองฝั่งของรูป ซึ่งส่วนนั้นจะถูกตัดออกไป 
 <br/> <br/>
   ![alt text](/Images_for_readme/label_320.PNG)
    <br/> <br/>
ตัวอย่างของรูปที่ใช้ในโปรเจ็คนี่ มิติเท่ากัน 96 X 96 จะไม่ถูกปรับเปลี่ยน
  <br/> <br/>
   ![alt text](/Images_for_readme/label_96.PNG)
<br/> <br/> <br/>
 <strong> 2. หลังจากกำหนด class ของเราเรียบร้อยแล้ว ให้กดไปที่ Create impulse ในช่องด้านซ้าย ในพาร์ทนี่เราสามารถกำหนดขนาดของโมเดลเรา โดยจะต้องอยู่ในผลคูณของ 8 </strong>
    <br/><br/>
    - เราสามารถกำหนดขนาดของโมเดลให้ใหญ่กว่าหรือเล็กกว่ารูปเราก้ได้ โดยจะมีข้อดีกับข้อเสียที่ตรงกันข้าม ในโมเดลใหญ่ ข้อดีคือความแม่นยำอาจจะสูงกว่า แต่จะใช้เวลาในการตรวจจับจะนานขึ้น เมื่อเทียบกับโมเดลที่เล็กกว่า ซึ่งความแม่นยำอาจจะน้อยกว่าแต่เร็วกว่า เราควรเทส AI ของเราและหาขนาดที่เหมาะสมสถานนะการณ์ ในส่วนของโมเดลนับนิ้วมือตัวนี้ เราต้องใช้ความเร็วเป็นหลักเนื่องจากเราจะใช้มันในเวลาจริง เรากำหนดขนาดที่ 96 X 96 ให้มีขนาดเท่ารูปของเรา 
 <br/> <br/>
 ![alt text](/Images_for_readme/input_size.PNG)
<br/> <br/>
กดไปตรงที่ add processing block และเลือกตัวบนสุด
<br/> <br/>
 ![alt text](/Images_for_readme/add_processing.PNG)
<br/><br/>
หลังจากนั้นกดตรง add learning block และเลือกตัวบนสุด จากนั้นให้กดไปที่ save impulse
 <br/> <br/>
 ![alt text](/Images_for_readme/learning_block.PNG)
<br/><br/> <br/>
<strong> 3. หลังจากสร้าง impulse ของเราแล้ว เราสามารถกำหนดการฝึก AI โดยใช้รูปแบบ ขาวดำ(Gray Scale) หรือ สี(RGB) ซึ่งเราควรทดลองว่าแบบไหนเหมาะสมมากกว่า เราจะใช้ รูปแบบเป็น สี(RGB) พอเลือกเสร็จให้กดไปที่ save parameters </strong>
<br/>  <br/>
 ![alt text](/Images_for_readme/rgb.PNG)
<br/> <br/>
<strong> 4. กดไปตรง generate feature เพื่อโชว์กราฟของความคล้ายเคลียงในแต่ละประเภท ในส่วนของโมเดลตัวนี้ จุดสีแดงคือหนึ่งนิ้ว ส่วนจุดสีชมพูคือสองนิ้ว ถ้าจะให้โมเดลของเรามีความแม่นยำ วัตถุแต่ละประเภทจะต้องอยู่แยกกัน ซึ่งในเคสที่วงใว้นั้น โมเดลของเราไม่สามารถแยกความแตกต่างของรูปกลุ่มนั้นได้ ซึ้งจะทำให้มีความแม่นยำต่ำ ในเคสนี้ เราควรลบรูปส่วนนั้นออกและอัพโหลดรูปเพิ่มให้มีตามขั้นต่ำ (70 รูป) และควรปรับรูปให้มีแสงไฟส่องให้ชัดขึ้น หรือ ปรับการตั้งค่าของกล้องให้เหมาะสมกับแสงไฟ </strong>
<br/><br/>
 ![alt text](/Images_for_readme/feature_unedit.PNG)
<br/> <br/>
- กราฟรูปแบบนี่จะทำให้โมเดลแยกแยะได้ดี เพราะวัตถุแต่ละประเภทนั้นไม่อยุใกล้กัน
 <br/> <br/>
 ![alt text](/Images_for_readme/feature_edited.PNG)
<br/><br/> <br/>
<strong> 5. กดไปที่ Object detection ในช้องด้านซ้าย เราจะเห็นค่าต่างๆ ที่ใช้ในการเทรน AI </strong>
  - Traning cycles คือจำนวน Step ที่โมเดลทำการเทรนนิ่ง ในส่วนนี่อย่าปรับเกิน 70 เนื่องจากไม่ค่อยมีผล ส่วนในนี้ เราจะใช้เพียงแค่ 25 ครั้ง
  - Data augmentation คือการคูณรูปภาพให้มีจำนวนมากยิ่งขึ้น โดยแต่ละรูปจะมีการปรับแสง พลิก เปลี่ยนมุม ๆลๆ ส่วนนี่จำเป็นต้องเปิดใว้เนื่องจากเรามีแค่ 170 รูป
  - Learning rate ควบุคมความเร็วที่โมเดลเราจะเรียน feature ต่างๆ ในแต่ละ training cycles สามารถอ่านเพื่มเติมได้ใน [mindphp](https://www.mindphp.com/%E0%B8%9A%E0%B8%97%E0%B9%80%E0%B8%A3%E0%B8%B5%E0%B8%A2%E0%B8%99%E0%B8%AD%E0%B8%AD%E0%B8%99%E0%B9%84%E0%B8%A5%E0%B8%99%E0%B9%8C/python-tensorflow/8491-what-is-the-learning-rate.html) สำหรับโปรเจ็คนี้เราจะทิ้งค่าเดิมของมันใว้ที่ 0.001
  - Validation set size ในส่วนนี้ หลังทุกๆ traning cycle โมเดลของเราจะทำการทดสอบความแม่นยำของแต่ละประเภทโดยการนำเปอร์เซ็นของรูปทั้งหมดมาทดสอบ ควรปล่อยใว้ที่ค่าเดิมของมัน
  - batch size คือจำนวนรูปที่โมเดลจะใช้ในการเทรนในทุกๆ traning cycle ยกตัวอย่างเช่น ค่ามันอยู่ที่ 8 โมเดลของเราจะฝึกโดยรูปที่ 1 - 8 และใน traning cycle ต่อไป จะทำการเทรน รูปที 9 - 16 โดยค่าของ Batch size ควรอยู่ในผลคูณของ 2^n เช่น 2, 4, 8, 16, 32, 64 ยิ่งเราเซ็ทค่า batch size สูง เวลาการใช้เทรนนิ่งก้จะมากขึ้นเช่นกัน ส่วนตัวแล้วสำหรับ dataset เพียงแค่ 170 รูป batch size ที่แนะนำคือ 8 16 หรือ 32 สำหรับโมเดลนี่เราจะเซ็ทใว้ที่ 8 
<br/><br/>
 ![alt text](/Images_for_readme/best_setting.PNG)
<br/><br/>
  - เรามีสองตัวเลือกของ FOMO ในส่วน alpha 0.35 หรือ 0.1 เราจะใช้โมเดล FOMO 0.35
<br/><br/>
   ![alt text](/Images_for_readme/model_choice.PNG)
<br/><br/>
  - กด start training เพื่อเริ่มการฝึกโมเดลเรา
     <br/><br/>
   ![alt text](/Images_for_readme/100.PNG)
  <br/><br/>
  <strong> เทคนิคสำหรับเพิ่มความแม่นยำโมเดล </strong>
  - ลดจำนวน batch size
  - เพิ่ม traning cycles
  - เช็คขึ้นตอนที่ 4
  - เพิ่มความละเอียด(resolution) ของรูปถาพและขนาดของโมเดล
  - เพิ่มจำนวนรูป
  <br/><br/><br/><br/>
## การ build โปรเจ็คสำหรับ Arduino
  <strong> 1. ในช่องด้านซ่ายให้กดตรงที่ deployment และเลือก change deployment เป็น Arduino library </strong>
    <br/> <br/>
   ![alt text](/Images_for_readme/deployment1.PNG)
   <br/><br/><br/>
  <strong> 2. กดไปที่ change target option และเลือก Esp32 </strong>
   <br/> <br/>
   ![alt text](/Images_for_readme/deployment2.PNG)
   <br/> <br/><br/>
  <strong> 3. กด Build เพื่อโหลดโมเดล เป็น zip และไป import บน Arduino ในส่วนของการเทสโมเดลของเรา ผมมีสอง library ให้เลือก [FOMO-object-detect-stream-Esp32](https://github.com/San279/FOMO-object-detect-stream-Esp32) สำหรับการ Stream โมเดลเราขึ้นเว็ป หรือ [FOMO-object-detect-TFT](https://github.com/San279/FOMO-object-detect-stream-Esp32) สำหรับการแสดงผลของโมเดลเราบนจอ TFT </strong>
 <br/><br/><br/><br/><br/>
## Credit
ต้องขอขอบคุณ [WIRELESS SOLUTION ASIA CO.,LTD](https://wirelesssolution.asia/) สำหรับ AIOT board และ support ในโปรเจ็คนี่ และ [Bodmer / TFT_eSPI](https://github.com/Bodmer/TFT_eSPI/blob/master/README.md) สำหรับ library จอ TFT และสกริปสำหรับการรัน FOMO จาก [Edge Impulse](https://edge-impulse.gitbook.io/docs/edge-impulse-studio/learning-blocks/object-detection/fomo-object-detection-for-constrained-devices) 
