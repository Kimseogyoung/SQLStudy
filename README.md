# SQLStudy
각 DB들의 설치 방법. 기본적인 쿼리, 차이점들을 기록함.

1. MSSQL 
2. MYSQL
3. Sqlite

## 1. MSSQL
### 설치 방법
MSSQL Express 2022
https://bigexecution.tistory.com/252 참고

프로그램 설치 : SQL Server Management Studio 21 : https://aka.ms/ssms/21/release/vs_SSMS.exe

1433 포트 오픈이 디폴트로 되어있지 않으므로 추가 설정 필요함 (MSSQL cil 관련 가이드 참고)

## 2. MYSQL
MYSQL WorkBench : https://downloads.mysql.com/archives/installer/
Select Products -> Server, Workbench, Shell 선택
Root 비밀번호 설정

## 3. Sqlite
별도 설치가 필요없으나, 툴 다운로드함.
https://www.sqlite.org/download.html 에 접속하여 sqlite-tools-linux-x64-3500200.zip 다운로드.

압축 해제 후 sqlite.exe 실행 (명령 프롬프트)

DB Browser for Sqlite : https://sqlitebrowser.org/

---
## Tool
1. MSSQL - SSMS(Sql Server Management Studio)
2. MYSQL - Mysql Workbench
3. MYSQL - Heidisql : SSH 연결에 유용
4. 통합 - DBeaver : 여러 종류의 DB 연결