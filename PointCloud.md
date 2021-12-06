# Overview
- Image Data : 시각적 정보로 가장 직관적 이해와 분석
  - 하지만 3차원 공간 정보를 얻는 것은 어렵다.
  - 시각적으로 표현이 가능하면서도 3차원의 공간 정보를 담고 있는 데이터가 필요하다. : Point Cloud

# PointCloud data
![image](https://user-images.githubusercontent.com/69780812/144807955-f3f00045-2bd7-4641-91bf-bfe200e8f1e7.png)
- 3D 공간 정보를 표현하는 방법 : 위치 정보를 가진 수 많은 점들을 모아 공간을 표현한다.
- 공간 정보를 가진 점들의 집합을 Point Cloud라고 한다.
  - 3D 공간 정보 수집 시 얻을 수 있는 가장 기본적인 형태
  - 수집한 모든 3D 데이터는 Point Cloud를 기반으로 후처리 작업을 통해 얻을 수 있다.

## PointCloud 데이터의 성질
### 1. 정렬되지 않고 정형화 되어있지 않다.

![image](https://user-images.githubusercontent.com/69780812/144808359-ac57d934-3167-4199-bc0a-c5fbb86ecdd0.png)
- Point Cloud 데이터는 2D 이미지 데이터와 달리 정형화 되어있지 않다.
- 2D Image Data : 정해진 격자 구조의 형태 안에 정보가 저장된다.
- Point Cloud Data : 3D 공간상의 수많은 점들을 순서 없이 기록하는 방식으로 데이터가 저장된다.
  - 딥러닝 모델에 있어서 상당히 치명적이다. (객체들의 형상, Point들 간의 상호작용 등 데이터가 가지고 있는 기하학적 특성을 파악하기가 상당히 어렵기 때문이다.)

### 2. Sparse 하다.

![image](https://user-images.githubusercontent.com/69780812/144811820-cc52f32c-1801-439f-bfaa-74271d6e8cd8.png)
- 2D image data : 격자 pixel 값이 존재하는 dense한 특성
- Point Cloud data : 매우 Sparse한 성질
  - 주어진 데이터의 3D 공간 안에 Point들에 비해 빈공간이 상당히 많다는 것을 의미한다.
  - 크기에 비해 유의미한 정보가 거의 없다는 것이다.
- 이는 전 처리 작업을 통해 Point Cloud 데이터를 정형화 데이터로 변환하여도 동일하게 가지고 있는 성질이다.
  - 이러한 데이터로 딥러닝 모델이 학습 되면 유의미한 정보를 거의 얻기 어려우며 학습 난이도만 올라간다.

## 3D 인공지능의 발전
- Point Cloud 데이터를 전부 해석하는 것이아니라 Point Cloud 데이터로부터 유의미한 정보를 추출하는 방식으로 연구가 이루어졌다.

### 1. PointNet
![image](https://user-images.githubusercontent.com/69780812/144812356-64dd3e6a-3a27-4ded-a83c-922bebdd4bc3.png)
- 데이터 형태의 변환 없이 Point Cloud 데이터를 그대로 입력해주어 학습하는 모델
- Point Cloud 데이터가 순서 없이 나열된 비정형 구조를 띔에도 불구하고 Max Pooling, Spatial Transformer Network와 같이 데이터의 기하학적 성질을 유지할 수 있는 딥러닝 네트워크들을 소개하며 Point Cloud 데이터를 직접적으로 해석하는 데 성공한 모델이다.

### 2. VoxelNet
![image](https://user-images.githubusercontent.com/69780812/144812869-e5ccd546-85bd-4147-8a99-ed4c09640950.png)
- Point Cloud 데이터로부터 Voxel Feature를 추출한 다음 이를 해석 해 물체를 검출하는 3D Object Detection 모델
- Voxel Fetarue : Data의 3차원 공간을 Voxel 단위로 나눈 후 각 Voxel 안의 점들을 Voxel Feature Encoding Layer라는 딥러닝 네트워클 거쳐 Feature map을 얻어낸 것이다.
- Point Cloud를 단순히 Voxel 형태로 전처리하는 것이 아니라 딥러닝 네트워크를 통해 Voxel 단위 Feature Map을 만들어낸 것이 특징이다.

### 3. SECOND
![image](https://user-images.githubusercontent.com/69780812/144813132-6ec71b57-9374-4723-82a3-cd1cb0950957.png)
- Sparsely Embedded Convolutional Detection
- VoxelNet가 기본적으로 구조는 동일하며 VoxelNet 내 존재하는 Convolutional Middle Layer를 기존 CNN 대신 Sparse Conv Layer라는 레이어를 사용하는 것이 특징이다.
- SPCONV는 반복되는 패턴에 대해 Rule을 설정하는 방식을 사용해 CNN에서 요구되는 계산량을 대폭 낮췄다.
  - Point Cloud 데이터와 같이 매우 Sparse한 데이터를 해석하는 데 용이한 구조를 띄고있으면서도 처리속도가 매우 빠르다.

### 4. PointPillars
![image](https://user-images.githubusercontent.com/69780812/144813383-6ed80898-2b90-4449-b9b3-08f2faac29b2.png)
- Pillar 라는 새로운 형태의 Point Cloud Encoder를 사용해 Point Cloud 데이터로부터 격자 형태의 Feature Map을 얻는다. 이를 해석해 물체를 검출하는 3D Object Detection 모델이다.
- VoxelNet이 3D Voxel 단위의 Feature Map을 얻은 반면, PointPillar는 Point Cloud 데이터를 특정 시점에서 투영시킨 다음 2D 격자 단위의 Feature Map을 얻어낸 것이 특징이다.
  - 이렇게 얻어낸 Feature Map은 이미지 데이터와 동이랗게 2D CNN을 통한 해석이 가능해져 처리 속도가 상당히 빠르다.

# Point Cloud 데이터 수집
- Point Cloud 데이터 및 이들과 함께 수집하는 정보들을 잘파악하고 어떻게 사용되는지 알아야한다.

## 1. 점 속성 정보
![image](https://user-images.githubusercontent.com/69780812/144814009-75c7dbb3-d49b-4ed4-980f-84260167ea79.png)
- 공간 상 위치 정보 x,y,z 와 속성정보를 담고있다.
- 속성 정보는 센서마다 상이하다.
  - RGB-D 카메라에서 수집하는 점의 색상 정보 R,G,B 라이다를 통해 수집하는 반사강도(Refelectance Intensity) 등이 있다.
  - 3D 데이터를 학습하는 딥러닝 ㅁ델들은 대부분 이 속성 정보를 같이 학습하므로 수집되어야한다.

## 2. Visualization
![image](https://user-images.githubusercontent.com/69780812/144814287-90bfd97f-71b9-4548-8f36-501400033caf.png)
- 가공자의 Labeling 작업이 반드시 필요하므로 점들만 보고는 그것이 무엇인지 알기 어렵다.
  - 그러므로  Point Cloud 데이터 수집 시 카메라를 동원해 이미지를 같이 수집해 객체 속성을 시각적으로 확인할 수 있어야 한다.

## 3. 센서 동기화
![image](https://user-images.githubusercontent.com/69780812/144814483-18ce10b4-4cf7-4649-95d4-a1cf79ebb7d7.png)
- Point Cloud 데이터 기반 인공지능 모델 학습은 시각화 문제로 인해 수집 장비가 최소 2개 이상 필요하다.
- 그러므로 수집 장비들 간 동기화 작업이 반드시 필요하다.
- 동기화 : 센서들의 수집 시간을 이치시켜주는 시간 동기화 작업, 센서들의 서로 다른 좌표를 일치시켜주는 공간 동기화 작업이 있다.
  - 시간 동기화의 경우, 센서들의 시간이 동기화가 된 상태에서 데이터를 수집해주어야한다.
  - 공간 동기화의 경우, 공간 동기화가 진행된 데이터를 수집하거나 수집 장비 간의 공간 동기화 작업을 위한 좌표 변환 정보를 같이 저장해준다.

## 4. Point Cloud 데이터 가공
### Point Cloud Segmentation
![image](https://user-images.githubusercontent.com/69780812/144814835-378d96e1-e84d-4b89-be3f-d221579c7f01.png)
- Point Cloud 데이터 내에 있는 모든 점드레 대한 레이블 정보를 각각 정의한다.
- Segmentation 학습을 위한 데이터 가공 시 점들의 수가 상당히 많아 데이터의 시각화가 반드시 필요하다.
- 도구 활용을 통해 효과적인 방법으로 각 점들에 레이블 속성을 부여할 수 있어야 한다.

### 3D Object Detection
![image](https://user-images.githubusercontent.com/69780812/144814970-1b9b3333-2586-429f-9264-5fcf8c5faddd.png)
- 객체의 속성을 직육면체 형태의 3D Bounding Box를 통해 정의한다.
  - 3D Bbox를 Cuboid라고도 한다.
- Cuboid 정보는 일반적으로 Cuboid의 Position, Scale, Rotation 정보만을 통해 표현하는 PSR(Position Scale Rotation) 방식으로 정의한다.
- 따라서 3D Object detection 모델의 학습을 위한 데이터셋 가공 역시 PSR 방식으로 레이블 데이터를 생성하는 것이 효과적이다.