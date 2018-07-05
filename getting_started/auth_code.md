# 인가 코드 확인

인가 코드는 액세스 토큰을 발급받기 위해 서버로부터 발급받은 값입니다.

### 인가 코드 발행

인가 코드는 다음의 명령을 통해 발행 가능합니다.

```bash
$ coinstack-signon code create --client ${CLIENT_ID} --user ${USER_NAME} --password ${PASSWORD} --endpoint ${ENDPOINT}
```

명령어 실행 후 다음과 같이 출력된다면, 클라이언트 아이디 또는 사용자 정보가 유효하지 않아 발생한 오류입니다.

```text
Check your clientId or username, password.
```

만약, Coinstack SignOn 서버가 꺼져있다면 다음과 같은 결과를 확인할 수 있습니다.

```bash
Connection refused. Check server endpoint.
```

#### 인가 코드 정보 조회

발급받은 인가 코드에 관한 정보는 다음의 명령어를 통해 확인할 수 있습니다.

```bash
$ coinstack-signon code check ${AUTHORIZATION_CODE}
```

조회가 잘 되면 다음과 같은 결과를 확인할 수 있습니다.

```text
Authentication
        AccessTokenValue        
        Authorities               ${AUTHORITY}
                                  .
                                  .
                                  .
        ClientId                  ${CLIENT_ID}
        Details   
        RedirectUri               ${REDIRECT_URI}
        RequestParameters         ${PARAMETERS}
        ResourceIds   
        RolesOfAuthorities   
        Scope                     ${SCOPE}
                                  .
                                  .
                                  .
        UserPrinciple             ${USER_PRINCIPLE}

Value                      ${AUTH_CODE}
```

유효하지 않은 인가 코드 정보를 조회하면 다음과 같은 결과를 확인할 수 있습니다.

```text
The code not found.
```

