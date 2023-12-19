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

- 문자열 길이를 구하는 LENGTH 함수<br>
  - LENGTH 함수 사용하기
    ```SQL
    -- 사원 이름의 문자열 길이 구하기
    SELECT ENAME, LENGTH(ENAME)
    FROM EMP;
    ```
  - WHERE절에서 LENGTH 함수 사용하기
    ```SQL
    -- 사원 이름 길이가 5 이상인 행 출력하기
    SELECT ENAME, LENGTH(ENAME)
    FROM EMP
    WHERE LENGTH(ENAME) >= 5;
    ```
  - LENGTH 함수와 LENGTHB 함수 비교하기<br>
    LENGTH 함수는 문자열 길이를 반환하고, LENGTHB 함수는 문자열의 바이트 수를 반환합니다.
    ```SQL
    SELECT LENGTH('한글'), LENGTHB('한글')
    FROM DUAL;
    ```
    ◎ DUAL 테이블은 어떤 테이블인가요?<br>
      오라클의 최고 권한 관리자 계정인 SYS 소유의 테이블로 SCOTT 계정도 사용할 수 있는 더미(dummy)테이블 입니다.
   
- 문자열 일부를 추출하는 SUBSTR 함수
  
  |함수|설명|
  |:----|:----|
  | SUBSTR(문자열 데이터, 시작 위치, 추출 길이) | 문자열 데이터의 시작 위치부터 추출 길이만큼 추출합니다. 시작 위치가 음수일 경우에는 마지막 위치부터 거슬러 올라간 위치에서 시작합니다. |
  | SUBSTR(문자열 데이터, 시작 위치) | 문자열 데이터의 시작 위치부터 문자열 데이터 끝까지 추출합니다. 시작 위치가 음수일 경우에는 마지막 위치부터 거슬러 올라간 위치에서 끝까지 추출합니다. |

  - SUBSTR 함수 사용하기
    ```SQL
    -- SUBSTR(JOB, 1, 2): 첫 번째 글자부터 두 글자 출력
    -- SUBSTR(JOB, 3, 2): 세 번째 글자부터 두 글자 출력
    -- SUBSTR(JOB, 5): 다섯 번째 글자부터 끝까지 출력
    SELECT JOB, SUBSTR(JOB, 1, 2), SUBSTR(JOB, 3, 2), SUBSTR(JOB, 5)
    FROM EMP;
    ```
  - SUBSTR 함수와 다른 함수 함께 사용하기<br>
    다른 함수의 결과 값을 SUBSTR 함수의 입력 값으로 사용할 수도 있습니다. SUBSTR 함수 안에 다음과 같이 LENGTH 함수를 사용하는 경우도 종종 있습니다.
    ```SQL
    SELECT JOB,
      SUBSTR(JOB, -LENGTH(JOB)),
      SUBSTR(JOB, -LENGTH(JOB), 2),
      SUBSTR(JOB, -3)
    FROM EMP;
    ```
- 문자열 데이터 안에서 특정 문자 위치를 찾는 INSTR 함수
  ```SQL
  -- 기본 형식
  INSTR([대상 문자열 데이터 (필수)],
  [위치를 찾으려는 부분 문자(필수)],
  [위치 찾기를 시작할 대상 문자열 데이터 위치(선택, 기본값은 1)],
  [시작 위치에서 찾으려는 문자가 몇 번째인지 지정(선택, 기본값은 1)])
  ```
  - 특정 문자 위치 찾기
    ```SQL
    SELECT INSTR('HELLO, ORACLE!', 'L') AS INSTR_1,
          INSTR('HELLO, ORACLE!', 'L','5') AS INSTR_2,
          INSTR('HELLO, ORACLE!', 'L', 2, 2) AS INSTR_3,
    FROM DUAL;
    ```
    위치 찾기를 시작하는 위치 값에 음수를 쓸 때 원본 문자열 데이터의 오른쪽 끝부터 왼쪽 방향으로 검색합니다.
    만약 찾으려는 문자가 문자열 데이터에 포함되어 있지 않다면 위치 값이 없으므로 0을 반환합니다.

  - 특정 문자를 포함하고 있는 행 찾기
    ```SQL
    -- INSTR 함수로 이름에 문자 S가 있는 사원 찾기
    SELECT *
    FROM EMP
    WHERE INSTR(ENAME, 'S') > 0;

    -- LIKE 연산자로 이름에 문자 S가 있는 사원 찾기
    SELECT *
    FROM EMP
    WHERE ENAME LIKE '%S%'
    ```
- 특정 문자를 다른 문자로 바꾸는 REPLACE 함수
  ```SQL
  -- 기본형식
  REPLACE([문자열 데이터 또는 열 이름(필수)], [찾는 문자(필수)], [대체할 문자(선택)]
  ```
  만약 대체할 문자를 입력하지 않는다면 찾는 문자로 지정한 문자는 문자열 데이터에서 삭제됩니다.
  ```SQL
  -- 문자열 안에 있는 특정 문자 바꾸기
  SELECT '010-1234-5678' AS REPLACE_BEFORE,
        REPLACE('010-1234-5678', '-', ' ') AS REPLACE_1, -- 한 칸 공백으로 바꾸어 출력
        REPLACE('010-1234-5678', '-') AS REPLACE_2 -- 문자가 삭제된 상태로 출력 
  FROM DUAL;
  ```
  별칭 REPLACE_1은 문자를 한 칸 공백으로 바꾸어서 출력하고, REPLACE_2은 대체할 문자를 지정하지 않아서 '-' 문자가 삭제된 상태로 출력됩니다.
  REPLACE 함수는 카드 번호나 주민 번호, 계좌번호, 휴대전화 번호 또는 날짜나 시간을 나타내는 데이터처럼 특정 문자가 중간중간 끼어 있는 데이터에서 해당 문자를 없애버리거나 다른 문자로 바꾸어 출력할 때 종종 사용합니다.

- 데이터의 빈 공간을 특정 문자로 채우는 LPAD, RPAD 함수<br>
  데이터와 자릿수를 지정한 후 데이터 길이가 지정한 자릿수보다 작을 경우에 나머지 공간을 특정 문자로 채우는 함수입니다.
  만약 빈 공간에 채울 문자를 지정하지 않으면 LPAD와 RPAD 함수는 빈 공간의 자릿수만큼 공백 문자로 띄웁니다.
  ```SQL
  -- 기본형식
  LPAD([문자열 데이터 또는 열이름(필수)], [데이터의 자릿수(필수)], [빈 공간에 채울 문자(선택)]
  RPAD([문자열 데이터 또는 열이름(필수)], [데이터의 자릿수(필수)], [빈 공간에 채울 문자(선택)]
  ```
  ```SQL
  -- 출력 예시
  SELECT 'Oracle',
        LPAD('Oracle', 10, '#') AS LPAD_1, -- #으로 공백을 채움
        RPAD('Oracle', 10, '*') AS RPAD_1, -- *으로 공백을 채움
        LPAD('Oracle', 10) AS LPAD_2, -- 입력값이 없어 빈 공백으로 채움
        RPAD('Oracle', 10) AS RPAD_2, -- 입력값이 없어 빈 공백으로 채움
  FROM DUAL;
  ```
  
  - 특정 문자로 자릿수 채워서 출력하기
    ```SQL
    -- 개인정보 뒷자리 * 표시로 출력하기
    SELECT
          RPAD('971225-', 14, '*') AS RPAD_JMNO,
          RPAD('010-1234-', 13, '*') AS RPAD_PHONE
    FROM DUAL;  
    ```
  
  
