## C# 서버에서 MSSQL 저장 프로시저(Stored Procedure) 사용 예제
C# 프로젝트에서 MSSQL에 정의된 저장 프로시저를 어떤 식으로 사용하는지 설명함.
TODO: 아직 테스트를 안해봤으므로 테스트 필요함.

---
### 저장 프로시저 생성
```
CREATE PROCEDURE usp_AddUserExp
    @UserId INT,
    @Exp INT
AS
BEGIN
    SET NOCOUNT ON;

    UPDATE Users
    SET Exp = Exp + @Exp
    WHERE Id = @UserId;

    SELECT Id, Name, Exp FROM Users WHERE Id = @UserId;
END
```

### C# DTO
```
public class UserDto
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Exp { get; set; }
}
```

### C# 호출 코드
```
public async Task<UserDto> AddUserExpAsync(int userId, int exp)
{
    using var connection = new SqlConnection(_connectionString);

    var parameters = new DynamicParameters();
    parameters.Add("@UserId", userId);
    parameters.Add("@Exp", exp);

    var result = await connection.QueryFirstOrDefaultAsync<UserDto>(
        "usp_AddUserExp",
        parameters,
        commandType: CommandType.StoredProcedure
    );

    return result;
}
```