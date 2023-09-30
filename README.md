# M5 Forecasting - Uncertainty
- 플랫폼: Kaggle
- 대회명: M5 Forecasting - Uncertainty
- 주최: University of Nicosia
- 대회 페이지 [링크](https://www.kaggle.com/c/m5-forecasting-uncertainty)

## Objective
1. 판매량 예측은 적절한 재고량 유지, 주문량 결정 및 비용/기회 비용의 손실을 막기 위해 다양한 사업에서 이루어지는 중요한 주제 중 하나
2. 대부분의 예측 모델링은 하나의 예측값이 정확하도록 이루어지지만, 범위를 예측하면 예측이 불확실한 정도를 확인할 수 있음  

위 두 가지 목표를 위하여 월마트의 계층적/위계적(Hierarchical) 판매량 데이터를 이용하여 특정 기간 동안 다양한 집계 레벨에서의 판매량 범위 예측

## Data

### calender.csv
|Column|Description|Example|
|------|-----------|-------|
|date|날짜(포맷: %Y-%m-%d)|2011-01-29|
|wm_yr_wk|주 ID|11101|
|weekday|요일|Saturday|
|wday|일 ID(토요일:1 ~ 금요일:7)|1|
|month|월|12|
|year|연도|2011|
|d|2011-01-29일 이후 지난 일수|d_20|
|event_name_\[i\]|이벤트 이름|SuperBowl|
|event_type_\[i\]|이벤트 타입|Sporting|
|snap_\[State\]|SNAP* 구매 가능 여부(Binary)|0 또는 1|

\* SNAP(Supplement Nutrition Assistance Program)은 저소득 가정에게 이루어지는 식품 지원 프로그램으로 직불카드 형태의 EBT(Electronic Benefit Transfer) 카드로 지원금이 지급된다.

### sales_train_\[validation/evaluation\].csv
|Column|Description|Example|
|------|-----------|-------|
|id|\[item_id\]\_\[store_id\]_\[validation/evaluation\]|HOBBIES_1_001_CA_1_evaluation|
|item_id|상품 ID|HOBBIES_1_001|
|dept_id|담당 부서 ID|HOBBIES_1|
|cat_id|카테고리 ID|HOBBIES|
|store_id|점포 ID|CA_1|
|state_id|주 ID|CA|
|d_\[# of day\]*|해당 날짜 판매량|3|

\* validation의 경우 d_1 ~ d_1913, evaluation의 경우 d_1 ~ d_1940까지의 칼럼이 존재 

### sell_prices.csv
|Column|Description|Example|
|------|-----------|-------|
|store_id|점포 ID|CA_1|
|item_id|상품 ID|HOBBIES_1_001|
|wm_yr_wk|주 ID|11101|
|sell_price|판매 가격|9.58|

### submission.csv
|Column|Description|Example|
|------|-----------|-------|
|id|\[ID Prefix by aggregation level\]\_\[Prediction quartile\]\_\[validation/evaluation\]*|HOBBIES_1_001_CA_1_0.005_validation|
|F\[Forecast Day\]|ID에 해당하는 Forecast Day 시점 예측값||

\* 아래 내용 참조

#### Aggregation Level
|Level|ID Prefix|Number of series|
|-----|---------|----------------|
|전체 판매량 합계|Total_X_|1|
|주별 판매량 합계|\[STATE\]\_X\_|3|
|점포별 판매량 합계|\[STORE\]\_X\_|10|
|카테고리별 판매량 합계|\[CAT\]\_X\_|3|
|담당 부서별 판매량 합계|\[DEPT\]\_X\_|7|
|주 & 카테고리별 판매량 합계|\[STATE\]_\[CAT\]\_|9|
|주 & 담당 부서별 판매량 합계|\[STATE\]_\[DEPT\]\_|21|
|점포 & 카테고리별 판매량 합계|\[STORE\]_\[CAT\]\_|30|
|점포 & 담당 부서별 판매량 합계|\[STATE\]_\[DEPT\]\_|70|
|상품별 판매량 합계|\[ITEM\]\_X\_|3,049|
|주 & 상품별 판매량 합계|\[STATE\]_\[ITEM\]\_|9,147|
|점포 & 상품별 판매량 합계|\[ITEM\]_\[STORE\]\_|30,490|

### Prediction Quantiles
- 50%
- 33%, 67%
- 5%, 95%
- 1%, 99%

### Forecast days
- 본 대회에서는 각 데이터가 끝난 시점 이후 28일 동안의 판매량 예측을 진행함
    - Validation 데이터의 예측 기간: d_1914 ~ d_1941
    - Evaluation 데이터의 예측 기간: d_1941 ~ d_1969
- 따라서 `submission.csv` 파일에는 F1~F28까지의 칼럼이 존재

## Evaluations
- Weighted Scaled Pinball Loss(WSPL)을 이용하여 평가 진행
    - Pinball Loss 설명 블로그 [링크](https://tigris-data-science.tistory.com/entry/ML-Pinball-Loss)