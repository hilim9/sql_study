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
DISTINCE는 조회한 데이터의 내용에서 불필요한 중복을 제거하고 특정 데이터 종류만 확인하고 싶을 때 유용합니다
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
  
  


