# 인가 코드 확인

인가 코드는 액세스 토큰을 발급받기 위해 서버로부터 발급받은 값입니다.

## 인가 코드 발행

인가 코드 발행은 다음과 같은 스크립트를 통해 가능합니다.

**issue\_code.sh**

```bash
#!/bin/bash

CLIENT_ID=$1
CLIENT_SECRET=$2
USER_NAME=$3
PASSWORD=$4
SIGNON_ENDPOINT="http://localhost:8080"
REDIR_ENDPOINT="http://localhost:8888"

AUTH_URL="${SIGNON_ENDPOINT}/oauth/authorize?response_type=code&\
scope=read&\
grant_type=authorization_code&\
client_id=${CLIENT_ID}&\
secret=${CLIENT_SECRET}&\
redirect_uri=${REDIR_ENDPOINT}"
OPTIONS=""
RET="Location"

FIRST_TRY="$(curl "$OPTIONS" -is "$AUTH_URL")"
JSESSIONID_BEFORE_LOGIN="$(echo "$FIRST_TRY" | grep "JSESSIONID" | awk '{print $2}' | cut -d ';' -f 1)"
LOGIN_PAGE="$(curl "$OPTIONS" -is "${SIGNON_ENDPOINT}/login" -H "Cookie: $JSESSIONID_BEFORE_LOGIN")"

LOGIN_RESULT="$(curl -is "${SIGNON_ENDPOINT}/login" \
  -H "Cookie: $JSESSIONID_BEFORE_LOGIN" \
  --data "username=${USER_NAME}&password=${PASSWORD}&submit=Login")"
JSESSIONID_AFTER_LOGIN="$(echo "$LOGIN_RESULT" | grep "JSESSIONID" | awk '{print $2}' | cut -d ';' -f 1)"

AUTH_REQUEST_RESULT="$( curl "$OPTIONS" -is "$AUTH_URL" -H "Cookie: $JSESSIONID_AFTER_LOGIN" )"
if [ -z "$(echo "$AUTH_REQUEST_RESULT" | grep 'OAuth Approval')" ]; then
  export FINAL_AUTH_RESULT="$(echo "$AUTH_REQUEST_RESULT" | grep "$RET" )"
else
  AUTHORIZE_RESULT="$(curl "$OPTIONS" -is "${SIGNON_ENDPOINT}/oauth/authorize" \
    -H "Cookie: $JSESSIONID_AFTER_LOGIN" \
    --data "user_oauth_approval=true&authorize=Authorize" )"
  export FINAL_AUTH_RESULT="$(echo "$AUTHORIZE_RESULT" | grep "$RET" )"
fi

AUTH_CODE="$(echo "$FINAL_AUTH_RESULT" |  grep -o "code=[^ ]*"  | cut -d"=" -f 2)"
AUTH_CODE=${AUTH_CODE:0:10}
export AUTH_CODE
echo "${AUTH_CODE}"
```

스크립트를 실행하는 명령어는 다음과 같습니다.

```text
$ ./issue_code.sh ${CLIENT_ID} ${CLIENT_SECRET} ${USER_NAME} ${PASSWORD}
```

명령어 실행 후 인가 코드가 출력되지 않았다면, 다시 한번 입력한 매개변수에 대하여 확인하고 재실행합니다.

### 인가 코드 정보 조회

발급받은 인가 코드에 관한 정보는 다음의 명령어를 통해 확인할 수 있습니다.

```text
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

