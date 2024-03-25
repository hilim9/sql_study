## 다중행 함수
### 7-1. 하나의 열에 출력 결과를 담는 다중행 함수
그룹함수 또는 복수행 함수로도 불리는 다중행 함수는 여러행을 바탕으로 하나의 결과 값을 도출해 내기 위해 사용하는 함수입니다.
|함수|설명|
|:----|:----|
| SUM | 지정한 데이터의 합 반환 |
| COUNT | 지정한 데이터의 개수 반환 |
| MAX | 지정한 데이터 중 최댓값 반환 |
| MIN | 지정한 데이터 중 최솟값 반환 |
| AVG | 지정한 데이터의 평균값 반환 |

- 합계를 구하는 SUM 함수<br>
  SUM 함수는 데이터를 합을 구하는 함수입니다.
  SUM 함수를 분석하는 용도로 사용한다면 OVER 절을 사용할 수도 있습니다.
  ```SQL
  -- 기본형식
  SUM([DISTINCT, ALL 중 하나를 선택하거나 아무 값도 지정하지 않음(선택)]
      [합계를 구할 열이나 연산자, 함수를 사용한 데이터(필수)])
  OVER(분석을 위한 여러 문법을 지정)(선택)
  ```
  
  - SUM 함수와 DISTINCT, ALL 함께 사용하기
    ```SQL
    SELECT SUM(DISTINCT SAL),
           SUM(ALL SAL),
           SUM(SAL)
    FROM EMP;
    ```
    DISTINCT를 지정한 함수의 결과 값이 다르게 나오는 이유는 SUM 함수에 DISTINCT를 지정하면 같은 결과 값을 가진 데이터는 합계에서 한 번만 사용되기 때문입니다.
    즉, 중복 데이터는 제외하고 계산합니다. 하지만 일반적으로 합계를 구할 때 같은 값을 제외하는 경우는 많지 않으므로 보통은 SUM처럼 간단한 형식을 주로 사용합니다.
    
- 데이터 개수를 구해주는 COUNT 함수<br>
  COUNT 함수는 데이터 개수를 출력하는 데 사용합니다. COUNT 함수에 *을 사용하면 SELECT문의 결과 값으로 나온 행 데이터의 개수를 반환해 줍니다.
  옵션을 지정하지 않았을 때는 중복을 허용하여 결과 값을 반환하는 ALL을 기본으로 합니다.
  ```SQL
  COUNT([DISTINCT, ALL 중 하나를 선택하거나 아무 값도 지정하지 않음(선택)]
        [개수를 구할 열이나 연산자, 함수를 사용한 데이터(필수)])
  OVER(분석을 위한 여러 문법 지정)(선택)
  ```
  특정 조건을 만족하는 데이터를 COUNT 함수와 함께 사용한 결과 값은 다양한 분야에서 활용할 수 있습니다.
  예를 들어 웹 커뮤니티에서 특정 회원이 작성한 총 글 수, 댓글 수, 글에서 받은 찬성 수, 반대 수 등을 잘 조합하여 회원 등급이나 레벨 등을 관리할 수 있습니다.
  또는 웹 쇼핑몰에서 어떤 상품이 많이 구매되었는지 화면의 어느 위치에 있는 항목이 자주 선택되었는지 등을 분석할 때도 활용할 수 있습니다.

  - COUNT 함수와 DISTINCT, ALL 함께 사용하기
    ```SQL
    -- COUNT 함수를 사용하여 급여 개수 구하기
    SELECT COUNT(DISTINCT SAL),
           COUNT(ALL SAL),
           COUNT(SAL)
    FROM EMP;
    ```

    COUNT 함수를 사용하면 추가 수당 열처럼 NULL이 데이터로 포함되어 있을 경우, NULL데이터는 반환 개수에서 제외됩니다.
    ```SQL
    -- COUNT 함수를 사용하여 추가 수당 열 개수 출력하기
    SELECT COUNT(COMM)
    FROM EMP;

    -- COUNT 함수와 IS NOT NULL을 사용하여 추가 수당 열 개수 출력하기
    SELECT COUNT(COMM)
    FROM EMP
    WHERE COMM IS NOT NULL;

    -- 두 예제의 실행 결과 같습니다.
    ```
- 최댓값과 최솟값을 구하는 MAX, MIN 함수
  ```SQL
  -- 기본 문법
  -- MAX
  MAX([DISTINCT, ALL 중 하나를 선택하거나 아무 값도 지정하지 않음(선택)]
      [최댓값을 구할 열이나 연산자, 함수를 사용한 데이터(필수)])
  OVER(분석을 위한 여러 문법 지정)(선택)

  -- MIN
  MIN([DISTINCT, ALL 중 하나를 선택하거나 아무 값도 지정하지 않음(선택)]
      [최솟값을 구할 열이나 연산자, 함수를 사용한 데이터(필수)])
  OVER(분석을 위한 여러 문법 지정)(선택)
  ```

  - 숫자 데이터에 MAX, MIN 함수 사용하기
    ```SQL
    -- 부서 번호가 10번인 사원들의 최대 급여 출력하기
    SELECT MAX(SAL)
    FROM EMP
    WHERE DEPTNO = 10;

    -- 부서 번호가 10번인 사원들의 최소 급여 출력하기
    SELECT MIN(SAL)
    FROM EMP
    WHERE DEPTNO = 10;
    ```

  - 날짜 데이터에 MAX, MIN 함수 사용하기<br>
    오라클 데이터베이스에서는 날짜 및 문자 데이터의 크기 비교가 가능합니다.
    ```SQL
    -- 부서 번호가 20번인 사원의 입사일 중 제일 최근 입사일 출력하기
    SELECT MAX(HIREDATE)
    FROM EMP
    WHERE DEPTNO = 20;

    -- 부서 번호가 20번인 사원의 입사일 중 제일 오래된 입사일 출력하기
    SELECT MIN(HIREDATE)
    FROM EMP
    WHERE DEPTNO = 20;
    ```
- 평균 값을 구하는 AVG 함수<br>
  AVG 함수는 입력 데이터의 평균 값을 구하는 함수 입니다. 숫자 또는 숫자로 암시적 형 변환이 가능한 데이터만 사용할 수 있습니다.
  ```SQL
  -- 기본문법
  AVG([DISTINCT, ALL 중 하나를 선택하거나 아무 값도 지정하지 않은(선택)]
      [평균 값을 구할 열이나 연산자, 함수를 사용한 데이터(필수)]
  OVER(분석을 위한 여러 문법을 지정)(선택)
  ```

  ```SQL
  -- 부서 번호가 30인 사원들의 평균 급여 출력하기
  SELECT AVG(SAL)
  FROM EMP
  WHERE DEPTNO = 30;
  ```
  
  자주 사용하는 방식은 아니지만 DISTINCT를 지정하면 중복 값을 제외하고 평균값을 구하므로 결과 값이 달라질 수 있습니다.
  ```SQL
  -- DISTINCT로 중복을 제거한 급여 열의 평균 급여 구하기
  SELECT AVG(DISTINCT SAL)
  FORM EMP
  WHERE DEPTNO  = 30;
  ```

---
### 7-2. 결과 값을 원하는 열로 묶어 출력하는 GROUP BY 절
부서 번호 DEPTNO 열 값별로 급여의 평균값을 구하려면 각 부서 평균 값을 구하기 위해 SELECT문을 하나하나 제작해야 하거나 집합 연산자를 사용해서 구할 수 있는데,
이렇게 하면 한눈에 보기에도 번거롭고 나중에 특정 부서를 추가하거나 삭제할 때마다 SQL문을 수정해야 하므로 바람직하지 않습니다.
그래서 GROUP BY 절을 이용하여 특정 열 또는 데이터를 그룹화하여 사용합니다.

- GROUP BY절의 기본 사용법<br>
  여러 데이터에서 의미 있는 하나의 결과를 특정 열 값별로 묶어서 출력할 때 데이터를 '그룹화' 한다고 표현합니다. SELECT문에서는 GROUP BY 절을 작성하여 데이터를 그룹화 할 수 있습니다.
  주의할 점은 GROUP BY 절에는 별칭이 인식되지 않습니다. 즉 열 이름이나 연산식을 그대로 지정해 주어야 합니다.
  ```SQL
  -- 기본형식
  SELECT   [조회할 열1 이름], [열2 이름], ..., [열N 이름]
  FROM     [조회할 테이블 이름]
  WHERE    [조회할 행을 선별하는 조건식]
  GROUP BY [그룹화 할 열을 지정(여러 개 지정 가능)]
  ORDER BY [정렬하려는 열 지정]
  ```

  ```SQL
  -- GROUP BY를 사용하여 부서별 평균 급여 출력하기
  SELECT AVG(SAL), DEPTNO
  FROM EMP
  GROUP BY DEPTNO;
  ```

- GROUP BY 절을 사용할 때 유의점<br>
  GROUP BY 절을 사용하여 출력 데이터를 그룹화할 경우 유의해야 할 점이 있는데
  바로 다중행 함수를 사용하지 않은 일반 열은 GROUP BY 절에 명시하지 않으면 SELECT절에서 사용할 수 없습니다.
  ```SQL
  -- GROUP BY절에 없는 열을 SELECT절에 포함했을 경우
  SELECT ENAME, DEPTNO, AVG(SAL)
  FROM EMP
  GROUP BY DEPTNO;

  -- 에러 출력 [Error] Execution (1:8): ORA-00979: GROUP BY 표현식이 아닙니다.
  ```
  위의 예시를 살펴보면 결과가 출력되지 않고 오류가 발생합니다.
  DEPTNO를 기준으로 그룹하되어 DEPTNO 열과 AVG(SAL)열은 한 행으로 출력되지만,
  ENAME 열은 여러 행으로 구성되어 각 열별 데이터 수가 달라져 출력이 불가능해집니다.

---
### 7-3. GROUP BY 절에 조건을 줄 때 사용하는 HAVING 절
HAVING 절은 SELECT문에 GROUP BY 절이 존재할 때만 사용할 수 있습니다. 그리고 GROUP BY 절을 통해 그룹화된 결과 값의 범위를 제한하는 데 사용합니다.

- HAVING 절의 기본 사용법<br>
  HAVING 절은 GROUP BY 절이 존재할 경우 GROUP BY 절 바로 다음에 작성합니다. 그리고 GROUP BY 절과 마찬가지로 별칭은 사용할 수 없습니다.
  ```SQL
  -- 기본형식
  SELECT   [조회할 열1 이름], [열2 이름], ..., [열N 이름]
  FROM     [조회할 테이블 이름]
  WHERE    [조회할 행을 선별하는 조건식]
  GROUP BY [그룹화 할 열을 지정(여러 개 지정 가능)]
  HAVING   [출력 그룹을 제한하는 조건식]
  ORDER BY [정렬하려는 열 지정]
  ```

- HAVING 절을 사용할 때 유의점<br>
  조건실을 지정한다는 점에서 HAVING 절이 WHERE 절과 비슷하다고 생각할 수도 있습니다. 
  HAVING 절도 WHERE 절처럼 지정한 조건식이 참인 결과만 출력한다는 점에서 비슷한 부분이 있습니다. 
  하지만 WHERE 절은 출력 대상 행을 제한하고, HAVING 절은 그룹화된 대상을 출력에서 제한하므로 쓰임새는 전혀 다릅니다.  
  
  만약 출력 결과를 제한하기 위해 HAVING을 사용하지 않고 조건식을 WHERE 절에 명시하면 다음과 같이 SELECT 문이 실행되지 않고 오류가 발생합니다.
  ```SQL
  -- HAVING절 대신 WHERE절을 잘못 사용했을 경우
  SELECT DEPTNO, JOB, AVG(SAL)
  FROM EMP
  WHERE AVG(SAL) >= 2000
  GROUP BY DEPTNO, JOB
  ORDER BY DEPTNO, JOB;

  -- 에러 출력 [Error] Execution (17:8): ORA-00934: 그룹 함수는 허가되지 않습니다.
  ```

- WHERE 절과 HAVING 절의 차이점
  ```SQL
  -- WHERE절을 사용하지 않고 HAVING절만 사용한 경우
  SELECT DEPTNO, JOB, AVG(SAL)
  FROM EMP
  GROUP BY DEPTNO, JOB
  HAVING AVG(SAL) >= 2000
  ORDER BY DEPTNO, JOB;
  ```
  ![1](https://github.com/hilim9/sql_study/assets/134352560/c893434d-fc16-441d-971a-dbe68dc7bc1f)

  ```SQL
  -- WHERE절과 HAVING절을 모두 사용한 경우
  -- WHERE절이 GROUP BY절, HAVING절보다 먼저 실행됩니다.
  SELECT DEPTNO, JOB, AVG(SAL)
  FROM EMP
  WHERE SAL <= 3000
  GROUP BY DEPTNO, JOB
  HAVING AVG(SAL) >= 2000
  ORDER BY DEPTNO, JOB;
  ```
  ![2](https://github.com/hilim9/sql_study/assets/134352560/6ac932ed-3d79-4593-a0f0-5718cdf621a9)

  WHERE 절을 추가한 SELECT문에서는 10번 부서의 PRESIDENT 데이터가 출력되지 않습니다.
  이는 WHERE절이 GROUP BY절과 HAVING절을 사용한 데이터 그룹화보다 먼저 출력 대상이 될 행을 제한하기 때문입니다.
  즉, GROUP BY절을 수행하기 전에 WHERE절의 조건식으로 출력 행의 제한이 먼저 이루어진다는 것을 반드시 기억하세요.

---
### 7-4. 그룹화와 관련된 여러 함수
- ROLLUP, CUBE, GROUPING SETS 함수
  - ROLLUP, CUBE 함수
    ROLLUP, CUBE, GROUPING SETS 함수는 GROUP BY절에 지정할 수 있는 특수 함수입니다.
    ROLLUP 함수와 CUBE 함수는 그룹화 데이터의 합계를 출력할 때 유용하게 사용할 수 있습니다.
    ```SQL
    -- ROLLUP 함수 기본형식
    SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
    FROM [조회할 테이블 이름]
    WHERE [조회할 행을 선별하는 조건식]
    GROUP BY ROLLUP [그룹화 열 지정(여러개 지정 가능)];
    ```

    ```SQL
    -- CUBE 함수 기본형식
    SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
    FROM [조회할 테이블 이름]
    WHERE [조회할 행을 선별하는 조건식]
    GROUP BY CUBE [그룹화 열 지정(여러개 지정 가능)];
    ```

    ```SQL
    -- 기존 GROUP BY절만 사용한 그룹화
    SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL)
    FROM EMP
    GROUP BY DEPTNO, JOB
    ORDER BY DEPTNO, JOB;
    ```

    ROLLUP 함수는 명시한 열을 소그룹부터 대그룹의 순서로 각 그룹별 결과를 출력하고 마지막에 총 데이터의 결과를 출력합니다.
    ROLLUP 함수에 명시한 열에 한아여 결과가 출력된다는 것과 ROLLUP 함수에는 그룹함수를 지정할 수 없다는 것을 기억해주세요.
    ```SQL
    -- ROLLUP 함수를 적용한 그룹화
    SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL)
    FROM EMP
    GROUP BY ROLLUP(DEPTNO, JOB);
    ```

    CUBE 함수는 ROLLUP 함수를 사용했을 때보다 좀더 많은 결과가 나옵니다. 여기에서 주의 깊게 확인해야 하는 부분은 부서와 상관없이 직책별 결과가 함깨 출력되고 있는 부분입니다. 
    이렇듯 CUBE 함수는 지정한 모든 열에서 가능한 조합의 결과를 모두 출력합니다.
    ```SQL
    -- CUBE 함수를 적용한 그룹화
    SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL)
    FROM EMP
    GROUP BY CUBE(DEPTNO, JOB);
    ORDER BY DEPTNO, JOB;
    ```
    그룹화 순서대로 출력해주는 ROLLUP 함수는 지정한 열 수에 따라 다음과 같이 결과 값이 조합됩니다. 
    n개의 열을 지정하면 기본적으로 n+1개 조합이 출력됩니다.<br>
    ```
    ROLLUP(A, B, C)
    1. A 그룹별 B 그룹별 C 그룹에 해당하는 결과 출력
    2. A 그룹별 B 그룹에 해당하는 결과 출력
    3. A 그룹에 해당하는 결과 출력
    4. 전체 데이터 결과 출력
    ```
    CUBE 함수는 지정한 모든 열의 조합을 사용하므로 다음과 같은 결과가 출력됩니다. 
    CUBE 함수에 n개 열을 지정하면 2ⁿ개 조합이 출력됩니다. 
    특히 CUBE 함수는 제곱수로 조합 경우의 수가 올라가므로 감당하기 어려울 정도의 기하급수적인 증가가 일어납니다. 
    이를 방지하기 위해 필요한 조합의 출력만 보려면 ROLLUP 함수와 CUBE함수에 그룹화 열 중 일부만을 지정할 수도 있습니다.<br>
    ```
    CUBE(A, B, C)
    1. A 그룹별 B 그룹별 C 그룹에 해당하는 결과 출력
    2. A 그룹별 B 그룹에 해당하는 결과 출력
    3. B 그룹별 C 그룹에 해당하는 결과 출력
    4. A 그룹별 C 그룹에 해당하는 결과 출력
    5. A 그룹 결과
    6. B 그룹 결과
    7. C 그룹 결과
    8. 전체 데이터 결과
    ```
  - GROUPING SETS 함수
    GROUPING SETS 함수는 같은 수준의 그룹화 열일 여러 개일 때 각 열별 그룹화를 통해 결과 값을 출력하는 데 사용합니다.
    ```SQL
    -- 기본형식
    SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
    FROM   [조회할 테이블 이름]
    WHERE  [조회할 행을 선별하는 조건식]
    GROUP BY GROUPING SETS [그룹화 열 지정(여러 개 지정 가능)];
    ```

    ```SQL
    -- GROUPING SETS 함수를 사용하여 열별로 그룹으로 묶어 출력하기
    SELECT DEPTNO, JOB, COUNT(*)
    FROM EMP
    GROUP BY GROUPING SETS(DEPTNO, JOB)
    ORDER BY DEPTNO, JOB;
    ```
    ![11](https://github.com/hilim9/sql_study/assets/134352560/d45ca034-0435-46d0-9328-cbb4bb2d03af)

    실행 결과를 살펴보면 그룹화를 위해 지정한 열이 계층적으로 분류되지 않고 각각 따로 그룹화한 후 연산을 수행했음을 알 수 있습니다.

- 그룹화 함수
  - GROUPING 함수<br>
    ROLLUP 또는 CUBE 함수를 사용한 GROUP BY절에 그룹화 대상으로 지정한 열일 그룹화된 상태로 결과가 집계되었는지 확인하는 데 사용합니다. 
    GROUP BY절에 명시된 열 중 하나를 지정할 수 있습니다.

    ```SQL
    -- 기본형식
    SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
      GROUPING [GROUP BY절에 ROLLUP 또는 CUBE에 명시한 그룹화 할 열 이름]
    FROM [조회할 테이블 이름]
    WHERE [조회할 행을 선별하는 조건식]
    GROUP BY ROLLUP 또는 CUBE [그룹화할 열];
    ```
    
    GROUPING 함수의 결과 값을 0,1로 구별하여 출력하면 현재 출력되는 데이터가 어떤 열의 그룹화를 통해 나온 것인지 알 수 있습니다.
    ```SQL
    -- DEPTNO, JOB열의 그룹화 결과 여부를 GROUPING 함수로 확인하기
    SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL),
           GROUPING(DEPTNO),
           GROUPING(JOB)
    FROM EMP
    GROUP BY CUBE(DEPTNO, JOB)
    ORDER BY DEPTNO, JOB;
    ```
    ![312321](https://github.com/hilim9/sql_study/assets/134352560/ef22271d-c1a8-4912-a98b-dfabffcc169d)

    여기에서 0은 GROUPING 함수에 지정한 열이 그룹화되었음을 의미하고 1이 나왔다는 것은 그룹화되지 않은 데이터를 의미합니다.

  - GROUPING_ID 함수<br>
    ROLLUP 또는 CUBE 함수로 연산할 때 특정 열이 그룹화되었는지 출력하는 함수입니다.
    그룹화 여부를 검사할 열을 하나씩 지정하는 GROUPING 함수와 달리 GROUPING_ID 함수는 한 번에 여러 열을 지정할 수 있습니다.

    ```SQL
    -- 기본형식
    SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
      GROUPING_ID [그룹화 여부를 확인할 열(여러 개 지정 가능)]
    FROM [조회할 테이블 이름]
    WHERE [조회할 행을 선별하는 조건식]
    GROUP BY ROLLUP 또는 CUBE [그룹화 할 열];
    ```

    GROUPING_ID 함수는 한 번에 여러 개 열을 지정할 수 있으므로 지정한 열의 순서에 따라 0,1 값이 하나씩 출력됩니다.
    이렇게 0과 1로 구성된 그룹화 비트 벡터 값을 2진수로 보고 10진수로 바꾼 값이 최종결과로 출력됩니다.
    ```SQL
    -- DEPTNO, JOB을 함께 명시한 GROUPING_ID 함수 사용하기
    SELECT DEPTNO, JOB, COUNT(*), SUM(SAL),
           GROUPING(DEPTNO),
           GROUPING(JOB),
           GROUPING_ID(DEPTNO, JOB)
    FROM EMP
    GROUP BY CUBE(DEPTNO, JOB)
    ORDER BY DEPTNO, JOB;
    ```
    ![12213](https://github.com/hilim9/sql_study/assets/134352560/e73c99c9-e607-4254-919b-d4cd6150fed3)

- LISTAGG 함수<br>
  LISTAGG 함수는 오라클 11g 버전부터 사용할 수 있는 함수입니다. 그룹에 속해 있는 데이터를 가로로 나열할 때 사용합니다.

  ```SQL
  -- 기본형식
  SELECT [조회할 열1 이름], [열2 이름], ..., [열N 이름]
         LISTAGG([나열할 열(필수))], [각 데이터를 구분하는 구분자(선택)])
         WITHIN GROUP(OREDR BY 나열할 열의 정렬 기준 열 (선택))
  FROM [조회할 테이블 이름]
  WHERE [조회할 행을 선별하는 조건식];
  ```

  ```SQL
  -- 부서별 사원 이름을 나란히 나열하여 출력하기
  SELECT DEPTNO,
         LISTAGG(ENAME, ', ')
         WITHIN GROUP(ORDER BY SAL DESC) AS ENAMES
  FROM EMP
  GROUP BY DEPTNO;
  ```

  ![123](https://github.com/hilim9/sql_study/assets/134352560/433e5053-6654-4fe9-8cea-ad182f900d3f)

  
