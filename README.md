# 친환경 관련 기업주 & 유류 관련 기업주 & 유가 EDA - 날짜, 현재가, 종가 중심으로

- 친환경 관련 기업주와 유류 관련 기업주, 유가에 대해 탐색적 데이터 분석을 진행한다.
- 날짜를 기준으로 현재가, 종가의 상관관계를 분석하여 주식투자 시 의사결정과 종목 분석 등에 활용할 수 있다.

## 프로젝트 주제 선정 배경
코로나 19의 전 세계 확산으로 인해 주가들이 한 달 사이에 크게 폭락했고, 이에 따라 몇몇 대기업들과 중견 중소기업들이 크게 휘청거리거나 부도가 났다. 이러한 위기 상황에서 20~30대들은 주식투자를 하나의 기회로 보고 마치 모두 약속이라도 한 듯 투자계좌를 개설하여 삼성전자를 비롯해 국내외 다양한 기업들의 주식을 사기 시작했다. 많은 투자 종목 중 친환경 관련주와 유류 관련 주로 선정한 이유는 코로나 19로 인해 친환경에 많은 관심을 가지게 되었고, 이와 정반대인 종목과 유의미한 관계가 있는지 비교해보고 싶어 유류 관련주로 선정했다. 친환경 기업에 투자를 한다는 가정하에 투자 전 먼저 데이터를 탐색해 봄으로써 투자의사 결정에 유의미한 인사이트를 얻고자 하며, 통상적으로 알려진 상식으로 판단하여 투자해도 괜찮은지 살펴본다.

## Getting Started

You will require Python 3 and the following libraries

### Prerequisites

* numpy - Used for fast matrix operations.
* pandas - Used for performing Data Analysis.
* matplotlib - Used to create plots.
* preprocessing - Used for making a comparison easier to see

### Dataset

* 2015년 1월 ~ 2019년 12월 풍국주정, 한온시스템, 에코바이오, 세종공업, 유니슨, 동국S&C, 금호페트롤, 한화솔루션, 포스코케미칼, SK케미칼, 롯데케미칼, S-OIL, Dubai, WTI, Brent 데이터
    * 대신증권 HTS 프로그램 (.xlsx)

## EDA Project Progress

* 1. 데이터를 읽어온다 (친환경 기업, 유류 관련 기업, 유가)
* 2. 데이터 프레임을 만들고, 비교할 데이터만 불러온다
       * 기업 : 날짜, 현재가
       * 유가 : 날짜, 종가
* 3. 기업 데이터와 유가 데이터 가공 : merge 사용
        * 데이터 개수 통일
* 4. 친환경 기업, 유류 관련 기업, 유가 간의 상관관계 분석 : preprocessing.minmax_scale 사용 => 모든 데이터를 0~1 사이값으로 변경
        * z = (x - min(x)) / (max(x) - min(x))
        * np.corrcoef 사용 => 상관계수 도출
* 5. 상관계수가 높은 순으로 전체 데이터 정렬
* 6. 양의 상관관계, 음의 상관관계 선그래프로 시각화
* 7. 인사이트 도출


## Team Questions

* Q1) 친환경 관련주랑 친환경과 정반대인 유류 관련 기업주는 주가도 서로 정반대일까?
* Q2) 친환경 관련 기업주와 유가는 음의 상관관계일까?
* Q3) 그럼 유류 관련 기업주와 유가는 당연히 양의 상관관계?


## 상관계수 데이터 정렬 (ascending=False)
```
corr_result.sort_values(by=['상관계수'], axis=0, ascending=False)
corr_result["상관계수"] = round(corr_result["상관계수"],2)
corr_result.rename(columns={"상관계수" : "상관계수(%)"}, inplace=True)
corr_result.sort_values(by=['상관계수(%)'], axis=0, ascending=False)
```
<img width="243" alt="Screen Shot 2020-11-19 at 5 57 25 PM" src="https://user-images.githubusercontent.com/72849922/99644031-c5601e00-2a90-11eb-9cd5-f8f1d9dd9866.png">

