## 오라클 함수
### 6-2 문자 데이터를 가공하는 문자 함수
- 대·소문자를 바꿔 주는 UPPER, LOWER, INITCAT 함수<br>
  함수를 사용하려면 입력 데이터에 열 이름이나 데이터를 직접 지정해야 합니다.
  제목이나 본문에 문자열이 포함된 데이터를 검색하는 기능을 구현할 때 다음과 같이 LIKE 연산자를 와일드 카드와 함께 사용할 수 있습니다.
 
  |연산자|사용법|
  |:----|:-----|
  | UPPER(문자열) | 괄호 안 문자 데이터를 모두 대문자로 변환하여 반환 |
  | LOWER(문자열) | 괄호 안 문자 데이터를 모두 소문자로 변환하여 반환 |
  | INITCAP(문자열) | 괄호 안 문자 데이터 중 첫 글자는 대문자로, 나머지 문자를 소문자로 변환 후 반환 |

  ```SQL
  -- 사원 이름이 SCOTT인 데이터 찾기
  SELECT *
  FROM EMP
  WHERE UPPER(ENAME) = UPPER('scott');

  -- 사원 이름에 SCOTT 단어를 포함한 데이터 찾기
  SELECT *
  FROM EMP
  WHERE UPPER(ENAME) LIKE UPPER('scott');
  ```

  
