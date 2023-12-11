## WHERE절과 연산자
### 5-1. 필요한 데이터만 쏙 출력하는 WHERE절
WHERE절은 SELECT문으로 데이터를 조회할 때 특정 조건을 기준으로 원하는 행을 출력하는 데 사용합니다.<br>
그리고 여러 연산자를 함께 사용하면 더욱 세밀하게 데이터 검색을 할 수 있습니다.

```sql
-- 기본형식
SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
FROM [조회할 테이블 이름]
WHERE [조회할 행을 선별하기 위한 조건식];
```

```sql
--- 부서번호가 30인 데이터만 출력하기
SELECT *
FROM EMP
WHERE DEPTNO = 30;
```

### 5-2. 여러개 조건식을 사용하는 AND, OR 연산자
WHERE절에서는 조건식을 여러 개 지정할 수 있습니다.<br>
이때 사용하는 것이 바로 논리 연산자 AND, OR입니다.
```sql
-- AND 연산자로 여러 개의 조건식 사용하기
SELECT *
FROM EMP
WHERE DEPTNO = 30
AND JOB = 'SALESMAN';
```

```sql
-- OR 연산자로 여러 개의 출력 조건 사용하기
SELECT *
FROM EMP
WHERE DEPTNO = 30
OR JOB = 'CLERK';
```
- 실무에서의 AND, OR 연산자<br>
실무에서 사용하는 SELECT문은 OR연산자보다 AND연산자를 많이 사용합니다.<br>
다양한 조건을 한 번에 만족시키는 데이터만을 추출해야 할 때가 많기 때문입니다.<br>
예를 들어 은행 계좌의 이체 내역 중 최근 1개월 이내의 출금 데이터만 보고자 할 경우,<br>
기간이 1개월 이내라는 조건과 출금 내역 데이터라는 조건을 모두 만족 시켜야 합니다.<br>

### 5-3. 연산자 종류와 활용방법
- 산술 연산자<br>
  수치 연산에 사용하는 산술 연산자는 더하기 +, 빼기 -, 곱하기 *, 나누기 /을 사용합니다.
  ```sql
  -- 급여에 12를 곱한 값이 36000인 사원 조회
  SELECT *
  FROM EMP
  WHERE SAL * 12 = 36000;
  ```
- 비교 연산자
  - 대소 비교 연산자<br>
    비교 연산자는 연산자 앞뒤에 있는 데이터 값을 비교하는 데 사용합니다
    ```sql
    -- 급여가 3000이상인 사원 조회
    SELECT *
    FROM EMP
    WHERE SAL >= 3000;
    ```
    |연산자|사용법|설명|
    |:----|:-----|:---| 
    | > | A > B | A 값이 B 값을 초과할 경우 true |
    | >= | A >= B | A 값이 B 값 이상일 경우 true |
    | < | A < B | A 값이 B 값 미만일 경우 true |
    | <= | A <= B | A 값이 B 값 이하일 경우 true |

    ```sql
    -- 비교 문자열이 문자 하나일 때
    -- 사원 이름의 첫 글자가 F와 같거나 알파벳 순서상 F보다 뒤에 있는 사원 조회
    SELECT *
    FROM EMP
    WHERE ENAME >= 'F';

    -- 비교 문자열이 문자 여러 개일 때
    -- 사원 이름이 FORZ와 같거나 FORZ를 포함한 문자열 보다 알파벳 순서로 앞에 있는 사원 조회
    SELECT *
    FROM EMP
    WHERE ENAME <= 'FORZ';
    ```
  - 등가 비교 연산자<br>
    연산자 양쪽 항목이 같은 값인지 검사하는 연산자로서 연산자의 양쪽 항목 값이 같으면 true가 반환됩니다.<br>
    이와는 반대로 연산자 양쪽 같이 다를 경우 true를 반환하는 연산자도 있습니다.

    |연산자|사용법|설명|
    |:----|:-----|:---| 
    | = | A = B | A 값이 B 값과 같을 경우 true, 다를 경우 false 반환 |
    | != | A != B | A 값이 B 값과 다를 경우 true, 같을 경우 false 반환 |
    | <> | A <> B | A 값이 B 값과 다를 경우 true, 같을 경우 false 반환 |
    | ^= | A ^= B | A 값이 B 값과 다를 경우 true, 같을 경우 false 반환 |
    ```sql
    -- 급여가 3000이 아닌 사원 조회하기
    -- 실무에서는 !=와 <>를 많이 사용합니다
    
    -- != 사용
    SELECT *
    FROM EMP
    WHERE SAL != 3000;

    -- <> 사용
    SELECT *
    FROM EMP
    WHERE SAL <> 3000;

    -- ^= 사용
    SELECT *
    FROM EMP
    WHERE SAL ^= 3000;    
    ```
    