### 양의 상관관계 Top5 시각화
```
corr_result.sort_values(by=['상관계수(%)'], axis=0, ascending=False).head(5)
```
<img width="234" alt="Screen Shot 2020-11-19 at 6 01 01 PM" src="https://user-images.githubusercontent.com/72849922/99644389-39022b00-2a91-11eb-81c5-764899a86e5c.png">

#### 1위 - 금호페트롤 vs Brent 비교 (상관계수 : 0.83)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["금호페트롤_현재가"]), label="geumhopetrol_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["Brent_종가"]), label="brent_minmax")
plt.legend(loc=0);
```
<img width="984" alt="Screen Shot 2020-11-19 at 6 03 37 PM" src="https://user-images.githubusercontent.com/72849922/99644675-94ccb400-2a91-11eb-8839-f077636049bf.png">

#### 2위 - 금호페트롤 vs Dubai 비교 (상관계수 : 0.82)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["금호페트롤_현재가"]), label="geumhopetrol_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["두바이_종가"]), label="dubai_minmax")
plt.legend(loc=0);
```
<img width="988" alt="Screen Shot 2020-11-19 at 6 04 58 PM" src="https://user-images.githubusercontent.com/72849922/99644833-c5ace900-2a91-11eb-95c0-7bff5f5101b7.png">

#### 3위 - 금호페트롤 vs WTI 비교 (상관계수 : 0.81)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["금호페트롤_현재가"]), label="geumhopetrol_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["WTI_종가"]), label="wti_minmax")
plt.legend(loc=0);
```
<img width="988" alt="Screen Shot 2020-11-19 at 6 06 16 PM" src="https://user-images.githubusercontent.com/72849922/99644969-f2f99700-2a91-11eb-8942-ef4f6be9ce18.png">

#### 4위 - 풍국주정 vs 포스코케미칼 비교 (상관계수 : 0.76)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["풍국주정_현재가"]), label="poonggook_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["포스코케미칼_현재가"]), label="poscochemical_minmax")
plt.legend(loc=0);
```
<img width="983" alt="Screen Shot 2020-11-19 at 6 07 22 PM" src="https://user-images.githubusercontent.com/72849922/99645085-1b819100-2a92-11eb-97f9-d7945b2bfa36.png">

#### 5위 - 포스코케미칼 vs Dubai 비교 (상관계수 : 0.76)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["포스코케미칼_현재가"]), label="poscochemical_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["두바이_종가"]), label="dubai_minmax")
plt.legend(loc=0);
```
<img width="995" alt="Screen Shot 2020-11-19 at 6 08 38 PM" src="https://user-images.githubusercontent.com/72849922/99645206-44a22180-2a92-11eb-8f52-538ea9351906.png">

#### 양의 상관관계 Top5 시각화 결과
* - 대부분 예측했던 대로 유류 관련 기업주와 유가는 높은 양의 상관관계를 보였다.
* - 친환경 기업인 풍국주정과 유류 관련 기업인 포스코케미칼이 높은 양의 상관관계를 가진다는 것은 특이한 점이다.

### 음의 상관관계 Top5 시각화

```
corr_result.sort_values(by=['상관계수(%)'], axis=0, ascending=False).tail(5)
```
<img width="241" alt="Screen Shot 2020-11-19 at 6 14 47 PM" src="https://user-images.githubusercontent.com/72849922/99645896-22f56a00-2a93-11eb-81fb-1d5c47378f45.png">

#### 1위 세종공업 vs 포스코케미칼 비교 (상관계수 : -0.78)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["세종공업_현재가"]), label="sejong_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["포스코케미칼_현재가"]), label="poscochemical_minmax")
plt.legend(loc=0);
```
<img width="983" alt="Screen Shot 2020-11-19 at 6 16 06 PM" src="https://user-images.githubusercontent.com/72849922/99646012-50421800-2a93-11eb-859f-bc626abbd0d9.png">

