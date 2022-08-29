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

1. 음식 데이터 불러오기
```python
food_data=pd.read_csv("food_data_preprocessing_final.csv")
food_data=food_data.drop(columns=['Unnamed: 0']) #인덱스 번호가 이중 생성되지 않도록 데이터 내의 인덱스 번호 열 
food_data
```
2. 입력한 식단으로 일상섭취량 추출값 반환하기
```python
def day_7_nutrient(food_data):
    food=[x for x in input("공백을 구분자로 먹은 식단을 입력바랍니다: ").split()] #일상섭취량을 구하기 위해 식단 입력 (일상섭취량 : 하루동안 섭취하는 평균적인 섭취량)
    food_result=pd.DataFrame() #day_7_nutrient 함수를 통한 출력값으로 데이터프레임 생성
    for i in range(len(food_data)): 
        for j in range(len(food)):
            if food_data['식품명'][i]==food[j]:    
                food_result_=food_data.loc[i]
                food_result=pd.concat([food_result,food_result_],axis=1) #음식 데이터와 일치하는 이름의 음식의 행들을 모아 새로운 데이터 프레임 생성

    food_result=food_result.T
    day7_mean=pd.DataFrame(food_result.iloc[:,1:].sum()/7)
    day7_mean.columns=['7일영양소평균'] #입력받은 식단 데이터를 다 합한 뒤 7로 나눈 하루 평균 섭취량(일상섭취량) 추출
    return day7_mean #일상섭취량 반환
```
3. 개인별 권장섭취량 추출값 반환하기
```python
def recommend_intake(age,sex,height,weight,PA): #개인별 권장섭취량 계산(나이, 성별, 키, 몸무게, 활동수준 고려)
    if sex=='남': #남자일 경우
        energy=662-9.53*age+PA*(15.91*weight+539.6*height/100) #남성 에너지 계산식
        iron=(0.014*weight)/0.12*1.3 #남성 철 계산식
        if 19<=age<=64: 
            thiamin=1*1.2 #19세 이상 64세 이하 남성 티아민 계산식
            riboflavin=1.3*1.2 #19세 이상 64세 이하 남성 리보플라빈 계산식
            niacin=12*1.3 #19세 이상 64세 이하 남성 니아신 계산식
        elif age>=65: 
            thiamin=1*weight/68.9*1.2 #65세 이상 남성 티아민 계산식 
            riboflavin=1.3*(weight/68.9) #65세 이상 남성 리보플라빈 계산식
            niacin=11*1.3 #65세 이상 남성 니아신 계산식
            if age>=75: 
                niacin=10*1.3 #75세 이상 남성 니아신 계산식
    elif sex=='여': #여자일 경우
        energy=354-6.91*age+PA*(9.36*weight+726*height/100) #여성 에너지 계산식
        iron=(0.014*weight+0.05)/0.12*1.3 #여성 철 계산식
        if 19<=age<=64: 
            thiamin=0.9*1.2 #19세 이상 64세 이하 여성 티아민 계산식
            riboflavin=1*1.2 #19세 이상 64세 이하 여성 리보플라빈 계산식
            niacin=11*1.3 #19세 이상 64세 이하 여성 니아신 계산식
        elif age>=65:
            thiamin=0.9*(weight/55.9)*1.2 #65세 이상 여성 티아민 계산식
            riboflavin=1*(weight/55.9)*1.2 #65세 이상 여성 리보플라빈 계산식
            niacin=10*1.3 ##65세 이상 여성 니아신 계산식
            if age>=75:
                niacin=9*1.3 #75세 이상 여성 니아신 계산식
    protein=0.66/0.9*weight*1.25 #프로틴 계산식 (남녀 동일)
    calcium=9.39*weight*1.2 #칼슘 계산식 (남녀 동일)
    phosphorus=580*1.2 #인 계산식 (남녀 동일)
    vitaC=75*1.3 #비타민C 계산식 (남녀 동일)
    folate=320*1.2 #엽산 계산식 (남녀 동일)
    recommend=pd.DataFrame({'권장섭취량':[energy,protein,calcium,phosphorus,iron,niacin,folate,vitaC,thiamin,riboflavin]},index=['energy','protein','calcium','phosphorus','iron','niacin','folate','vitaC','thiamin','riboflavin']) #계산된 개인별 권장섭취량 값들로 데이터프레임 생성  
    return recommend #권장섭취량 반환
```
4. 유클리디안 거리 계산 함수
```python
def dist(x,y):   #유클리디안 거리를 계산하는 함수 식 
    return np.sqrt(np.sum((x-y)**2)) #유클리디안 거리 반환
```
5. 맞춤 영양제 추천(필요섭취량 추출 및 유클리디안 거리를 통한 유사도 분석)과 추천한 맞춤 영양제 섭취시 영양 변화 시각화
필요섭취량 추출
```python
def yournutrientinfo(age,sex,height,weight,PA): 
    info=pd.concat([recommend_intake(age,sex,height,weight,PA),day_7_nutrient(food_data)],axis=1) #concat 명령어로 위에서 추출한 사용자의 권장섭취량과 일상섭취량 한 데이터프레임으로 합치기 
    info['필요섭취량']=info['권장섭취량']-info['7일영양소평균'] #데이터프레임 연산으로 필요섭취량 계산 
    info.loc[info['필요섭취량']<0,'필요섭취량']=0 #음수인 필요섭취량은 고려하지 않기 위해 0으로 치환(음수인 필요섭취량은 과잉된 영양소로 더이상 보충이 필요하지 않아 0으로 치환하여 고려하지 않음)
    need=pd.DataFrame(info['필요섭취량']) #필요섭취량만으로 구성된 need 데이터프레임 생성
    need=need.T
    
    user_nutri=need.iloc[0].values #user_nutri라는 사용자의 필요섭취량 값만 추출
```
영양제 데이터 불러오기
```python
file = 'nutri_clean.xlsx'
    df = pd.read_excel(file)
    df = pd.read_excel(file, sheet_name = 0, index_col = 1)
    df = pd.read_excel(file, sheet_name = 0, header = 0) #영양제 데이터 불러오기
```
필요섭취량과 모든 영양제의 유클리디안 거리를 계산한 유사도 분석 후 가장 최적의 영양제 추출 및 출력
```python
    for i in range(len(df)):
        df.iloc[i,1:].values #영양제 데이터 모든 인덱스의 값들 하나씩 호출

    x = df.iloc[i,1:].values #유클리디안 거리를 이용한 유사도 분석에 활용될 영양제 데이터 인덱스의 값들 하나씩 호출하여 x로 지정
    y = np.array(user_nutri) #유클리디안 거리를 이용한 유사도 분석에 활용할 사용자의 필요섭취량 y로 지정

    df_recommend=pd.DataFrame()
    df_recommend['영양제명']=df['영양제명'] #영양제명으로 이루어진 데이터프레임 생성
    user_recommend=[] #사용자 필요섭취량과 모든 영양제의 유클리디안 거리를 계산한 값으로 리스트 생성
    for i in range(len(df)):
        x = df.iloc[i,1:].values
        user_recommend.append(dist(x,y)) #반복문과 append 명령어를 통해 모든 영양제의 유클리디안 거리를 리스트에 추가
    df_recommend2=pd.DataFrame(user_recommend) #user_recommend의 리스트 형태를 df_recommend2라는 데이터프레임으로 변환
    df_recommend2.columns=['dist'] 
    df_recommend3=pd.concat([df_recommend,df_recommend2],axis=1) #영양제명과 영양제마다의 유클리디안 거리로 이루어진 데이터프레임 생성

    nutri=df_recommend3.sort_values(by='dist' ,ascending=True) #유클리디안 거리가 짧은 순서대로 데이터프레임 재생성
    print(nutri.iloc[0,0]) #거리가 가장 짧은(유사도가 제일 큰) 영양제명 출력
```
시각화에 필요한 사용자의 권장섭취량, 일상섭취량, 추천한 영양제 섭취시 영양변화의 값들로만 이루어진 데이터프레임을 생성 후 정규화
```python
    select_nutri = df.iloc[[nutri.index.values[0]],1:] #추천하는 영양제의 영양성분 가져오기
    select_nutri = select_nutri.transpose()
    select_nutri.columns=['영양제 영양성분']
    visual_data=pd.concat([info,select_nutri],axis=1) #위에서 생성한 info 데이터프레임에 영양제 영양성분 칼럼 추가
    visual_data['영양제 섭취시 영양상태']=visual_data['영양제 영양성분']+visual_data['7일영양소평균'] #데이터 프레임 연산으로 데이터프레임에 영양제 섭취시 영양상태 컬럼 추가
    
    scaler = MinMaxScaler()
    visual_data[:] = scaler.fit_transform(visual_data[:]) #범위 차이를 왜곡하지 않고 공통 척도로 변경하기 위해 사용자의 권장섭취량, 일상섭취량, 추천한 영양제 섭취시 영양변화의 값들 정규화
    
    df_visual = pd.DataFrame({
        'Character': ['recommend','day_nutrient','take_nutrient'],
        'protein' : [visual_data.values[1,0], visual_data.values[1,1],visual_data.values[1,4]],
        'calcium': [visual_data.values[2,0], visual_data.values[2,1],visual_data.values[2,4]],
        'phosphorus': [visual_data.values[3,0], visual_data.values[3,1],visual_data.values[3,4]],
        'iron': [visual_data.values[4,0], visual_data.values[4,1],visual_data.values[4,4]],
        'niacin': [visual_data.values[5,0], visual_data.values[5,1],visual_data.values[5,4]],
        'folate': [visual_data.values[6,0], visual_data.values[6,1],visual_data.values[6,4]],
        'vitaC' : [visual_data.values[7,0], visual_data.values[7,1],visual_data.values[7,4]],
        'thiamin' : [visual_data.values[8,0], visual_data.values[8,1],visual_data.values[8,4]],
        'riboflavin' : [visual_data.values[9,0], visual_data.values[9,1],visual_data.values[9,4]] #정규화가 완료된 데이터에서 권장섭취량, 일상섭취량, 추천한 영양제 섭취시 영양변화 컬럼의 각 영양소의 값들 인덱싱하여 데이터프레임 생성 
    })
```
추천한 맞춤 영양제 섭취시 영양 변화 레이더 차트로 시각화
```python
labels = df_visual.columns[1:]
    num_labels = len(labels) #각도의 축 눈금의 라벨이 되는 영양소명들 num_labels에 입력
    
    angles = [x/float(num_labels)*(2*pi) for x in range(num_labels)] ## 각도의 값이 되는 리스트 입력
    angles += angles[:1] ## 그래프가 시작점으로 다시 돌아올 수 있도록 시작점(각도) 추가
    
    my_palette = plt.cm.get_cmap("Set2", len(df_visual.index))
    
    fig = plt.figure(figsize=(8,8))
    fig.set_facecolor('white')
    ax = fig.add_subplot(polar=True)
    for i, row in df_visual.iterrows():
        color = my_palette(i)
        data = df_visual.iloc[i].drop('Character').tolist()
        data += data[:1] #데이터는 각도 값이 되는 리스트와 길이가 같아야하므로 시작 데이터 값 추가
        
        ax.set_theta_offset(pi / 2) ## 그래프를 그리는 시작점 지정
        ax.set_theta_direction(-1) ## 그래프가 그려지는 방향을 시계방향으로 지정
        
        plt.xticks(angles[:-1], labels, fontsize=13) ## 각도 축 눈금 라벨
        ax.tick_params(axis='x', which='major', pad=15) ## 각 축과 눈금 사이에 여백 지정
        
        ax.set_rlabel_position(0) ## 반지름 축 눈금 라벨 각도 설정
        plt.yticks([0,0.1,0.2,0.3,0.4,0.5,0.6,0.7],['0','0.1','0.2','0.3','0.4','0.5','0.6','0.7'], fontsize=10) ## 반지름 축 눈금 설정
        plt.ylim(0,0.7)
        
        ax.plot(angles, data, color=color, linewidth=2, linestyle='solid', label=row.Character) ## 레이더 차트 출력
        ax.fill(angles, data, color=color, alpha=0.4) ## 도형 안쪽에 색 채우기
    
    plt.legend(loc=(1, 0.9))
    return plt.show() #그래프 반환
```
