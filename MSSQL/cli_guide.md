# MSSQL Command Line 사용 가이드 (WSL 기반)

이 문서는 Linux(Wsl2 포함)에서 MSSQL 서버에 연결하고 사용하는 전 과정을 정리한 문서입니다.

---

## 1. ODBC Driver 및 sqlcmd 설치 (Ubuntu 기준, Windows는 MSSQL 설치 시 sqlcmd 포함함)
랑크 참고해서 진행행: https://learn.microsoft.com/ko-kr/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver17&tabs=ubuntu2004


---

## 2. sqlcmd 접속 명령어

```bash
sqlcmd -S 127.0.0.1,1433 -U sa -P "<비밀번호>" -N -C
```
(wsl에서 접속하는 경우 1433 포트포워딩 설정 및 로컬 컴퓨터의 ip로 지정 필요)

| 옵션   | 설명                                         |
| ---- | ------------------------------------------ |
| `-S` | 서버 주소 (IP, 포트 지정)                          |
| `-U` | 사용자명 (기본: `sa`)                            |
| `-P` | 비밀번호                                       |
| `-N` | 암호화 사용 (Encrypt=yes)                       |
| `-C` | 서버 인증서 검증 생략 (TrustServerCertificate=true) |

---

## 3. MSSQL 서버 기본 설정 (Windows 측)

### SQL Server Configuration Manager

1. 경로: `SQL Server Network Configuration` → `Protocols for SQLEXPRESS`
2. `TCP/IP` → **사용(Enabled)** → 더블클릭 → **IPAll 섹션** 설정:

   * `TCP Dynamic Ports`: 비움움
   * `TCP Port`: `1433`
3. 설정 후: **SQL Server (SQLEXPRESS) 서비스 재시작**

```powershell
Restart-Service -Name 'MSSQL$SQLEXPRESS'
```

### Windows 방화벽 설정 (방화벽 비활성화 상태일 시 스킵(?))

```powershell
New-NetFirewallRule -DisplayName "SQL Server TCP 1433" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action Allow
```

---

### 4. SQL Server Management Studio (SSMS) 서버 활성화 및 사용자 설정

1. 실행: SSMS → `localhost\SQLEXPRESS` 로그인 (Windows 인증)
2. 인스턴스 우클릭 → **속성 → 보안**
   * "SQL Server 및 Windows 인증 모드"로 변경
3. 왼쪽 메뉴 → 보안 → 로그인 → `sa` 계정:
   * **사용 허용** 체크
   * **비밀번호 설정 및 저장**
4. SQL Server 재시작 필요

---

## 5. cli 기본 쿼리 사용법

### 현재 DB 확인

```sql
SELECT DB_NAME();
GO
```

### DB 목록 보기

```sql
SELECT name FROM sys.databases;
GO
```

### DB 선택

```sql
USE <데이터베이스명>;
GO
```

### 테이블 목록 보기

```sql
SELECT name FROM sys.tables;
GO
```

### 테이블 조회

```sql
SELECT TOP 10 * FROM <테이블명>;
GO
```

### 종료

```sql
EXIT
```

---

## 참고 팁

* `GO`는 sqlcmd에서 쿼리 블록을 실행하기 위한 배치 구분자임
* `EXIT`으로 종료할 수 있음
* Windows에서 SQL Server가 `TCP 1433` 포트로 리슨 중이어야 WSL에서 연결됨
* ODBC Driver 18부터는 암호화(`-N`) 및 인증서 신뢰 설정(`-C`) 필수임

---

## 관련 명령 요약

### SQL Server 포트 리슨 확인 (Windows)

```powershell
netstat -ano | findstr :1433
```

### 포트포워딩 (WSL → Windows)

```powershell
netsh interface portproxy add v4tov4 listenaddress=127.0.0.1 listenport=1433 connectaddress=<내 컴퓨터 ip> connectport=1433

이후 wsl에서 sqlcmd 연결 시 
sqlcmd -S <내 컴퓨터 ip>,1433 -U sa -P "<비밀번호>" -N -C
```

### 인증서 오류 발생 시

```bash
sqlcmd -S 127.0.0.1,1433 -U sa -P "<비밀번호>" -N -C
```
