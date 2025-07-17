## 저장 프로시저(Stored Procedure)

저장 프로시저는 이러한 방식이 가능하도록하는 각 DBMS 에서 제공하는 프로그래밍 기능암
대부분의 DBMS에서 지원함. 본 문서에서는 MSSQL의 저장 프로시저에 대해 서술.

 

저장 프로시저는 쿼리문들의 집합으로, 어떤 동작을 여러쿼리를 거쳐서 일괄적으로 처리할 때 사용함.

---
### 저장 프로시저의 형식

```
CREATE { PROC | PROCEDURE} [schema_name.]procedure_name [; number ]
	[ { @parameter [type_schema_name. ] data_type } [ VARYING] [ = default ] [ OUT | OUTPUT ] [READONLY]] 
	[  ,...n ]
[ WITH <procedure_option> [,...n ]]
[ FOR REPLICATION ]
AS { [BEGIN ] sql_statement [;] [ ...n ] [ END] }
[;]
```

#### 예시 1: 입력 변수가 1개임임
```
CREATE PROCEDURE select_user_by_name
	@userName NVARCHAR(10)
AS
	SELECT * FROM userTbl WHERE name = @userName;

GO
EXEC select_user_by_name '김서경';
```


#### 예시 2: 입력 변수가 2개임
```
CREATE PROCEDURE select_user_by_name_and_age
	@userName NVARCHAR(2),
	@userAge INT
AS
	SELECT * FROM userTbl WHERE name = @userName AND age = @userAge;
GO
EXEC select_user_by_first_name_and_height '김서경', 20;
```

#### 예시 3: 출력 파라미터 설정
```
CREATE PROCEDURE output_table_identity
	@txtValue NCHAR(10),
	@outputValue INT OUTPUT
AS
	INSERT INTO outputTestTbl Values(@txtValue);
	SELECT @outputValue = IDENT_CURRENT('outputTestTbl'); -- 테이블의 현재 identity 값
GO

CREATE TABLE outputTestTbl ( id INT IDENTITY, txt NCHAR(10));
GO
```
실행
```
DECLARE @myValue INT;
EXEC output_table_identity '텍스트', @myValue OUTPUT;
PRINT 'Current input Id value : ' +  CAST(@myValue AS CHAR(5));
```

---
### 참고 문서
- 개념: https://devkingdom.tistory.com/323
- 사용법: https://devkingdom.tistory.com/324