- 논리 부정 연산자<br>
  위의 예시와 똑같은 결과를 출력하기 위해 사용할 수 있는 연산자가 하나 더 있는데 바로 NOT 연산자입니다.<br>
  A 값이 true일 경우 논리 부정 연산자의 결과 값은 false가 되고 반대로 A 값이 false일 경우 결과 값은 true가 됩니다.
  ```sql
  -- 급여가 3000이 아닌 사원 조회하기
  SELECT *
  FROM EMP
  WHERE NOT SAL = 3000;
  ```
  보통 NOT 연산자를 IN, BETWEEN, IS NULL 연산자와 함께 복합적으로 사용하는 경우가 많고,
  대소·등가 비교 연산자에 직접 사용하는 경우는 별로 없습니다.<br>
  하지만 복잡한 여러 개 조건식이 AND, OR로 묶여 있는 상태에서 정반대 결과를 얻고자 할 때에는 유용하게 사용할 수 있습니다.<br>
  복잡한 조건식에서 정반대의 최종 결과를 원할 때 NOT 연산자로 한 번에 뒤집어서 사용하는 것이 간편하고 SQL문 작성 시간도 줄일 수 있기 때문입니다.
  
- IN 연산자<br>
  OR 연산자로 여러 조건식을 묶어주는 것도 하나의 방법이지만, 조건이 늘어날수록 조건식을 많이 작성해야하기 때문에 번거롭습니다.
  이때 IN 연산자를 사용하면 특정 열에 해당하는 조건을 여러 개 지정할 수 있습니다.
  
  ```sql
  -- 기본형식
  SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
  FROM [조회할 테이블 이름]
  WHERE 열 이름 IN (데이터1, 데이터2, ... 데이터N);
  ```

  ```SQL
  -- 직업이 MANAGER, SALEMAN, CLERK 인 사원 조회하기
  SELECT *
  FROM EMP
  WHERE JOB IN ('MANAGER', 'SALEMAN', 'CLERK');
  ```

  IN 연산자 앞에 논리 부정 연산자 NOT을 사용하면 간단하게 반대의 경우를 조회할 수 있습니다.
  ```SQL
  -- 직업이 MANAGER, SALEMAN, CLERK이 아닌 사원 조회하기
  SELECT *
  FROM EMP
  WHERE JOB NOT IN ('MANAGER', 'SALEMAN', 'CLERK');
  ```
  
