Deep Learning Project Overview - Fake Vehicle Detection

My deep learning project focuses on fake vehicle detection on our roads. Vehicles with fake number plates or unregistered/expired registration numbers often traverse our roads, posing potential risks. Our goal is to detect these fraudulent vehicles using surveillance cameras or CCTV footage, as they may use number plates from different vehicle types. For instance, a four-wheeler might have the number plate of a two-wheeler.

This project is inspired by the Malayalam movie 'Joseph' which depicts a crime where criminals use fake vehicles to evade detection by the police.

Initially, I planned to utilize the Motor Vehicle Department's API for real-time vehicle registration details. However, accessing the API via the APISETU platform is restricted for private individuals. Despite our efforts to request access, approval is only granted to government-registered organizations, which is a time-consuming process. Consequently, I proceeded with a demo project using fabricated data generated from videos shot on our roads, stored as a CSV file.

Here's a breakdown of our project steps:

1. Data Collection: I took photos of different categories of vehicles (2-wheelers, 3-wheelers, 4-wheelers, Heavy Motor Vehicles) from our local roads. I opted for local photos rather than internet images to ensure accuracy since our model is trained with images from our roads. Additionally, I captured videos of vehicles for testing purposes.

2. Labeling the objects in the image: I labeled each image I took for training using the labelImg software. In each image, I labeled the number plate and the vehicle type (2-wheeler, 3-wheeler, 4-wheeler, or heavy_motor_vehicle). Thus, each image had two labels: vehicle type and number plate, totaling five classes.

3. Preparing the data for training and testing: I arranged the images and labels in text format in two separate folders. Within the Image and Label folders, I split the images and labels into Training and Validation sets. I then zipped the folders into a data.zip file and uploaded it to Google Drive for access in Google Colab for training purposes.

3. Training: I used YOLO V7 for training to identify our classes (number_plate, 2_wheeler, 3_wheeler, 4_wheeler, or heavy_motor_vehicle). YOLOv7 is a deep learning object detection algorithm, an evolution of the popular YOLO (You Only Look Once) series of models. It aims to achieve better accuracy and efficiency in object detection tasks. YOLOv7 has a highly trained weight capable of identifying about 80 classes. I modified its configuration files (coco.yaml and yolov7.yaml) to identify our five classes. Then, I started training the weights using the available GPU in Google Colab. Training took about 8 hours, even for 200 epochs (cycles).

4. Saving the YOLOv7 files and trained weights on our local machine: After training the weights, I uploaded the folders and trained weights to Google Drive from the Colab notebook and then downloaded them to our local machine.

5. Creating fake data of vehicles (for testing purposes): The data contains columns for vehicle registration number and vehicle category (2-wheeler, 3-wheeler, 4-wheeler, or heavy_motor_vehicle) as a CSV file.

6. Modifying the Code in YOLOv7 for our specific purpose: I created a new project in VS Code and modified the detect.py module (created by the YOLO team to detect objects). The modifications included:

a) Changing the weights for detection to our custom trained weights 'best.pt'.
b) Changing the source for testing from the default webcam ('0') to the path of our video shot from the road for testing purposes.
c) Reading each frame from the video using OpenCV and passing it to the code in detect.py to detect the number plate and vehicle type. I implemented a condition: if the detected number plate is inside the rectangle of the identified vehicle type, then I read the characters in the number_plate object using OCR (optical character recognition) and checked the data (detected registration number and detected vehicle category) against the data I created for testing. If the vehicle category does not match the detected category, I save the frame as a JPG file locally and append the fake vehicle data to a data frame (fake_vehicles_df) containing columns such as reg_num, vehicle_category, detected_category, and image_of_detected_vehicle (as the path of the saved JPG file). I save this as 'fake_vehicles.csv' using the to_csv function in pandas. This allows us to access data on detected fake vehicles at any time using the 'fake_vehicles.csv' file. Additionally, by cross-checking with the mParivahan portal, I can verify the reliability of the detections.

This is just a demo project demonstrating how I can identify fake vehicles on our roads. If the API of the Motor Vehicle Department is available at an organizational level and by extending the training to identify vehicle brand, model, color, etc., I believe this project idea will help identify vehicles running on our roads and could be instrumental in preventing many crimes involving fake vehicles.
