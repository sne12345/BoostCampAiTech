## 1. 3d
### 1.1 why 3d important?
: 우리 세계는 3d로 이루어져있기 때문에  

* application 
: AR / VR, medical application

* 이미지는 projection of 3d world
* 2d 사진 두 개가 있으면 3d point를 구할 수 있음(Triangulation)
* if multiple view?

### 1.3 how is 3d represented in computer?


* triangle mesh
: vertex와 edge로 구성되어 있고, 모든 연결은 삼각형로 되어있음
![image](https://user-images.githubusercontent.com/51853700/133721983-d385f677-43e5-43d1-8998-9e9e3b3cf506.png)



### 1.4 3d datasets

* shapeNet (51,300 3D models with 55 categories)
* partNet : segmentation instance도 나눠져 있음 
* SceneNet : indoor images => RGB-Depth로 시뮬레이션
* ScanNet : RGB-Depth, 2.5million views from 1500 scans
* Outdoor datasets => LiDAR, labeled, bounding box


## 2. 3d tasks
### 2.1 3d recognition
![image](https://user-images.githubusercontent.com/51853700/133730653-07b5650e-859e-4168-96f2-350b187b3173.png)

* 3d object recognition => volumetric CNN
![image](https://user-images.githubusercontent.com/51853700/133730696-c5882f83-cf1c-47bc-9796-e87ae4967e7e.png)


### 2.2 3d object detection
: detect in 2d or 3d spaces
: 무인차 application에서 사용

### 2.3 3d sementic segmentation
![image](https://user-images.githubusercontent.com/51853700/133730817-39d224a7-95cc-4d20-abac-fab50c79681d.png)

### 2.4 conditional 3d generation

* Mask R-CNN (remind)
: box, classes and mask
![image](https://user-images.githubusercontent.com/51853700/133731107-5f0e7540-f8b8-4da9-9bac-e9de7ae7d9b4.png)

* Mesh R-CNN
: input - 2d image
: output - 3d meshed of detected object
![image](https://user-images.githubusercontent.com/51853700/133730884-15e8ca74-d16e-41af-8462-0e0aa79455cd.png)
![image](https://user-images.githubusercontent.com/51853700/133731175-8e54e680-01c6-4a20-b9ae-da90f0f0175a.png)


* more complex 3d reconstruction tasks
![image](https://user-images.githubusercontent.com/51853700/133731402-cff356b6-6015-46e1-b6f9-f06d4eb06e55.png)
