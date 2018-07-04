# 인증 토큰 확인 및 만료

토큰은 리소스 서버에 리소스를 요청하기 위해 인가 서버로부터 발급받은 값입니다.

### 액세스 토큰 발행

액세스 토큰 발행은 다음과 같은 스크립트를 통해 가능합니다.

**issue\_token.sh**

```bash
#!/bin/bash

CLIENT_ID=$1
CLIENT_SECRET=$2
AUTH_CODE=$3
SIGNON_ENDPOINT="localhost:8080"
REDIR_ENDPOINT="http://localhost:8888"

RESULT=$(curl -is "$CLIENT_ID:$CLIENT_SECRET@${SIGNON_ENDPOINT}/oauth/token" --data "grant_type=authorization_code&\
redirect_uri=$REDIR_ENDPOINT&\
code=${AUTH_CODE}")

export ACCESS_TOKEN="$(echo "$RESULT" | grep -o '{"access_token":"[^"]*' | grep -o '[^"]*$')"
export REFRESH_TOKEN="$(echo "$RESULT" | grep -o '"refresh_token":"[^"]*' | grep -o '[^"]*$')"

echo -e "access token : ${ACCESS_TOKEN}\n"
echo -e "refresh token : ${REFRESH_TOKEN}\n"
```

스크립트를 실행하는 명령어는 다음과 같습니다.

```text
$ ./issue_token.sh ${CLIENT_ID} ${CLIENT_SECRET} ${AUTH_CODE}
```

만약, 다음과 같은 문구가 서버창에 출력된다면 유효하지 않은 인가 코드를 입력하여 오류가 발생한 것 입니다.

```text
20:59 INFO  o.s.s.o.p.e.TokenEndpoint - Handling error: InvalidGrantException, Invalid authorization code: ${AUTH_CODE}
```

### 액세스 토큰 정보 조회

발급받은 액세스 토큰에 관한 정보는 다음의 명령어를 통해 확인할 수 있습니다.

```text
$ coinstack-signon token check ${ACCESS_TOKEN}
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

해당 토큰이 무효한 경우 다음과 같은 결과를 확인할 수 있습니다.

```text
The token not found.
```

## 리프레시 토큰으로 재발행

발급받은 액세스 토큰과 함께 제공되어지는 리프레시 토큰을 이용하여 만료되거나 만료되기 전인 액세스 토큰을 다음의 명령어를 통해 재발행하실 수 있습니다.

**issue\_refresh.sh**

```bash
#!/bin/bash

CLIENT_ID=$1
CLIENT_SECRET=$2
REFRESH_TOKEN=$3
SIGNON_ENDPOINT="localhost:8080"

REFRESH_REQUEST="$(curl -is "$CLIENT_ID:$CLIENT_SECRET@${SIGNON_ENDPOINT}/oauth/token" --data "grant_type=refresh_token&\
refresh_token=$REFRESH_TOKEN")"

export ACCESS_TOKEN="$(echo "$REFRESH_REQUEST" | grep -o '{"access_token":"[^"]*' | grep -o '[^"]*$')"
export REFRESH_TOKEN="$(echo "$REFRESH_REQUEST" | grep -o '"refresh_token":"[^"]*' | grep -o '[^"]*$')"

echo -e "Refresh request :\n ${REFRESH_REQUEST}\n"
echo -e "Access token : ${ACCESS_TOKEN}\n"
echo -e "Refresh token : ${REFRESH_TOKEN}\n"
```

스크립트를 실행하는 명령어는 다음과 같습니다.

```text
$ ./issue_refresh.sh ${CLIENT_ID} ${CLIENT_SECRET} ${REFRESH_TOKEN}
```

만약, 다음과 같은 오류가 출력된다면 유효하지 않은 리프레시 토큰을 입력하여 오류가 발생한 것 입니다.

```text
21:00 INFO  o.s.s.o.p.e.TokenEndpoint - Handling error: InvalidGrantException, Invalid refresh token: ${REFRESH_TOKEN}
```

### 액세스 토큰 무효화

발급받은 토큰을 명시적으로 무효화하는 것은 다음의 명령어를 통해 할 수 있습니다.

```text
$ coinstack-signon token remove \
    --privatekey ${ADMIN_PRIVATEKEY} \
    ${ACCESS_TOKEN}
```

발급받은 액세스 토큰을 무효화 하게 되면 같이 발급받은 리프레시 토큰도 무효화 되게 됩니다.

