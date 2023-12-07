## SELECT문의 기본형식
4-3. SQL의 기본 뼈대, SELECT절과 FROM절
```sql
기본형식
SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
FROM [조회할 테이블 이름];
```

- *로 테이블 전체 열 출력하기  
```sql
EMP 테이블 전체 열 조회하기
SELECT * FROM EMP;
```
  
- 테이블 부분 열 출력하기
```sql
EMP 테이블에서 사원번호, 이름, 부서번호 조회하기
SELECT EMPNO, ENAME, DEPTNO
FROM EMP;
```

4-4. 중복 데이터를 삭제하는 DISTINCT<br>
DISTINCE는 조회한 데이터의 내용에서 불필요한 중복을 제거하고 특정 데이터 종류만 확인하고 싶을 때 유용합니다.
- 열이 한 개일 때 열 중복 제거하기
```sql
SELECT DISTINCT DEPTNO
FROM EMP; 
```
- 열이 여러 개인 경우 열 중복 제거하기
```sql
SELECT DISTINCT JOB, DEPTNO
FROM EMP;
```
- ALL로 중복되는 열 제거 없이 그대로 출력하기
```sql
SELECT ALL JOB, DEPTNO
FROM EMP;
```

4-5. 한눈에 보기 좋게 별칭 설정하기<br>
긴 열 이름을 짧고 간단한 다른 이름으로 알기 쉽게 출력할 때 별칭을 사용합니다.<br>
즉, 열 이름 대신 붙이는 이름을 별칭(alias)이라고 합니다.

- 별칭을 지정하는 방식

|사용 방법|설명| 
|:-------|:--| 
| SAL*12+COMM ANNSAL | 연산 및 가공된 문장 이후 한 칸 띄우고 별칭 지정 |
| SAL*12+COMM "ANNSAL" | 연산 및 가공된 문장 이후 한 칸 띄우고 별칭을 큰따옴표(" ")로 묶어 지정 |
| SAL*12+COMM AS ANNSAL | 연산 및 가공된 문장 이후 한 칸 띄운 후 'AS', 한 칸 뒤에 별칭 지정 |
| SAL*12+COMM AS "ANNSAL" | 연산 및 가공된 문장 이후 한 칸 띄운후 'AS', 한 칸 뒤에 별칭을 큰따옴표로(" ")로 묶어 지정 |

- 별칭을 사용하여 사원의 연간 총 수입 출력하기
```sql
-- SAL * 12 + COMM 열이름을 ANNSAL로 별칭 설정
SELECT ENAME, SAL, SAL * 12 + COMM AS ANNSAL, COMM
FROM EMP;
```

별칭을 사용하는 이유는 단순히 길 열 이름의 불편함 외에도 현재 데이터가 나오기까지의 진행과정을 숨기는 용도로도 사용합니다.<br>
(보안이나 데이터 노출 문제 방지)

- 실무에서 별칭 지정<br>
실무에서는 별칭을 지정하는 4가지 방식 중<br>
세 번째 방식 (연산 및 가공된 문장 이후 한 칸 띄운 후 'AS', 한 칸 뒤에 별칭 지정)을 선호하는 경향이 있습니다.<br>
AS가 붙는 형식을 선호하는 이유는 조회해야 할 열이 수십, 수백 개일 경우에 어떤 단어가 별칭인지 알아보기 편하기 때문이고<br>
큰 따옴표를 생략한 형식을 선호하는 이유는 큰 따옴표를 사용했을때 SQL문에 사용한 건지 기존 프로그래밍 코드에서 문법으로 사용한 건지 구별하는 추가 작업이 필요하기 때문입니다.<br>
```java
String sql = "SELECT ENAME, SAL, SAL * 12 + COMM AS ANNSAL, COMM FROM EMP";

String sql = "SELECT ENAME, SAL, SAL * 12 + COMM AS "ANNSAL", COMM FROM EMP"; // ""로 인한 오류나 예외상황 발생가능성이 있음
```

4-6. 원하는 순서로 출력 데이터를 정렬하는 ORDER BY<br>
SELECT문을 사용하여 데이터를 조회할 때 시간이나 이름 순서 또는 어떤 다른 기준으로 데이터를 정렬해서 출력해야 하는 경우 ORDER BY를 사용합니다. ORDER BY절은 SELECT문을 작성할 때 사용할수 있는 여러 절 중 가장 마지막 부분에 적습니다.

- 오름차순 사용하기
```sql
-- EMP 테이블의 모든 열을 급여 기준으로 오름차순 정렬하기
SELECT *
FROM EMP
ORDER BY SAL;
```
- 내림차순 사용하기
```sql
-- EMP 테이블의 모든 열을 급여 기준으로 내림차순 정렬하기
SELECT *
FROM EMP
ORDER BY SAL DESC;
```
- 각각의 열에 내림차순과 올림차순 동시에 사용하기
ORDER BY절에는 우선순위를 고려하여 여러 개의 정렬 기준을 지정할 수 있습니다.

```sql
-- EMP 테이블의 전체 열을 부서 번호(오름차순)와 급여(내림차순)로 정렬하기
SELECT *
FROM EMP
ORDER BY DEPTNO ASC, SAL DESC;
```

- ORDER BY절을 사용할 때 주의 사항<br>
ORDER BY절이 존재할 경우 SELECT문을 통해 조회할 데이터를 모두 확정한 상태에서 ORDER BY절의 명시된 기준에 따라 정렬하는데<br>
데이터의 양 또는 정렬 방식에 따라 출력 데이터를 선정하는 시간보다 정렬하는 데 시간이 더 걸리게 됩니다.<br>
즉, SQL문의 효율이 낮아지므로 ORDER BY절을 사용한 정렬은 꼭 필요한 경우가 아니면 사용하지 않는 것이 좋습니다.<br>

