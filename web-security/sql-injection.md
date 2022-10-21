- (What) 악의적인 사용자가 보안상의 취약점을 이용해 임의의 SQL문을 주입 후 실행시켜 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위다.
- (What) DB에 쿼리 시 입력된 데이터의 유효성 검증을 하지 않아 개발자가 의도하지 않은 동적 쿼리를 생성하여 DB 정보를 열람하거나 조작할 수 있는 보안 취약점이다.
- (Why) 웹 애플리케이션은 User의 행동에 따라 DB 데이터가 서로 다르게 표시된다. 이를 위해 쿼리는 유저가 입력한 데이터를 포함하여 다이나믹하게 변하는데, 개발자가 의도하지 않은 쿼리가 실행 시 정보를 열람할 수 있다.

## 공격 종류 및 방법
### 1. Error based SQL Injection
``` sql
1️⃣ SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2'

# INPUT1 -> ' OR 1=1 --
2️⃣ SELECT * FROM Users WHERE id = '' OR 1=1 -- ' AND password = 'INPUT2'
➡️ SELECT * FROM Users
```
- 1️⃣ 구문에서 INPUT1 값을 ' OR 1=1 -- 을 넣게 되면,
  1. WHERE 절에 있는 싱글 쿼터를 닫고 OR 1=1 구문을 이용해 WHERE 절을 모두 참으로 만든다.
  2. --으로 뒤의 구문을 모두 주석 처리한다.
- 해당 구문은 Users 테이블에 있는 모든 정보를 조회하게 됨으로 가장 먼저 만들어진 계정에 로그인이 가능하다.


### 2. Union based SQL Injection
- `Union` 키워드는 두 개의 쿼리문에 대한 결과를 통합해 하나의 테이블로 보여주는 키워드이다.
- 정상적인 쿼리문에 Union 키워드를 사용하여 인젝션에 성공하면 원하는 쿼리문을 실행할 수 있다.
- Union 하는 두 테이블의 컬럼 수 일치, 데이터 형 일치해야 Union Injection을 성공할 수 있다.
``` sql
# Board table
id     title     contents
-------------------------
1      hi        hello
2      bye       bye bye

1️⃣ SELECT * FROM Board WHERE title LIKE '%INPUT%' OR contents '%INPUT%'

# INPUT -> ' UNION SELECT null, id, password FROM Users --
2️⃣ SELECT * FROM Board WHERE title LIKE '%' UNION SELECT null, id, password FROM Users -- INPUT%' OR contents '%' UNION SELECT null, id, password FROM Users --%'
```
- 1️⃣ 쿼리문은 Board 테이블의 게시글을 검색하는 쿼리문으로 입력값을 title과 contents 컬럼의 데이터와 비교 후 비슷한 글자가 있는 레코드를 조회한다.
- 입력값으로 Union 키워드와 함께 컬럼 수를 맞춰 조회 구문을 넣어주면 두 쿼리문이 합쳐져 하나의 테이블로 보여진다.
- 인젝션한 구문(2️⃣)은 사용자 id와 password를 요청하는 쿼리문으로 인젝션 성공 시 사용자의 개인정보가 게시글과 함께 보여진다.


### 3. Blind SQL Injection > Boolean based SQL
- 디비로부터 특정 값이나 데이터를 전달받지 않고 단순히 참과 거짓의 정보만 알 수 있을 때 사용한다.
- 로그인 폼에서 SQL Injection이 가능하다고 가정했을 때, 서버가 응답하는 로그인 성공과 실패 메시지를 이용하여 디비의 테이블 정보 등을 추출할 수 있다.
```sql
1️⃣ SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2'

# INPUT1 -> test123' AND ASCII(SUBSTR(SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0, 1), 1, 1)) > 100 --
2️⃣ SELECT * FROM Users WHERE id = 'test123' AND ASCII(SUBSTR(SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0, 1), 1, 1)) > 100 -- ' AND password = 'INPUT2'
```
- 2️⃣ 구문은 디비의 테이블 명을 알아내는 방법으로, 인젝션이 가능한 로그인 폼을 통해 악의적인 사용자는 임의로 가입한 아이디와 함께 테이블명을 알아낼 수 있는 구문을 주입한다.
- `ASCII(SUBSTR(SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0, 1), 1, 1)) > 100`
  1. 테이블 명을 조회하는 구문으로 limit 키워드를 통해 하나의 테이블만 조회하고 substr 함수로 첫 글자를 조회 후 ASCII함수를 통해 아스키 값으로 변환한다.
  1. 조회한 테이블 명이 Users라면 'U'자가 조회될 것이고 100 숫자와 비교하게 된다.
  1. 거짓이면 로그인에 실패하고 참이 될 때 까지 100 숫자를 변경하면서 비교하면 된다.


