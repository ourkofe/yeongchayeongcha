# 영차영차 (식습관 기반 개인 맞춤 영양제 추천 시스템)  
[![yeongchayeongcha compliant](https://img.shields.io/badge/project-yeongchayeongcha-yellow)](https://github.com/ourkofe/yeongchayeongcha)  
<p align="center"><img src="https://user-images.githubusercontent.com/104803703/186654412-7172e77c-da2c-4dc0-a64a-91bee9031540.png" height="300px" width="300px"></p>  

----------  
## 2022 데이터 청년 캠퍼스 프로젝트 
> 상명대학교 빅데이터 분석 엔진을 활용한 바이오헬스 분야의 수량화 모델 개발 과정
-----------

## Table of Contents

- Project Description
- 사용 방법
- 코드 설명

## Project Description

1. 프로젝트 명 : 영차영차 (영양소야! 차올라!! 영양소가 차오른다!!)
2. 선정 주제 : 7일간의 식단을 기본으로 한 개인 맞춤 영양제 추천 시스템 (계산 모델)
3. 팀원 : 정하은, 이민희, 장재리, 홍성준, 홍유리
4. 프로그래밍 언어 : Python3 (알고리즘 구현은 주피터 노트북을 통해 진행)
5. 사용하는 패키지 : Numpy, Pandas, matplotlib.pyplot, sklearn.preprocessing, math, matplotlib.path, matplotlib.spines, matplotlib.transforms

## 사용 방법

1. 주피터 노트북 설치 및 실행
2. 코드 시작 전 패키지 다운로드 : Numpy, Pandas, matplotlib, sklearn, math, openpyxl
3. 코드 다운로드 : 영차영차_개인 맞춤 영양제 추천 시스템.ipynb 저장
4. 코드 시작 전 동일 폴더로 데이터 다운로드 : food_data_preprocessing_final.csv 와 nutri_clean.xlsx 저장
5. 코드 순서대로 실행 (작성한 코드에는 맞춤 영양제 추천의 예시로 키가 175이고 몸무게가 70인 저활동적인 25세 남성의 영양제 추천이 이루어진다.)
6. yournutrientinfo라는 함수 실행시 나타나는 입력칸에 7일간 섭취한 식단 입력
7. 식단을 통해 분석한 맞춤 추천 영양제와 영양제 섭취 시 영양변화 및 권장섭취량간의 비교를 확인한다.

## 코드 설명

1.
```
food_data=pd.read_csv("food_data_preprocessing_final.csv")
food_data=food_data.drop(columns=['Unnamed: 0'])
food_data
```
