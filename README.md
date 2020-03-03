# Object detection Demo 

Steps to set up 
* Using Ubuntu virtual machine on Azure cloud.
```
root@instance-2:~/object-detection# cat /etc/os-release 
NAME="Ubuntu"
VERSION="18.04.4 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.4 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

* Installing python 
```
apt-get update
apt-get install python3-pip
```

* Installing following packages
```
pip3 install -U tensorflow keras opencv-python
pip3 install imageai
```

* Create a file name Detection.py 
```
from imageai.Detection import ObjectDetection
import os

execution_path = os.getcwd()

detector = ObjectDetection()
detector.setModelTypeAsYOLOv3()
detector.setModelPath( os.path.join(execution_path , "yolo.h5"))
detector.loadModel()

detections, objects_path = detector.detectObjectsFromImage(input_image=os.path.join(execution_path , "image3.jpg"), output_image_path=os.path.join(execution_path , "image3new.jpg"), minimum_percentage_probability=30,  extract_detected_objects=True)

for eachObject, eachObjectPath in zip(detections, objects_path):
    print(eachObject["name"] , " : " , eachObject["percentage_probability"], " : ", eachObject["box_points"] )
    print("Object's image saved in " + eachObjectPath)
    print("--------------------------------")
```

* Dowload yolo.h5 file  
```
wget https://github-production-release-asset-2e65be.s3.amazonaws.com/125932201/1b8496e8-86fc-11e8-895f-fefe61ebb499?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20200303%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200303T091951Z&X-Amz-Expires=300&X-Amz-Signature=ed557a52e9cdb27760d2359036a8413d8dbb035e91262928dda5b17c32e365af&X-Amz-SignedHeaders=host&actor_id=5243932&response-content-disposition=attachment%3B%20filename%3Dyolo.h5&response-content-type=application%2Foctet-stream
```

* Run the detection file
```
python3 Detection.py
```

Got some issue error 
```
root@object-detection:~/test# python3 Detection.py 
python3: Relink `/lib/x86_64-linux-gnu/libsystemd.so.0' with `/lib/x86_64-linux-gnu/librt.so.1' for IFUNC symbol `clock_gettime'
python3: Relink `/lib/x86_64-linux-gnu/libudev.so.1' with `/lib/x86_64-linux-gnu/librt.so.1' for IFUNC symbol `clock_gettime'
Segmentation fault (core dumped)
```
but able to fix it by running below command 
```
apt install python3-opencv
```

Input file 
[images/image_rit1.pdf](images/image_rit1.jpg)


After running Detection.py file you will find one newly created file image_rit_new.jpg and folder image_rit_new.jpg-objects

```
.
├── image_rit1.jpg
├── image_rit_new.jpg
└── image_rit_new.jpg-objects
    ├── apple-5.jpg
    ├── chair-4.jpg
    ├── cup-6.jpg
    ├── dining\ table-2.jpg
    ├── dining\ table-3.jpg
    ├── person-7.jpg
    └── vase-1.jpg
```

Output file 
[images/image_rit_new.pdf](images/image_rit_new.jpg)

