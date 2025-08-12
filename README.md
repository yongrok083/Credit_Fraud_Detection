# Credit_Fraud_Detection

# 신용카드 사기거래 데이터 전처리 & 피처 만들기

## 📌 오늘 한 일
- fraud.csv 불러오기
- 불필요한 컬럼 삭제 (이름, 주소, 생년월일 등 개인정보)
- 컬럼 이름 전부 소문자로 바꾸고 띄어쓰기 → 언더바(_)로 변경
- 결측치 확인 (`isna().mean()`)
- 거래 금액, 거리, 날짜/시간 정보를 이용해 새로운 변수 만들기

---

## 🛠 사용한 함수 & 작업

# 결측치 비율 확인
fraud.isna().mean()

# 컬럼 이름 깔끔하게 변경
fraud.columns.str.strip().str.lower().str.replace(' ', '_')

# 컬럼 삭제
fraud.drop(columns=drop_cols, errors='ignore')

# 4분위수 구하기
fraud['amt'].quantile(0.75)

# 그룹별 평균
fraud.groupby('category')['amt'].mean()

# 피벗테이블
pd.pivot_table(fraud, index='category', values='amt', aggfunc='mean')

# 로그변환
np.log1p(fraud['amt'])

# 위도/경도 거리 계산 함수 (haversine)
def haversine(lat1, lon1, lat2, lon2):


# 날짜/시간에서 시(hour), 요일(weekday), 주말 여부(is_weekend) 추출
fraud['trans_date_trans_time'].dt.hour
fraud['trans_date_trans_time'].dt.weekday
fraud['trans_weekday'].isin([5, 6]).astype(int)

# 그룹별 변환 (거래 횟수, 평균)
fraud.groupby(['cc_num', 'trans_date'])['amt'].transform('count')
fraud.groupby(['cc_num', 'trans_date'])['amt'].transform('mean')

🆕 만든 주요 변수

trans_hour → 거래 시간

trans_weekday → 거래 요일

is_weekend → 주말 여부

amt_log → 거래금액 로그 변환

high_amount → 금액 상위 25% 여부

distance_km → 고객-상점 거리(km)

far_transaction → 먼 거리 거래 여부

daily_trans_count → 하루 거래 횟수

daily_avg_amt → 하루 평균 거래금액

amt_vs_cust_avg → 고객 평균 금액과 차이

weekend_far → 주말 & 먼 거래 여부
