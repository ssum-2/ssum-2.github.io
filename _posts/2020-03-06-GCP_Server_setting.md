---
title: "Google Cloud Platform에서 GPU 환경 구축하기(feat. Pytorch)"
classes: wide
categories:
  - GCP
  - Server
  - 파이토치 첫 걸음
tags:
  - python
  - anaconda3
  - pytorch
author_profile : true
use_math : true
sidebar:
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
last_modified_at: 2020-03-05
---

# google cloud platform

---

### 1. 개요

> `파이토치 첫 걸음`을 시작하면서 그냥 구글 코랩을 사용하기에는 뭔가 심심한 느낌이 들었다. 별도의 환경설정 없이 코랩에서 주어진 환경 그대로 사용하는 건 개발자로써 용납할 수 없었다...! GPU도 사용해야하고, local에서는 불가능할 것 같아서 이것 저것 찾아보다가 `Google Cloud Platform`을 알게 되었다.

- **Google Cloud Platform?**

  구글 클라우드 플랫폼은 AWS와 같은 클라우드 서버를 대여해주는 것으로 비교적 저렴한 가격에 GPU가 달린 가상환경을 구축할 수 있다.

  구글 계정으로 가입할 수 있으며 첫 이용시에는 **$300**를 충전시켜주면서 해당 금액만큼 무료로 사용할 수 있게 해준다. 

  ​		_가성비 넘치는 서버 구축 삽가능,,,_

