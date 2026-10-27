** 기본 필수 **

**목표:** CV·valid 비교, 중요도, 예측 결과를 만들고 설명합니다.

**열 파일:** `07_project.py`  
**완성할 위치:** `B07a`, `B07b`

- **B07a**: 전처리까지 연결한 model을 train에서 교차검증하세요.
- **B07b**: 전체 train으로 학습하고 valid를 예측하세요.

**힌트:** TODO는 교차검증 1문장, 학습·예측 2문장뿐입니다. 저장부는 제공 코드입니다.

```powershell
python 07_project.py
```

**실제 정답 실행으로 확인한 출력**

```text
분할: 12 4 4
 model  CV_F1  CV_std  valid_F1
  Tree  0.50   0.204       0.0
Forest  0.25   0.000       0.0
중요도
               feature  importance
      num__input_chars       0.857
     cat__task_type_qa       0.055
cat__task_type_summary       0.088
저장: classification_comparison.csv, cv_scores.csv, validation_predictions.csv, feature_importance.csv
test는 학습·변환·예측·평가하지 않았습니다.
```

**생성 파일:** `classification_comparison.csv`, `cv_scores.csv`, `validation_predictions.csv`, `feature_importance.csv`

**완료 기준:** TODO가 실제 계산·모델 호출에 사용되고, 위 출력 및 단위·행 수가 일치하며 다음 질문에 답할 수 있어야 합니다. 반올림 끝자리 차이만 있다면 검증 버전부터 확인합니다.

- CV 평균과 valid F1의 자료가 어떻게 다른가요?   
 => CV 평균은 각각 Tree에서는 0.50, Forest에서는 0.25로 확인되었으나, Valid_F1에서는 0으로 나왔습니다. 이는 Valid의 n이 4이기에 비교 검증할 수 있는 데이터가 비교적 적어서   우연히 모두 0을 예측한 것일 수 있어 해석에 주의가 필요할 것으로 보입니다.

- 중요도는 어떤 모델에서 구했나요?   
=> Feature의 중요도는 Forest에서 구한 값으로 "num__input_chars"가 0.857로 분류 예측하는데 있어 상대적으로 중요한 것으로 보여집니다.

- test를 보관한 이유를 설명하세요.   
=> 실제 검증시에 활용하기 위해 보관함.


** 응용 **

**목표:** Dummy·Tree·Forest·XGB의 CV와 valid 결과를 재현합니다.

**열 파일:** `07_project.py`  
**완성할 위치:** `A07a`, `A07b`

- **A07a**: train 내부 교차검증 뒤 전체 train으로 학습하세요.
- **A07b**: valid의 예측을 prediction에 넣으세요.

**힌트:** 전처리·저장·반복 구조는 제공되었습니다.

```powershell
python 07_project.py
```

**실제 정답 실행으로 확인한 출력**

```text
model  CV_F1  CV_std  train_F1  valid_F1  FP  FN
 Dummy 0.7000  0.0408    0.0000    0.0000   0   6
  Tree 0.7333  0.0972    0.7097    0.3333   4   4
Forest 0.7333  0.0624    0.8000    0.2222   2   5
   XGB 0.7833  0.0667    0.7742    0.1818   4   5
CV 평균 F1 최대, 동률이면 미리 제시한 후보 순서로 선택합니다. valid는 별도 진단입니다.
저장: outputs/classification_comparison.csv, cv_scores.csv, validation_predictions.csv, feature_importance.csv
test는 학습·변환·예측·평가하지 않았습니다.
```

**생성 파일:** `outputs/classification_comparison.csv`, `outputs/cv_scores.csv`, `outputs/validation_predictions.csv`, `outputs/feature_importance.csv`

**완료 기준:** TODO가 실제 계산·모델 호출에 사용되고, 위 출력 및 단위·행 수가 일치하며 다음 질문에 답할 수 있어야 합니다. 반올림 끝자리 차이만 있다면 검증 버전부터 확인합니다.

- CV와 valid 순위가 다를 때 무엇을 보고하나요?   
=> 실제 데이터 상의 예측을 감안했을 때, 훈련 데이터 상의 CV의 값도 중요하지만, 검증 단계(Valid)에서의 F1 값도 중요할 것으로 보여집니다. 따라서, 두 지표 모두 보고되어야 하며, 재차 검증을 한다면, 모델의 예측력을 향상시키는데 도움이 될 것으로 보여집니다. 

- 동률일 때의 선택 규칙을 미리 정했나요?   
=> 기본적으로 train 상의 CV가 높은 쪽을 선택하겠지만, 만약, 동률이라면 Valid의 F1을 비롯하여 실제 loss율에 따라 최종 모델을 선택해야할 것으로 보여집니다.

- 이 표를 최종 test 성능이라고 불러도 되나요?   
=> 나온 수치만으로 봤을 때는 train과 valid의 F1의 차이가 큰 것으로 보여 좀 더 데이터를 비롯하여 전반적인 내용을 재확인해야할 것으로 보여집니다.