- BETWEEN A AND B 연산자<br>
  특정 열 값의 최소·최고 범위를 지정하여 해당 범위 내의 데이터만 조회할 경우에
  대소 비교 연산자 대신 BETWEEN A AND B 연산자를 사용하면 더 간단하게 표현할 수 있습니다.
  ```SQL
  -- 기본형식
  SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
  FROM [조회할 테이블 이름]
  WHERE 열 이름 BETWEEN 최솟값 AND 최댓값;
  ```

  ```SQL
  -- 급여가 2000이상 3000이하인 사원 조회하기
  SELECT *
  FROM EMP
  WHERE SAL BETWEEN 2000 AND 3000;

  ```SQL
  -- 급여가 2000이상 3000이하가 아닌 사원 조회하기
  SELECT *
  FROM EMP
  WHERE SAL NOT BETWEEN 2000 AND 3000;
  ```
- LIKE 연산자와 와일드 카드
  LIKE 연산자는 검색 기능처럼 일부 문자열이 포함된 데이터를 조회할 때 사용합니다.

  와일드 카드 종류
  |종류|설명|
  |:----|:-----|
  | _ | 어떤 값이든 상관없이 한 개의 문자 데이터를 의미 |
  | % | 길이와 상관없이(문자 없는 경우도 포함) 모든 문자 데이터를 의미 |
  
  ```SQL
  -- 대문자 S로 시작하는 사원 조회하기
  SELECT *
  FROM EMP
  WHERE ENAME LIKE 'S%';
  ```

  ```SQL
  -- 두 번째 글자가 L인 사원 조회하기
  SELECT *
  FROM EMP
  WHERE ENAME LIKE '_L%';
  ```

  ```SQL
  -- AM이 포함되어 있는 사원 조회하기
  SELECT *
  FROM EMP
  WHERE ENAME LIKE '%AM%';

  -- AM이 포함되어 있지 않은 사원 조회하기
  SELECT *
  FROM EMP
  WHERE ENAME NOT LIKE '%AM%';
  ```

  - 와일드 카드 문자가 데이터 일부일 경우<br>
    데이터에 와일드 카드 기호로 사용되는 _나 % 문자가 데이터로 포함되 경우가 간혹 있습니다.
    ESCAPE절을 사용하면 _, %를 와일드 카드 기호가 아닌 데이터로서의 문자로 다루는 것이 가능합니다.
    ```SQL
    SELECT *
    FROM SOME_TABLE
    WHERER SOME_COLUMN LIKE 'A\_A% ESCAPE '\';
    ```
    \ 문자 바로 뒤에 있는 _는 와일드 카드는 기호로서가 아닌 데이터에 포함된 문자로 인식하라는 의미입니다.
    ESCAPE 문자 \는 ESCAPE절에서 지정할 수 있습니다.

- IS NULL 연산자
  - NULL<br>
    NULL은 테이터베이스에서 중요한 의미가 있는 특수한 데이터 형식입니다.
    NULL은 데이터 값이 완전히 '비어있는' 상태를 말합니다.
    숫자 0은 값이 0이 존재한다는 뜻이므로 NULL과 혼동하지 않도록 주의해야 합니다.

    NULL의 의미
    |설명|예|
    |:---|:---|
    | 값이 존재하지 않음 | 통장을 개설한 적 없는 은행 고객의 계좌 번호 |
    | 해당 사항 없음 | 미혼인 고객의 결혼기념일 |
    | 노출할 수 없는 값 | 고객 비밀번호 찾기 같은 열람을 제한해야 하는 특정 개인 정보 |
    | 확정되지 않은 값 | 미성년자의 출신 대학 |

    따라서 NULL은 '현재 무슨 값인지 확정되지 않은 상태' 이거나 '값 자체가 존재하지 않는 상태'를 나타내는 데이터에 사용합니다.
    이 때문에 앞에서 살펴본 연산자는 대부분 연산 대상이 NULL일 때 연산 자체가 무의미해지는 현상이 발생합니다.

    ```SQL
    -- 추가 수당이 NULL값인 사원 조회
    SELECT *
    FROM EMP
    WHERE COMM = NULL;
    ```
    추가 수당 열 값이 NULL인 행이 나와야 할 것 같지만 실제로 출력되는 데이터는 없습니다.
    NULL은 산술 연산자와 비교 연산자로 비교해도 결과 값이 NULL이 되기 때문입니다.
    어떤 값인지 모르는 값에 숫자를 더해도 어떤 값인지 알 수 없고,
    어떤 값인지 모르는 값이 특정 값보다 큰지 작은지 알 수 없는 것과 같은 이치 입니다.
    ```
    NULL + 100 = NULL
    NULL > 100 = NULL
    ∞ + 100 = ∞
    ? > 100 = ?
    ```

    WHERE절은 조건식의 결과 값이 true인 행만 출력하는데 이처럼 연산 결과 값이 NULL이 되어 버리면
    조건식의 결과 값이 false도 true도 아니게 되므로 출력 대상에서 제외됩니다.
    따라서 지금까지 살펴본 연산자로는 특정 열의 데이터가 NULL인 경우를 구별해 낼 수 없습니다.

  특정 열 또는 연산의 결과 값이 NULL인지 여부를 확인하려면 IS NULL 연산자를 사용해야 합니다.

  ```SQL
  -- 추가수당(COMM)이 존재하지 않는 사원 조회하기
  SELECT *
  FROM EMP
  WHERE COMM IS NULL;

  -- 추가수당(COMM)이 존재하는 사원 조회하기
  SELECT *
  FROM EMP
  WHERE COMM IS NOT NULL;
  ```
  데이터가 NULL인지 아닌지를 확인하는 용도로 IS NULL과 IS NOT NULL 연산자는 자주 사용되므로 사용법을 기억해두세요.
  
- 집합 연산자
  두 개 이상의 SELECT문의 결과 값을 연결할 때 사용합니다.
   
  ```SQL
  -- 10번과 20번 부서에서 근무하는 사원 조회하기 
  SELECT EMPNO, ENAME, SAL, DEPTNO
  FORM EMP
  WHERE DEPTNO = 10
  UNION
  SELECT EMPNO, ENAME, SAL, DEPTNO
  FROM EMP
  WHERE DEPTNO 20;
  ```    
  주의할 점은 집합 연산자로 두 개의 SELECT문의 결과 값을 연결할 때 각 SELECT문이 출력하려는 열 개수와 각 열의 자료형이 순서별로 일치해야 한다는 것입니다. 
  
