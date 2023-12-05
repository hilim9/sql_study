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
  
  


