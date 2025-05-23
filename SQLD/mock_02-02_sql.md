# SQLD 기출변형 모의고사 정리 2-2

SQLD 자격증 대비용 모의고사 개념 요약입니다.

<br>

## COUNT와 GROUP BY

- `COUNT(*)`는 **행(row)의 수**를 반환함.
- `GROUP BY`가 함께 사용될 경우, **그룹의 수**가 반환됨.
- 예시:

  ```sql
  SELECT CATEGORY, COUNT(*) 
  FROM PRODUCT 
  GROUP BY CATEGORY;
  ```

  → `PRODUCT`의 전체 행이 아닌, `CATEGORY`별 묶음의 수를 셈

- `COUNT(CATEGORY)`는 NULL 값을 제외하고 계산함
- `COUNT(*)`는 NULL 포함 모든 행을 계산함

<br>

## 권한 부여 (GRANT 문)

- 권한 부여 문법:

  ```sql
  GRANT 권한명 ON 테이블명 TO 사용자명;
  ```

- 예시:

  ```sql
  GRANT SELECT ON EMP TO HR;
  ```

  → HR 사용자에게 EMP 테이블의 SELECT 권한 부여

### 주요 권한 종류

- `SELECT`: 조회
- `INSERT`: 삽입
- `UPDATE`: 수정
- `DELETE`: 삭제

<br>

## NULL 처리 함수: NVL

- `NVL(컬럼명, 대체값)`  
  → 컬럼 값이 NULL일 경우 대체값으로 변환

- 예시:

  ```sql
  SELECT NVL(SALARY, 0) FROM EMPLOYEES;
  ```

- Oracle 전용 함수이며, MySQL에서는 `IFNULL`, `COALESCE`를 사용

<br>

## OUTER JOIN

- OUTER JOIN은 **조건이 불일치해도** 한쪽 테이블의 행을 포함함
- `LEFT OUTER JOIN`은 **왼쪽 테이블의 모든 행**을 포함함

- 예시:

  ```sql
  SELECT * 
  FROM A 
  LEFT OUTER JOIN B 
  ON A.ID = B.ID;
  ```

- 기준 테이블은 JOIN 키워드 앞쪽(A 테이블)

<br>

## CROSS JOIN

- `A CROSS JOIN B`  
  → A의 각 행과 B의 각 행을 **모든 조합**으로 연결 (카티션 곱)

- 예시:

  ```sql
  SELECT * 
  FROM A 
  CROSS JOIN B;
  ```

- 예: A 테이블에 3행, B 테이블에 4행이 있다면 → 결과는 3 × 4 = 12행

- 실무에서는 조건 없이 JOIN을 작성하여 **실수로 CROSS JOIN이 발생**하는 경우가 있으므로 주의

<br>

## TIP 요약

- `JOIN` 문제는 **출력 행의 수**, **기준 테이블 방향**을 명확히 이해해야 함
- `GRANT`는 **권한 부여 구문**이며, 권한명과 테이블명을 정확히 명시해야 함
- `NVL`, `COUNT`, `GROUP BY`는 **함께 출제되는 빈도가 높으므로 패턴으로 학습**할 것