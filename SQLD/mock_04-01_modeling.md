# SQLD 기출변형 모의고사 정리 4-1

SQLD 자격증 시험 대비용 모의고사 개념 정리입니다. 본 문서는 **도메인 개념**, **관계 표현 방식**, **GROUP BY 확장 함수** 그리고 **윈도우 함수 구문 오류 사례 분석**을 다루고 있습니다.  

<br>

## 도메인(Domain)

- **정의**: 속성이 가질 수 있는 값의 **범위(Range)**  
- **역할**: 엔터티 내의 속성(컬럼)에 대해 데이터의 타입, 크기(Size), 제약조건 등을 정의  
- **예시**:  
  - `성별(GENDER)` → `M`, `F`만 허용  
  - `전화번호(PHONE)` → `VARCHAR(13)` 형식 제약

```sql
-- 도메인을 정의할 수 있는 예시 (Oracle에는 직접 도메인 생성은 없음)
CREATE TABLE USER (
  GENDER CHAR(1) CHECK (GENDER IN ('M', 'F')),
  PHONE VARCHAR2(13)
);
```

<br>

## 식별 관계 vs 비식별 관계

- **식별 관계 (Identifying Relationship)**:  
  - 실선으로 표현  
  - 자식 엔터티의 기본키가 부모 엔터티의 기본키를 포함 (식별자 상속)  
  - 예: 주문 상세는 주문 테이블의 PK를 반드시 포함  
- **비식별 관계 (Non-identifying Relationship)**:  
  - 점선으로 표현  
  - 자식 엔터티의 기본키에 부모의 기본키가 포함되지 않음  
  - 예: 사원 - 부서 관계는 단순 외래키만 포함

<br>

## GROUP BY 확장 구문 정리

| 구문                  | 동작 방식 요약 |
|-----------------------|----------------|
| `ROLLUP(A, B)`        | A, B로 그룹핑 → A로 그룹핑 → 총합계 |
| `GROUPING SETS(A, B)` | A로 그룹핑, B로 그룹핑 (독립적으로 처리) |
| `CUBE(A)`             | A로 그룹핑 → 총합계 |
| `CUBE(A, B)`          | A, B로 그룹핑 → A만 그룹핑 → B만 그룹핑 → 총합계 |

<br>

### 🔍 예시 비교

```sql
SELECT dept, job, SUM(sal)
FROM emp
GROUP BY ROLLUP(dept, job);
```

> DEPT+JOB → DEPT → 전체 합계  
> → 계층적으로 요약 데이터를 생성할 수 있음

<br>

```sql
SELECT dept, job, SUM(sal)
FROM emp
GROUP BY GROUPING SETS((dept), (job));
```

> DEPT만 그룹핑한 결과 + JOB만 그룹핑한 결과 (서로 독립)

<br>

## 윈도우 함수 문법 오류 사례 분석

```sql
-- 잘못된 예시
AVG(SAL) OVER (
  PARTITION BY DEPT
  ORDER BY EMPNO
  RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED PRECENDING
) AS SAL1
```

### ❌ 오류 포인트

| 구문 오류 항목 | 설명 |
|----------------|------|
| `PRECENDING` 오타 | `PRECEDING`이 맞는 표현입니다. |
| `BETWEEN` 범위 논리 오류 | 시작과 끝이 모두 `UNBOUNDED PRECEDING`이면 범위가 성립하지 않음 |
| `AS SAL1` 위치 | 함수 바깥에 있어야 함 (`)` 뒤에 `AS SAL1`) |

### ✅ 올바른 수정 예

```sql
SELECT AVG(SAL) OVER (
  PARTITION BY DEPT
  ORDER BY EMPNO
  RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
) AS SAL1
```

> 🔹 `PARTITION BY`: 부서별로 윈도우 설정  
> 🔹 `ORDER BY`: 사번 순으로 정렬  
> 🔹 `RANGE`: 현재 행까지의 평균을 계산 (누적 평균)
