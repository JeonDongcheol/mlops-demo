# MLOps Platform Demo Repository
> MLOps Platform Demo 시연에서 MNIST 이미지 손글씨 분류 관련 Repository 입니다.

## 프로젝트

프로젝트 생성 과정에서 __선행__ 으로 필요한 작업은 다음과 같습니다.
- 다른 프로젝트에 __참여 요청__ 보내놓기 (승인은 하지 않음)
- 기존에 하나의 프로젝트에 참여해두고 __탈퇴 요청__ 보내놓기 (승인은 하지 않음)

프로젝트 생성할 때 입력하는 값은 __이름, 설명__ 입니다.

```
# 복사해서 붙여넣기
MNIST 손글씨 이미지 분류 프로젝트
Faster RCNN 기반 Image Classification 모델
```

-------------------

## 노트북

노트북 생성 과정에서 필요한 선수 작업은 다음과 같습니다.
- 커스텀 개발환경이미지 생성

노트북 생성할 때 입력하는 값은 __이름, 설명__ 입니다.

```
# 복사해서 붙여넣기
mnist-image-classification-notebook
MNIST 이미지 분류 모델 학습, 평가, 핸들링 작업을 위한 노트북
```

추가적으로 선택할 항목은 다음과 같습니다.
- 과제 : MNIST 손글씨 이미지 분류 프로젝트
- 자원 카드 : E.Normal
- 저장 공간 : 3GB
- 개발환경이미지 : jupyter/mnist-tensorflow-cpu

만약 커스텀 개발환경 이미지를 생성하지 않았다면, [여기]() 를 눌러주세요.

### 노트북 내에서 수행할 커맨드
> 노트북을 생성한 뒤 열기 버튼을 누르면 Jupyter Notebook 화면을 열 수 있습니다.

노트북 생성 후 Demo 시연을 위한 커맨드입니다.

```shell
# Demo Repository Clone
git clone https://github.com/JeonDongcheol/mlops-demo.git
pip install -U pip
cd mlops-demo/mnist_model
# 노트북 테스트를 위한 모델 학습 코드
python3 mnist.py
# Pipeline YAML 파일 생성 코드
python3 mnist_pipeline.py
```

```mnist.py``` 코드를 실행한 뒤 노트북 상세보기에서 __텐서보드 열기__ 버튼을 누르면 모델을 학습하는 과정에서 발생한 로그를 Tensorboard를 통해 확인할 수 있습니다.

또한, 마지막 커맨드를 수행하게 되면 ```mnist_pipeline.yaml``` 파일이 하나 생성되는데, 해당 파일을 _반드시_ 로컬에 다운로드 해줍니다.

노트북 창은 닫지 마시고 추후 있을 추가적인 시연을 위해 다시 MLOps Platform 화면으로 돌아가도록 합니다.

-------------------

## 개발환경 이미지
> 개발환경이미지는 굳이 커스텀 개발환경 이미지를 생성할 필요는 없지만, jupyter/mnist-tensorflow-cpu 이미지를 생성하기 위해 필요하다면 작업해줍니다.

개발환경이미지는 시연 대상에 따라 생성할 수도 있는데, 일부 패키지는 가끔 정상 빌드가 안될 수 있습니다. (Python Version & GPU 여부 등 오류 발생 가능)

개발환경 이미지에서 입력하는 값은 __이름, 설명__ 입니다.

```
jupyter/mnist-tensorflow-cpu
MNIST Image Classification을 위한 Jupyter Tensorflow CPU 개발환경 이미지
```

추가로 선택할 항목은 다음과 같습니다.

- 기반 개발환경 이미지 : jupyter-tensorflow-cpu
- 패키지 : numpy(1.19.5), matplotlib(3.6.1), kfp(1.8.14), Pillow(9.3.0)
- 태그 : tensorflow, matplotlib, kfp

생성 후 잠시 기다리면 정상적으로 이미지가 넥서스로 빌드됩니다.

-------------------

## 하이퍼파라미터 튜닝

하이퍼파라미터 튜닝에서 입력하는 값은 __이름, 설명, 튜닝 목표, 튜닝 범위__ 입니다.

```
mnist-classification-hyperparam
MNIST 손글씨 분류 모델 하이퍼파라미터 튜닝 (num_hidden_layer_1, num_hidden_layer_2, dropout, learning_rate, epoch)
```

