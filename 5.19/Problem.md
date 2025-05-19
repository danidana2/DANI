## 문제 설명

다음은 아이스크림 가게의 상반기 주문 정보를 담은 `FIRST_HALF` 테이블과 7월의 아이스크림 주문 정보를 담은 `JULY` 테이블입니다.

### FIRST_HALF 테이블 구조

| NAME         | TYPE      | NULLABLE |
|--------------|-----------|----------|
| SHIPMENT_ID  | INT(N)    | FALSE    |
| FLAVOR       | VARCHAR(N)| FALSE    |
| TOTAL_ORDER  | INT(N)    | FALSE    |

- **SHIPMENT_ID**: 아이스크림 공장에서 아이스크림 가게까지의 출하 번호  
- **FLAVOR**: 아이스크림 맛 (기본 키)  
- **TOTAL_ORDER**: 상반기 아이스크림 총 주문량  
- `FIRST_HALF.SHIPMENT_ID`는 `JULY.SHIPMENT_ID`의 외래 키입니다.

---

### JULY 테이블 구조

| NAME         | TYPE      | NULLABLE |
|--------------|-----------|----------|
| SHIPMENT_ID  | INT(N)    | FALSE    |
| FLAVOR       | VARCHAR(N)| FALSE    |
| TOTAL_ORDER  | INT(N)    | FALSE    |

- **SHIPMENT_ID**: 아이스크림 공장에서 아이스크림 가게까지의 출하 번호 (기본 키)  
- **FLAVOR**: 아이스크림 맛 (`FIRST_HALF.FLAVOR`의 외래 키)  
- **TOTAL_ORDER**: 7월 아이스크림 총 주문량  

> 7월에는 주문량이 많아 같은 맛이라도 서로 다른 두 공장에서 출하할 수 있으므로, 동일한 맛이더라도 서로 다른 `SHIPMENT_ID`를 가질 수 있습니다.

---

## 문제

7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 **상위 3개의 맛**을 조회하는 SQL 문을 작성하세요.

---

## 예시 데이터

### FIRST_HALF

| SHIPMENT_ID | FLAVOR           | TOTAL_ORDER |
|-------------|------------------|-------------|
| 101         | chocolate        | 3,200       |
| 102         | vanilla          | 2,800       |
| 103         | mint_chocolate   | 1,700       |
| 104         | caramel          | 2,600       |
| 105         | white_chocolate  | 3,100       |
| 106         | peach            | 2,450       |
| 107         | watermelon       | 2,150       |
| 108         | mango            | 2,900       |
| 109         | strawberry       | 3,100       |
| 110         | melon            | 3,150       |
| 111         | orange           | 2,900       |
| 112         | pineapple        | 2,900       |

### JULY

| SHIPMENT_ID | FLAVOR         | TOTAL_ORDER |
|-------------|----------------|-------------|
| 101         | chocolate      | 520         |
| 102         | vanilla        | 560         |
| 103         | mint_chocolate | 400         |
| 104         | caramel        | 460         |
| 105         | white_chocolate| 350         |
| 106         | peach          | 500         |
| 107         | watermelon     | 780         |
| 108         | mango          | 790         |
| 109         | strawberry     | 520         |
| 110         | melon          | 400         |
| 111         | orange         | 250         |
| 112         | pineapple      | 200         |
| 208         | mango          | 110         |
| 209         | strawberry     | 220         |

---

## 예시 결과

각 맛별로 상반기 주문량과 7월 주문량(동일 맛에 여러 출하분이 있을 경우 합산)을 더한 값이 큰 순서대로 상위 3개 맛:

1. **strawberry**: 3,100 + (520 + 220) = 3,840  
2. **mango**:     2,900 + (790 + 110) = 3,800  
3. **chocolate**: 3,200 + 520       = 3,720  

```sql
-- 결과
FLAVOR
---------
strawberry
mango
chocolate