#### 2위 에코바이오 vs 포스코케미칼 비교 (상관계수 : -0.72)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["에코바이오_현재가"]), label="echobio_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["포스코케미칼_현재가"]), label="poscochemical_minmax")
plt.legend(loc=0);
```
<img width="984" alt="Screen Shot 2020-11-19 at 6 17 32 PM" src="https://user-images.githubusercontent.com/72849922/99646181-84b5d400-2a93-11eb-89ee-ffb4e925dc6a.png">

#### 3위 에코바이오 vs Brent 비교 (상관계수 : -0.67)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["에코바이오_현재가"]), label="echobio_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["Brent_종가"]), label="brent_minmax")
plt.legend(loc=0);
```
<img width="986" alt="Screen Shot 2020-11-19 at 6 18 48 PM" src="https://user-images.githubusercontent.com/72849922/99646326-b038be80-2a93-11eb-9767-db43ec328936.png">

#### 4위 에코바이오 vs Dubai 비교 (상관계수 : -0.65)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["에코바이오_현재가"]), label="echobio_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["두바이_종가"]), label="dubai_minmax")
plt.legend(loc=0);
```
<img width="982" alt="Screen Shot 2020-11-19 at 6 19 40 PM" src="https://user-images.githubusercontent.com/72849922/99646436-ce062380-2a93-11eb-807e-33cba6819deb.png">

#### 5위 세종공업 vs Soil 비교 (상관계수 : -0.61)
```
plt.figure(figsize=(20,5))

plt.plot(merge_df[::-1]["날짜"],preprocessing.minmax_scale(merge_df[::-1]["세종공업_현재가"]), label="sejong_minmax")

plt.plot(merge_df[::-1]["날짜"], preprocessing.minmax_scale(merge_df[::-1]["에스오일_현재가"]), label="soil_minmax")
plt.legend(loc=0);
```
<img width="983" alt="Screen Shot 2020-11-19 at 6 20 38 PM" src="https://user-images.githubusercontent.com/72849922/99646544-f3932d00-2a93-11eb-9f91-c40bc51007a2.png">

#### 음의 상관관계 Top5 시각화 결과
* - 대부분 예측했던 대로 친환경 관련 기업주와 유류 관련 기업, 유가는 높은 양의 상관관계를 보였다.

#### 질문에 답하기
<img width="1170" alt="Screen Shot 2020-11-19 at 6 30 41 PM" src="https://user-images.githubusercontent.com/72849922/99647762-9009ff00-2a95-11eb-9e8b-67fe17cb5819.png">
<img width="1242" alt="Screen Shot 2020-11-19 at 6 35 00 PM" src="https://user-images.githubusercontent.com/72849922/99648061-f3942c80-2a95-11eb-851b-4438fb26a298.png">
<img width="1164" alt="Screen Shot 2020-11-19 at 6 35 51 PM" src="https://user-images.githubusercontent.com/72849922/99648159-11619180-2a96-11eb-9d68-f55f7e91e4f4.png">
<img width="1163" alt="Screen Shot 2020-11-19 at 6 36 29 PM" src="https://user-images.githubusercontent.com/72849922/99648244-28a07f00-2a96-11eb-9684-7f43eea08409.png">

### 한계 및 시사점
<img width="1161" alt="Screen Shot 2020-11-19 at 6 37 44 PM" src="https://user-images.githubusercontent.com/72849922/99648622-98166e80-2a96-11eb-842b-58b71db5ee7d.png">


## Built With
- [서기현](https://github.com/jungryo): 데이터 가공, 상관관계 분석, PPT 작성, 발표, Readme 작성
- [이재석](https://github.com/Yenabeam): 데이터 제공, 상관관계 분석, 자료 조사