### 4. Blind SQL Injection > Time based SQL
- 서버로부터 특정한 응답 대신 참 또는 거짓의 응답을 통해 디비의 정보를 유출하는 기법이다.
- MySQL 함수 중 sleep, benchmark 함수가 사용된다.


### 5. Stored Procedure SQL Injection
- 일련의 쿼리들을 모아 하나의 함수처럼 사용하기 위한 것이다.
- 공격에 사용되는 대표적인 저장 프로시저는 MS-SQL의 xp_cmdshell로 윈도우 명령어를 사용할 수 있게 된다.
- 공격자가 시스템 권한을 획득해야해 공격난이도가 높으나 공격 성공 시 서버에 직접적인 피해를 입힐 수 있다.

## 대응 방안
### 1. 입력값에 대한 검증
- 사용자 입력 값에 대한 검증이 필요하다.
- 서버 단에서 화이트 리스트 기반으로 검증해야 한다.
  - 허용된 문자를 제외한 나머지 문자들은 모두 허용하지 않는 방식으로 블랙 리스트 방식보다 보안성 측에서 더 강력한 방법이다.
  - 웹 애플리케이션 기능에 따라 알맞게 화이트 리스트를 작성해야 하는 번거로움이 있다.
  - 번거로움과 유지보수의 편리함을 위해 정규식을 이용해 화이트 리스트를 범주화/패턴화 시키는 게 좋다.
- 블랙 리스트 기반으로 검증 시 수많은 차단 리스트를 등록해야 하고 하나라도 빠질 시 공격에 성공하게 된다.
  - SQL 쿼리의 구조를 변경시키는 문자나 키워드, 특수문자를 제한하는 방식이다.
  - SQL 에약어인 UNION, GROUP BY, COUNT 등의 함수명, 세미콜론, 주석 등의 특수문자들을 블랙리스트로 미리 정의하고 정의되어 있는 문자나 키워드가 외부 입력값에 존재하면 공백으로 치환하는 방식으로 방어한다.
### 2. Prepared Statement 구문 사용
- Prepared Statement란 미리 쿼리에 대한 컴파일을 수행하고 입력값을 나중에 넣는 방식이다. 실행 전까지의 과정을 최초로 1번만 수행 후 나중에 미리 컴파일 되어 있는 Prepared Statement를 가져다 사용하는 방식이다.
1. 사용자의 입력 값이 디비의 파라미터로 들어가기 전 DBMS가 미리 컴파일하여 실행하지 않고 대기한다.
2. 그 후 사용자의 입력 값을 문자열로 인식하게 해 공격 쿼리가 들어가도 사용자의 입력은 의미 없는 단순 문자열이라 전체 쿼리문도 공격자의 의도대로 작동하지 않는다.
### 3. Error Message 노출 금지
- 공격자가 SQL Injection을 하기 위해선 디비의 정보인 테이블명과 컬럼명이 필요하다.
- 디비 오류 발생 시 따로 처리하지 않는다면 오류가 발생한 쿼리문과 함께 오류에 관한 내용을 반환한다. 여기서 테이블명 및 컬럼명, 쿼리문이 노출될 수 있어 디비에 대한 오류 발생 시 사용자에게 보여줄 무언가를 띄어야 한다.