- 튜닝 목표 : accuracy 0.965
- 튜닝 범위

| `하이퍼파라미터`    | `type`   | `min` | `max`  | `step` |
|--------------------|--------|------|------|----|
| num_hidden_layer_1 | integer | 64   | 256  |64|
| num_hidden_layer_2 | integer | 64   | 256  |64|
| dropout | double | 0.25 | 1.0  | 0.25 |
| learning_rate | double | 0.01 | 0.1  | 0.01 |
| epoch | integer | 1000  | 3000 |500|

추가적으로 설정해줄 항목은 다음과 같습니다.

- 자원 카드 : H.Normal
- Trial Thresholds : Basic
- 튜닝 코드 : (노트북)mnist-image-classification-notebook > mlops-demo/mnist_hyper_tuning.py 선택

하이퍼파라미터 튜닝 과정은 반드시 지켜볼 필요는 없으며, 필요에 따라 다른 컨텐츠들을 먼저 시연하다가 결과를 확인해도 됩니다.

-------------------------------

## 파이프라인
> 파이프라인은 파이프라인 생성 -> 실행 -> 스케줄 순서로 갑니다. 스케줄은 가급적 후반부로 미루면 좋습니다.

### 파이프라인 목록

파이프라인 목록에서 입력할 값은 __이름, 설명__ 이며, 템플릿은 새롭게 추가해서 __이름, 설명__ 을 추가로 입력해서 생성해줍니다.

```
# 파이프라인 생성
mnist-image-classification-pipeline
Pipeline for MNIST Image Classification

# 파이프라인 템플릿 추가
mnist-template
mnist pipeline template
```

초반에 노트북에서 다운 받았던 __YAML 파일__ 을 불러와주고 난 뒤, 템플릿을 추가하고 추가된 템플릿을 선택한 다음에 __그래프 및 YAML 파일__ 을 한 번 씩 눌러줍니다.

### 실행

파이프라인을 생성하게 되면 목록에서 새롭게 추가된 파이프라인의 __실행__ 버튼을 눌러줍니다. 실행 버튼을 누르면 실행을 수행하기 위한 실험 설정이 나오는데, __MNIST__ 라는 실험을 새롭게 추가해줍니다.

파이프라인 실행은 수행되는 과정이 중요하기 때문에 반드시 __상세__ 버튼을 눌러서 실행 과정을 볼 수 있도록 합니다.

### 스케줄

스케줄을 생성할 때 입력하는 값은 __이름, 설명__ 입니다.

```
mnist-pipeline-schedule
MNIST Image Classification Model Workflow. 1 Weeks
```

추가적으로 설정할 값은 다음과 같습니다.

- 파이프라인 선택 : mnist-image-classification-pipeline
- 실행 간격 : 1, Weeks
- 최대 동시실험 : 1

-----------

## 하이퍼파라미터 튜닝 후 모델 학습

하이퍼파라미터를 튜닝 한 뒤에 __상세__ 버튼을 눌러 하이퍼파라미터 튜닝 값을 확인해주고 노트북으로 돌아와 ```mnist.py``` 를 다시 수행해주는데, 이번에는 하이퍼파라미터 튜닝된 값을 인자로 넣어서 수행합니다.

```
python3 mnist.py --num_hidden_layer_1=192 --num_hidden_layer_2=64 --dropout=0.4905680409621578 --learning_rate=0.09105850855225119 --epoch=2000
```

----------

## model serving

모델 서빙에서 입력할 값은 __이름, 설명__ 입니다.

```
mnist-model-server
MNIST 이미지 분류 모델 예측 API 서버
```

추가로 설정할 항목은 다음과 같습니다.

- 모델 서빙 엔진 : Tensorflow Serving
- 모델 경로 : (노트북) mnist-classification-notebook > mlops-demo/mnist_model
- 자원 카드 : I.Normal

### 모델 서빙 테스트

모델 서버가 생성되면, 노트북으로 돌아와 ```model_serve.ipynb``` 를 열어준 뒤에 __URL__ 부분을 예측 API에 나온 URL로 바꿔줍니다. 그리고 해당 코드를 실행하면, 입력 데이터를 이미지로 받고 이미지 데이터 분류 결과를 받을 수 있습니다.
   
다른 이미지를 사용하려면 __mnist_nums__ list의 숫자를 0~9의 숫자 중에서 선택해줍니다.
