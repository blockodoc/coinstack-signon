# 인증 토큰 확인 및 만료

토큰은 리소스 서버에 리소스를 요청하기 위해 인가 서버로부터 발급받은 값입니다.

#### 액세스 토큰 발행

액세스 토큰은 [인가증명\(Grant\)](../undefined-1/oauth-2.0/grant.md)에서 설명한 4가지 방식을 통해 발급 가능합니다.

```bash
// Authorization code
$ coinstack-signon token create --type code  --client ${CLIENT_ID} --secret ${CLIENT_SECRET}  --endpoint ${ENDPOINT} --redirect ${REDIRECT_URI} --code ${AUTHORIZATION_CODE}

// Implicit
$ coinstack-signon token create --type implicit --user ${USERNAME} --password ${PASSWORD} --client ${CLIENT_ID} --secret ${CLIENT_SECRET}  --endpoint ${ENDPOINT}

// Resource Owner Password Credentials
$ coinstack-signon token create --type password --user ${USERNAME} --password ${PASSWORD} --client ${CLIENT_ID} --secret ${CLIENT_SECRET}  --endpoint ${ENDPOINT}

//Client Credentials
$ coinstack-signon token create --type credentials  --client ${CLIENT_ID} --secret ${CLIENT_SECRET}  --endpoint ${ENDPOINT}
```

명령어 실행 후 다음과 같이 출력된다면, 클라이언트 아이디 또는 사용자 정보가 유효하지 않아 발생한 오류입니다.

```text
Check your clientId or username, password.
```

만약, 다음과 같은 문구가 출력된다면 유효하지 않은 인가 코드를 입력하여 발생한 오류입니다.

```text
The code not found.
```

만약, Coinstack SignOn 서버가 꺼져있다면 다음과 같은 결과를 확인할 수 있습니다.

```text
Connection refused. Check server endpoint.
```

#### 액세스 토큰 정보 조회

발급받은 액세스 토큰에 관한 정보는 다음의 명령어를 통해 확인할 수 있습니다.

```bash
// Direct access to the block chain
$ coinstack-signon token check ${ACCESS_TOKEN}

// Use endpoint
$ coinstack-signon token check --client ${CLIENT_ID} --secret ${CLIENT_SECRET} --endpoint ${ENDPOINT} ${ACCESS_TOKEN}
```

조회가 잘 되면 다음과 같은 결과를 확인할 수 있습니다.

```text
AdditionalInformation    {${ADDITIONAL_INFO_KEY}=${ADDITIONAL_INFO_VALUE}}
Expiration               ${DATE}
RefreshToken             ${REFRESH_TOKEN_VALUE}
Scope                    ${SCOPE}
                         .
                         .
                         .
TokenType                ${TOKEN_TYPE}
Value                    ${ACCESS_TOKEN_VALUE}
```

명령어 실행 후 다음과 같이 출력된다면, 클라이언트 아이디 또는 사용자 정보가 유효하지 않아 발생한 오류입니다.

```text
Check your clientId or username, password.
```

만약, 다음과 같은 문구가 출력된다면 유효하지 않은 토큰을를 입력하여 발생한 오류입니다.

```text
The token not found.
```

만약, Coinstack SignOn 서버가 꺼져있다면 다음과 같은 결과를 확인할 수 있습니다.

```text
Connection refused. Check server endpoint.
```

### 리프레시 토큰으로 재발행

발급받은 액세스 토큰과 함께 제공되어지는 리프레시 토큰을 이용하여 만료되기 전인 액세스 토큰을 다음의 명령어를 통해 재발행할 수 있습니다.

```bash
// Direct access to the block chain
$ coinstack-signon token refresh --privatekey ${ADMIN_PRIVATEKEY} ${REFRESH_TOKEN}

// Use endpoint
$ coinstack-signon token refresh --client ${CLIENT_ID} --secret ${CLIENT_SECRET} --endpoint ${ENDPOINT} ${REFRESH_TOKEN}
```

명령어 실행 후 다음과 같이 출력된다면, 클라이언트 아이디 또는 사용자 정보가 유효하지 않아 발생한 오류입니다.

```text
Check your clientId or username, password.
```

만약, 다음과 같은 문구가 출력된다면 유효하지 않은 토큰을를 입력하여 발생한 오류입니다.

```text
The token not found.
```

만약, Coinstack SignOn 서버가 꺼져있다면 다음과 같은 결과를 확인할 수 있습니다.

```text
Connection refused. Check server endpoint.
```

#### 액세스 토큰 무효화

발급받은 토큰을 명시적으로 무효화하는 것은 다음의 명령어를 통해 할 수 있습니다.

```text
$ coinstack-signon token remove \
    --privatekey ${ADMIN_PRIVATEKEY} \
    ${ACCESS_TOKEN}
```

발급받은 액세스 토큰을 무효화 하게 되면 같이 발급받은 리프레시 토큰도 무효화 되게 됩니다.