- **그래서 어떻게 하는데?**

  1. GCP 메인 화면에서 `IAM 및 관리자 > 할당량`으로 접근
  
     ![Imgur1](https://imgur.com/9vSaBvw.png)
     
  2. 할당량 수정을 위해 계정을 업그레이드 한다 (별거 아님. 결제정보 등록하게 하는데 카드 등록하면 됨)

  3. 측정항목에서 `gpu`를 입력해서 GCP에서 실제로 사용할 GPU 종류 및 `GPUs(all regions)`를 체크한다.
  
     ![Imgur2](https://imgur.com/FTXij3i.png)
     
  4. 서비스 위치에 따라서 가격이 달라지므로 가장 저렴한 `us-east1` 또는 `us-west1`, `us-central` 중에 원하는 GPU 모델을 선택하고, `GPUs (all regions), 글로벌` 까지 함께 선택한 뒤, 할당량 증가 신청을 한다.

     간단한 유저정보를 입력하면 할당량 한도를 입력하는 창이 뜨는데, 요청 설명에 대충 웅앵웅 버무려주면 된다.
     
     ![Imgur3](https://imgur.com/igOq207.png)
     
  5. working day 기준 최대 2일이 소요되며, 등록한 메일로 할당량 증가가 승인 되었다고 안내해준다.
  
     ![Imgur4](https://imgur.com/6flLwY5.png)
     
  6. 할당량이 증가되었음을 확인했다면 `Compute Engine > VM 인스턴스`로 들어가 새 인스턴스를 생성한다
  
     ![Imgur5](https://imgur.com/f5Jpgot.png)
     
     |                   |                                           |
     | ----------------: | ----------------------------------------- |
     |        **region** | us-east1(사우스캐롤라이나), us-east1-d    |
     |           **cpu** | N1, n1-standard-4(vCPU 4개, 15GB 메모리)  |
     |           **gpu** | NVIDIA Tesla K80, 1개                     |
     |          **disk** | Ubuntu-1604-xenial-v20200108, 100GB       |
     |    **가용성정책** | 선점 사용 (24시간 동안 선점, 비용 저렴함) |
     |        **방화벽** | HTTP/HTTPS 트래픽 허용                    |
     | **네트워크 태그** | tensorboard, jupyter, pytorch             |
     |                   |                                           |
  
- **gcloud로 terminal을 통해 VM 인스턴스에 접속하기**

  0. VM 인스턴스 실행

  1. Google Cloud SDK 설치

     -  **[SDK Document](https://cloud.google.com/sdk/downloads?authuser=2&hl=ko#windows)**

     - ```python
       curl https://sdk.cloud.google.com | bash
       
       exec -l $SHELL # 쉘 재시작
       
       gcloud init
       #계정설정, 프로젝트 선택
       ```

       > 질문이 등장하면 무조건 y 또는 공백 (환경변수 셋팅 여부에 대한 내용임)

  2. GCP 페이지 > vm 인스턴스 > SSH > gcloud 명령보기

     ```python
     #터미널에 입력
     gcloud beta compute --project "my-project-1543277834092" ssh --zone "us-east1-d" "pytorch-first-step-k80"
     ```

  3. 최초 접속 시 비밀번호 설정; 공백으로 설정가능



### 2. PyTorch 실습 환경 설정

1. locale 설정

   - ```
     sudo apt-get install language-pack-ko
     sudo locale-gen ko_KR.UTF-8
     ```

2. Anaconda 설치

   - 아나콘다를 사용하는 걸 별로 안좋아하지만 교재에서 아나콘다를 사용하길래 그냥 똑같이 따라감

     ```python
     wget http://repo.anaconda.com/archive/Anaconda3-5.3.1-Linux-x86_64.sh
     bash Anaconda3-5.3.1-Linux-x86_64.sh
     ```

     설치 과정을 묻는 것에 모두 `yes` 

   - 환경변수 세팅

     ```python
     export PATH=/home/username(ubuntu)/anaconda3/bin:$PATH
     source .bashrc
     ```

     나는 터미널에 gcloud setting을 하고난 뒤에 터미널에서 접속해서 그런지 기본 path에 사용자명이 들어가있었음. anaconda3이 설치된 디렉토리를 찾아 넣으면 됨

   - 가상환경 만들기

     ```python
     conda create -n pytorch python=3.7
     source activate pytorch #가상환경 실행
     
     source deactivate pytorch #가상환경 종료; 가상환경명 생략 가능
     ```

     > 가상환경 실행할 때 source가 없다는 식으로 오류가 난다면 맨 앞에 conda 명령어를 붙여줘야함

3. CUDA 설치

   - CUDA 10.0 version 설치

     ```python
     lspci | grep -i NVIDIA
     
     CUDA_REPO_PKG=cuda-repo-ubuntu1604_10.0.130-1_amd64.deb
     
     wget -o /home/username/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} # cuda설치파일 다운로드; 교재에는 root 하위의 tmp폴더에 받았지만 나는 그냥 유저명폴더 하위로 받음
     
     sudo dpkg -i /home/ssum/${CUDA_REPO_PKG} #패키지 해제
     
     sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub #key 설정
     
     rm -f /home/ssum/${CUDA_REPO_PKG} #설치파일 삭제
     
     #설치
     sudo apt-get update
     sudo apt-get install cuda-drivers
     sudo apt-get install cuda
     
     #설치 후 재부팅
     sudo reboot
     ```

   - CUDA environment path 설정

     - ```python
       # 1) 명령어로 path 잡아주기
       echo 'export CUDA_HOME=/usr/local/cuda' >> ~/.bashrc
       echo 'export PATH=$PATH:$CUDA_HOME/bin' >> ~/.bashrc
       echo 'export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
       source ~/.bashrc
       ```

     - ```python
       # 2) 직접 .bashrc 파일 변경하기
       $ vim .bashrc
       
       #-- 추가 --
       
       export CUDA_HOME=/usr/local/cuda
       export LD_LIBRARY_PATH=${CUDA_HOME}/lib64 
       
       PATH=${CUDA_HOME}/bin:${PATH} 
       export PATH 
       
       #저장후 종료
       
       $ source .bashrc
       ```

     - path 설정이 가상환경이 새로 시작될 때마다 초기화 되므로 아예 .bashrc 파일을 변경해야함
     - 두가지 방법 중 편한 방법으로 설정

   - CUDA 설치 확인

     ```python
     nvcc --version
     
     nvidia-smi
     
     cat /usr/local/cuda/version.txt
     ```

4. cuDNN 설치

   - 설치파일을 https://developer.nvidia.com/cudnn 에서 직접 다운로드 (runtime lib, dev lib)

     - 교재에서는 CUDA 10.0 ver. 7.4.1.5 파일을 받음

     - 설치파일을 GCP 서버 안으로 전송 

       - SSH > 콘솔 > 우측상단 톱니바퀴 > 파일 업로드

       - `scp -I ~/.ssh/my-ssh-key [LOCAL_FILE_PATH] [USERNAME]@[IP_ADDRESS]:~`

         > 이 방법이 가장 확실하긴 한데... 전송속도가 너무 느리다. 

     - ```python
       sudo dpkg -i libcudnn7_7.4.1.5-1+cuda10.0_amd64.deb
       
       cat /usr/include/x86_64-linux-gnu/cudnn_v*.h | grep CUDNN_MAJOR -A 2 # version 확인
       ```

   - 명령어로 설치하기

     ```python
     sudo sh -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" >> /etc/apt/sources.list.d/cuda.list'
     cat /etc/apt/sources.list.d/cuda.list
     
     #출력결과
     >>> deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /
     >>> deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /
     
     # 위 결과처럼 나오면 설치
     sudo apt-get update
     sudo apt-get install libcudnn7-dev
     ```

5. PyTorch 설치

   - `conda install pytorch torchvision cuda100 -c python`

   - ```python
     #python 실행
     import torch
     cpu_tensor = torch.zeros(2,3)
     device = torch.device("cuda:0")
     gpu_tensor = cpu_tensor.to(device)
     print(gpu_tensor)
     ```

6. Jupyter 등 환경설정

   - 방화벽에 Jupyter 세팅 추가

     - ![Imgur](https://imgur.com/YlJ6ut4.png)
     - 다른 건 다 기본값으로 설정, `tag`, `소스 IP 범위 `, `지정된 프로토콜 및 포트 > tcp` 만 바꿔줌

   - 주피터 configuration file 생성

     ```python
     jupyter notebook --generate-config
     
     #위 명령어 실행 후 결과
     >>> Writing default config to: /home/~~~/.jupyter/jupyter_notebook_config.py
     ```

   - config 파일 수정

     ```python
     vi ~/.jupyter/jupyter_notebook_config.py
     
     # 아래 내용을 config파일 최상단이나 최하단에 입력
     c = get_config()
     c.NotebookApp.ip = '할당 받은 외부IP'
     c.NotebookApp.open_browser = False
     c.NotebookApp.port = 8888 # 방화벽에 입력한 port
     ```

   - jupyter 실행

     ```python
     #jupyter notebook 실행
     jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root
     #jupyter lab 실행 (동일함)
     jupyter lab --ip=0.0.0.0 --port=8888 --allow-root
     ```

     로컬 웹브라우저에 http://외부IP:8888 을 입력하면 server의 주피터에 접근할 수 있음

     주피터 최초 실행 시 pw 설정

     ![img](https://k.kakaocdn.net/dn/AIYjO/btqvbJoChAd/PpG2IplYVbGCweSG40Ye70/img.jpg)

     > `token=` 하위의 암호화토큰을 복사

     ![img](https://k.kakaocdn.net/dn/bjBtfQ/btqvdkBpXyG/UV2bawSrMVoiPHnw1UjVY0/img.png)

     > 위에서 복사한 `token`을 입력한 뒤 new password에 원하는 비밀번호를 설정

   - 외부IP 고정하기

     ![Imgur](https://imgur.com/BrBnpnY.png)

     > VPC 네트워크 > 외부 IP 주소 > 고정 주소 예약

     - 고정 주소 예약 - 이름 작성 - 지역(인스턴스 설정 지역) - 연결 대상(연결하고자 하는 인스턴트 클릭) - 예약(완료)

       콘솔 - 인스턴스로 가서 확인

     - cf) 인스턴스를 아직 생성하지 않고 초기에 셋팅하고자 하는 경우

       - GCP 콘솔 - 네트워킹 - VPC 네트워크 - 외부 IP 주소
  - 고정 주소 예약 - 이름 작성 - 지역(인스턴스 설정 지역) - 예약
       - 콘솔 - 인스턴스(생성) - 생성 중 옵션 - 네트워킹 - 네트워크 인터페이스 - 외부 IP탭(만들어 놓은 고정 IP 네임으로 설정) - 완료
   
7. 자주 사용하는 명령어

   - 참고; [gcp-command](https://codechacha.com/ko/gcp-command/)

   - 파일 전송(복사)

     - 로컬 -> 인스턴스

       ```python
       $ gcloud compute scp [LOCAL_FILE_PATH] [INSTANCE_NAME]:~/
       ```

     - 인스턴스 -> 로컬

       ```python
       $ gcloud compute scp --recurse [INSTANCE_NAME]:[REMOTE_DIR] [LOCAL_DIR]
       ```

       





​	

