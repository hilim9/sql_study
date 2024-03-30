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
