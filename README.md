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
```
food_data=pd.read_csv("food_data_preprocessing_final.csv")
food_data=food_data.drop(columns=['Unnamed: 0']) #인덱스 번호가 이중 생성되지 않도록 데이터 내의 인덱스 번호 열 
food_data
```
2. ㅇㅇ
```
def day_7_nutrient(food_data):
    food=[x for x in input("공백을 구분자로 먹은 식단을 입력바랍니다: ").split()] #일상섭취량을 구하기 위해 식단 입력 (일상섭취량 : 하루동안 섭취하는 평균적인 섭취량)
    food_result=pd.DataFrame() #day_7_nutrient 함수를 통한 출력값으로 데이터프레임 생성
    for i in range(len(food_data)): 
        for j in range(len(food)):
            if food_data['식품명'][i]==food[j]:    
                food_result_=food_data.loc[i]
                food_result=pd.concat([food_result,food_result_],axis=1) #식품 데이터에 일치하는 이름의 행을 모아 새로운 데이터 프레임 생성

    food_result=food_result.T
    day7_mean=pd.DataFrame(food_result.iloc[:,1:].sum()/7)
    day7_mean.columns=['7일영양소평균'] #입력받은 식단 데이터를 종합한 후 7로 나눈 하루 평균 섭취량(일상섭취량) 추출
    return day7_mean
```
3. dd
```
def recommend_intake(age,sex,height,weight,PA):
    if sex=='남':
        energy=662-9.53*age+PA*(15.91*weight+539.6*height/100)
        iron=(0.014*weight)/0.12*1.3
        if 19<=age<=64:
            thiamin=1*1.2
            riboflavin=1.3*1.2
            niacin=12*1.3
        elif age>=65:
            thiamin=1*weight/68.9*1.2
            riboflavin=1.3*(weight/68.9)
            niacin=11*1.3
            if age>=75:
                niacin=10*1.3
    elif sex=='여':
        energy=354-6.91*age+PA*(9.36*weight+726*height/100)
        iron=(0.014*weight+0.05)/0.12*1.3
        if 19<=age<=64:
            thiamin=0.9*1.2
            riboflavin=1*1.2
            niacin=11*1.3
        elif age>=65:
            thiamin=0.9*(weight/55.9)*1.2
            riboflavin=1*(weight/55.9)*1.2
            niacin=10*1.3
            if age>=75:
                niacin=9*1.3
    protein=0.66/0.9*weight*1.25
    calcium=9.39*weight*1.2
    phosphorus=580*1.2
    vitaA=0.005*20*0.03*weight*1000*1.1*100/40*1.4 
    vitaC=75*1.3
    folate=320*1.2
    recommend=pd.DataFrame({'권장섭취량':[energy,protein,calcium,phosphorus,iron,niacin,folate,vitaC,thiamin,riboflavin]},index=['energy','protein','calcium','phosphorus','iron','niacin','folate','vitaC','thiamin','riboflavin'])
    return recommend
```
4. dd
```
def dist(x,y):   
    return np.sqrt(np.sum((x-y)**2))
```
5. dd
```
def yournutrientinfo(age,sex,height,weight,PA):
    info=pd.concat([recommend_intake(age,sex,height,weight,PA),day_7_nutrient(food_data)],axis=1)
    info['필요섭취량']=info['권장섭취량']-info['7일영양소평균']
    info.loc[info['필요섭취량']<0,'필요섭취량']=0
    need=pd.DataFrame(info['필요섭취량'])
    need=need.T
    
    user_nutri=need.iloc[0].values
    file = 'nutri_clean.xlsx'
    df = pd.read_excel(file)
    df = pd.read_excel(file, sheet_name = 0, index_col = 1)
    df = pd.read_excel(file, sheet_name = 0, header = 0)

    for i in range(len(df)):
        df.iloc[i,1:].values

    x = df.iloc[i,1:].values
    y = np.array(user_nutri)

    df_recommend=pd.DataFrame()
    df_recommend['영양제명']=df['영양제명']
    user_recommend=[]
    for i in range(len(df)):
        x = df.iloc[i,1:].values
        user_recommend.append(dist(x,y))
    df_recommend2=pd.DataFrame(user_recommend)
    df_recommend2.columns=['dist']
    df_recommend3=pd.concat([df_recommend,df_recommend2],axis=1)

    nutri=df_recommend3.sort_values(by='dist' ,ascending=True)
    print(nutri.iloc[0,0]) 
    
    select_nutri = df.iloc[[nutri.index.values[0]],1:]
    select_nutri = select_nutri.transpose()
    select_nutri.columns=['영양제 영양성분']
    visual_data=pd.concat([info,select_nutri],axis=1)
    visual_data['영양제 섭취시 영양상태']=visual_data['영양제 영양성분']+visual_data['7일영양소평균']
    
    scaler = MinMaxScaler()
    visual_data[:] = scaler.fit_transform(visual_data[:])
    
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
        'riboflavin' : [visual_data.values[9,0], visual_data.values[9,1],visual_data.values[9,4]]
    })
    
    labels = df_visual.columns[1:]
    num_labels = len(labels)
    
    angles = [x/float(num_labels)*(2*pi) for x in range(num_labels)] ## 각 등분점
    angles += angles[:1] ## 시작점으로 다시 돌아와야하므로 시작점 추가
    
    my_palette = plt.cm.get_cmap("Set2", len(df_visual.index))
    
    fig = plt.figure(figsize=(8,8))
    fig.set_facecolor('white')
    ax = fig.add_subplot(polar=True)
    for i, row in df_visual.iterrows():
        color = my_palette(i)
        data = df_visual.iloc[i].drop('Character').tolist()
        data += data[:1]
        
        ax.set_theta_offset(pi / 2) ## 시작점
        ax.set_theta_direction(-1) ## 그려지는 방향 시계방향
        
        plt.xticks(angles[:-1], labels, fontsize=13) ## 각도 축 눈금 라벨
        ax.tick_params(axis='x', which='major', pad=15) ## 각 축과 눈금 사이에 여백을 준다.
        
        ax.set_rlabel_position(0) ## 반지름 축 눈금 라벨 각도 설정(degree 단위)
        plt.yticks([0,0.1,0.2,0.3,0.4,0.5,0.6,0.7],['0','0.1','0.2','0.3','0.4','0.5','0.6','0.7'], fontsize=10) ## 반지름 축 눈금 설정
        plt.ylim(0,0.7)
        
        ax.plot(angles, data, color=color, linewidth=2, linestyle='solid', label=row.Character) ## 레이더 차트 출력
        ax.fill(angles, data, color=color, alpha=0.4) ## 도형 안쪽에 색을 채워준다.
    
    plt.legend(loc=(1, 0.9))
    return plt.show()
```
