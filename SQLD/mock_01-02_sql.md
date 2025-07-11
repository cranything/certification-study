# SQLD 기출변형 모의고사 정리 1-2

SQLD 자격증 대비용 모의고사 개념 요약입니다.

<br>

## 트랜잭션의 4대 특성 (ACID)

- **Atomicity (원자성)**: 트랜잭션은 전부 수행되거나 전혀 수행되지 않아야 한다.
- **Consistency (일관성)**: 트랜잭션 수행 전후에 데이터의 무결성이 유지되어야 한다.
- **Isolation (격리성)**: 동시에 수행되는 트랜잭션은 서로 영향을 주지 않아야 한다.
- **Durability (영속성)**: 트랜잭션이 성공적으로 완료되면 그 결과는 영구적으로 반영되어야 한다.

<br>

## 산술 연산자(+) vs 집계 함수(SUM)

| 구분         | 설명 |
|--------------|------|
| `+` 연산자   | 피연산자 중 하나라도 `NULL`이면 결과도 `NULL` |
| `SUM()` 함수 | `NULL`을 무시하고 나머지 값만 합산 |

<br>

## `NOT EXISTS` 서브쿼리

- 두 테이블 간 공통되는 값을 제외한 결과를 출력
- 조건 비교 중 `NULL`이 포함되면 `FALSE`로 간주되어 출력되지 않음

<br>

## `ESCAPE` 구문

- `LIKE` 절에서 `%`, `_` 등의 **와일드카드 문자를 문자 자체로 검색**할 때 사용

```sql
WHERE name LIKE '%30!%%' ESCAPE '!';
```

→ `"30%"` 라는 문자열을 검색

<br>

## `CUBE` vs `GROUPING SETS`

### 구문 비교

| 구문                            | 설명 |
|---------------------------------|------|
| `CUBE(A, B)`                    | A, B의 **모든 가능한 부분 집합**에 대해 집계 수행 |
| `GROUPING SETS ((A), (A, B))`   | **명시된 조합만** 집계 수행, 그 외 조합은 포함되지 않음 |

<br>

## 계층형 쿼리 내장 함수 (Oracle 기준)

- `LEVEL`: 현재 행의 계층 깊이 (루트 = 1)
- `CONNECT_BY_ISLEAF`: 말단 노드 여부 (1 = 말단 노드)
- `CONNECT_BY_ROOT(col)`: 루트 조상의 컬럼 값을 반환
- `SYS_CONNECT_BY_PATH(col, sep)`: 루트부터 현재 행까지의 경로를 문자열로 반환
- `CONNECT_BY_ISCYCLE`: 순환 여부를 반환 (1 = 순환 포함됨)
- `NOCYCLE`: 순환 구조를 방지하며 탐색 수행

<br>

## SQL 명령어 분류

| 분류 | 명령어 |
|------|--------|
| **DDL** (데이터 정의어) | `CREATE`, `ALTER`, `RENAME`, `DROP`, `TRUNCATE` |
| **DML** (데이터 조작어) | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** (데이터 제어어) | `GRANT`, `REVOKE` |
| **TCL** (트랜잭션 제어어) | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |
