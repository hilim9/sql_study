## 조인(join)
### 8-1. 조인
- 집합 연산자와 조인의 차이점<br>
  ```SQL
  집합 연산자를 사용: 두 개 이상 SELECT문의 결과 값을 세로로 연결한 것
  조인을 사용: 두 개 이상의 테이블 데이터를 가로로 연결한 것
  ```

- 조인 조건이 없을 때의 문제점<br>
  조인을 통한 출력은 결과로 나올 수 있는 모든 행을 조합하기 때문에 정확히 맞아떨어지지 않는 데이터도 함께 출력됩니다.
  그래서 조인 대상 테이블이 많을수록 조합 데이터 중 정확한 데이터만 뽑아내기 위해 출력 행을 선정하는 조건식을 명시하는 WHERE절을 잘 사용하는 것이 중요합니다.

  ```SQL
  -- 열 이름을 비교하는 조건식으로 조인하기
  SELECT *
  FROM EMP, DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO
  ORDER BY EMPNO;
  ```

  ![123](https://github.com/hilim9/sql_study/assets/134352560/34d95a4f-be68-4dc3-98b4-038ba9f61575)

- 테이블의 별칭 설정<br>
  FROM절에 지정한 테이블에는 SELECT절의 열에 사용한 것처럼 별칭을 지정할 수 있습니다.
  테이블에 별칭을 지정할 때는 명시한 테이블 이름에서 한 칸 띄운 후에 지정합니다.
  ```SQL
  -- 사용법
  FROM 테이블 이름1 별칭1, 테이블 이름2 별칭 ...
  ```

  ```SQL
  -- 테이블 이름을 별칭으로 표현하기
  SELECT *
  FROM EMP E, DEPT D
  WHERE E.DEPTNO = D.DEPTNO
  ORDER BY EMPNO;
  ```
---
### 8-2. 조인 종류
두 개 이상의 테이블을 하나의 테이블처럼 가로로 늘어뜨려 출력하기 위해 사용하는 조인은 
대상 데이터를 어떻게 연결하느냐에 따라 등가조인, 비등가 조인, 자체 조인, 외부 조인 등으로 구분합니다.

- 등가 조인<br>
  등가 조인(equi join)은 테이블을 연결한 후에 출력 행을 각 테이블의 특정 열에 일치한 데이터를 기준으로 선정하는 방식입니다.
  등가 조인은 내부 조인 또는 단순 조인으로 부르기도 합니다.
  등가 조인은 일반적으로 가장 많이 사용되는 조인 방식이며, 특정 열값이 일치한 출력 결과를 사용하는 방식입니다.

  - 여러 테이블의 열 이름이 같을 때 유의점<br>
    등가 조인을 사용할 때 조인 조건이 되는 각 테이블의 열이름이 같을 경우에 해당 열 이름을 테이블 구분 없이 명시하면 오류가 발생하므로
    어느 테이블에 속해 있는 열인지 반드시 명시해야 합니다.

    ```SQL
    -- 두 테이블에 부서 번호가 똑같은 열 이름으로 포함되어 있을 때    
    SELECT EMPNO, ENAME, SAL, DEPTNO, DNAME, LOC
    FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO;
    -- 오류 발생
    ```
    ![345](https://github.com/hilim9/sql_study/assets/134352560/19b96e9e-b013-4d07-a6d9-51df7a8c068e)

    ```SQL
    -- 열 이름에 각각의 테이블 이름도 함께 명시할 때
    SELECT E.EMPNO, E.ENAME, E.SAL, D.DEPTNO, D.DNAME, D.LOC
    FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO
    ORDER BY D.DEPTNO, E.EMPNO;
    ```
    ![234](https://github.com/hilim9/sql_study/assets/134352560/408b24db-9335-456d-92c5-b03d2b73d9f4)


  - WHERE절에 조건식 추가하여 출력 범위 설정하기<br>
    출력 행을 더 제한하고 싶다면 WHERE절에 조건식을 추가로 지정해 줄 수 있습니다.
    ```SQL
    -- WHERE절에 추가로 조건식 넣어 출력하기
    SELECT E.EMPNO, E.ENAME, E.SAL, D.DEPTNO, D.DNAME, D.LOC
    FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO
      AND SAL >= 3000;
    ```
    ![123](https://github.com/hilim9/sql_study/assets/134352560/2dc37c9b-692b-4efe-a259-3a091bffd055)

  - 조인 테이블 개수와 조건식 개수의 관계<br>
    조인 조건을 제대로 지정하지 않으면 데카르트 곱(CARTESIAN PRODUCT) 때문에 정확히 연결되지 않아서 필요 없는 데이터까지 모두 조합되어 출력됩니다.
    데카르트 곱 현상이 일어나지 않게 하는 데 필요한 조건식의 최소 개수는 조인 테이블 개수에서 하나를 뺀 값입니다.
    WHERE절의 조건식을 사용해 테이블을 조인할 때 반드시 각 테이블을 정확히 연결하는 조건식이 최소한 전체 테이블 수보다 하나 적은 수만큼은 있어야 합니다.
  
