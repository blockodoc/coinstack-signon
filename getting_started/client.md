# 클라이언트

OAuth 2.0 서비스를 사용하기 위해서는 클라이언트를 등록해야 합니다. 이 클라이언트는 Coinstack SignOn 서버의 인증 서비스를 사용할 수 있습니다. 자세한 내용은 [OAuth 2.0](../overview/oauth_2.0/)을 참조하시기 바랍니다.

### 관리자

클라이언트를 등록하기 위해서는 관리자의 개인키가 필요합니다. 관리자가 아니면 클라이언트를 등록, 삭제할 수 없습니다.

### 클라이언트 등록

클라이언트는 CLI\(Command Line Interface\)를 이용하여 등록할 수 있습니다.

클라이언트를 등록하는 명령은 다음과 같으며, 클라이언트 정보는 JSON 형태의 표준 입력으로 전달합니다.

#### 사용자 입력으로 등록하기

사용자 입력을 통한 클라이언트 등록은 다음과 같은 명령으로 가능합니다.

```text
$ coinstack-signon client create \
    --privatekey ${ADMIN_PRIVATEKEY} <<EOF
{
  "clientId": "${CLIENT_ID}",
  "clientSecret": "${CLIENT_SECRET}",
  "authorizedGrantTypes": ["${GRANT_TYPE}", ...],
  "scopes": ["${SCOPES}", ...],
  "registeredRedirectUris": ["${REDIRECT_URI}", ...],
  "authorities": ["${AUTHORITY}", ...],
  "autoApproves": ["${SCOPE}", ...]
  "resourceIds": ["${RESOURCE_ID}", ...],
  "accessTokenValidity": "${ACCESS_TOKEN_VALIDITY}",
  "refreshTokenValidity": "${REFRESH_TOKEN_VALIDITY}",
  "description": "${DESCRIPTION}",
  "additionalInformation": {"${ADDITIONAL_INFO_KEY}":"${ADDITIONAL_INFO_VALUE}"}
}
EOF
```

#### 파일로부터 등록하기

파일을 통한 클라이언트 등록은 다음 절차에 따라 진행합니다.

* 클라이언트 정보 파일인 ${CLIENT\_INFO\_FILE}을 생성하여 등록할 클라이언트 정보를 저장합니다.

```text
[{
  "clientId": "${CLIENT_ID}",
  "clientSecret": "${CLIENT_SECRET}",
  "authorizedGrantTypes": ["${GRANT_TYPE}", ...],
  "scopes": ["${SCOPES}", ...],
  "registeredRedirectUris": ["${REDIRECT_URI}", ...],
  "authorities": ["${AUTHORITY}", ...],
  "autoApproves": ["${SCOPE}", ...]
  "resourceIds": ["${RESOURCE_ID}", ...],
  "accessTokenValidity": "${ACCESS_TOKEN_VALIDITY}",
  "refreshTokenValidity": "${REFRESH_TOKEN_VALIDITY}",
  "description": "${DESCRIPTION}",
  "additionalInformation": {"${ADDITIONAL_INFO_KEY}":"${ADDITIONAL_INFO_VALUE}"}
}, ...]
```

* 클라이언트 정보 파일을 통해 클라이언트 정보를 등록합니다.

```text
$ coinstack-signon client create \
    --privatekey ${ADMIN_PRIVATEKEY} \
    --file ${CLIENT_INFO_FILE}
```

#### 클라이언트 속성

| 이름 | 설명 | 필수유무 | 예제 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| clientId | 클라이언트를 구분하는 인식값, 최대 영문 20자 | 필수 | "clientId": "trusted" |
| clientSecret | 클라이언트 자격 증명을 위한 문자열, 패스워드 |  | "clientSecret": "secret" |
| authorizedGrantTypes | 인가 방법 |  | "authorizedGrantTypes":\["authorization\_code"\] |
| scopes | 접근 범위 |  | "scopes": \["read", "write"\] |
| registeredRedirectUris | 인증 후 화면을 전환할 대상 URI |  | "registeredRedirectUris": \["[http://resource.blocko.io:9000](http://resource.blocko.io:9000/)"\] |
| authorities | 인증 후 얻게 되는 권한 |  | "authorities": \["USER","TRUSTED\_CLIENT"\] |
| autoApproves | 자동 인가 여부, scopes에종속 |  | "autoApproves": \["read", "write"\] |
| resourceIds | 접근할 자원 |  | "resourceIds": \["blocko"\] |
| accessTokenValidity | Access token 유효 시간,단위는 초 |  | "accessTokenValidity": "600" |
| refreshTokenValidity | Refresh token 유효 시간,단위는 초 |  | "refreshTokenValidity": "600" |
| description | 클라이언트에 대한 설명 |  | "description": "This is a client" |
| additionalInformation | 시스템별 추가 정보 |  | "additionalInformation" :{"OS":"CentOS"} |

### 클라이언트 조회

등록된 클라이언트는 단일/목록으로 조회할 수 있으며, 다음 명령을 통해 확인할 수 있습니다.

#### 클라이언트 단일 조회

```text
$ coinstack-signon client check ${CLIENT_ID}
```

조회가 완료되면 아래와 같은 문구가 나옵니다.

```text
AccessTokenValidity       ${ACCESS_TOKEN_VALIDITY}
AdditionalInformation     {${ADDITIONAL_INFO_KEY}=${ADDITIONAL_INFO_VALUE}}
Authorities               ${AUTHORITY}
                          .
                          .
                          .
AuthorizedGrantTypes      ${GRANT_TYPE}
                          .
                          .
                          .
AutoApproves              ${SCOPE}
                          .
                          .
                          .
ClientId                  ${CLIENT_ID}
ClientSecret              ${CLIENT_SECRET}
Description               ${DESCRIPTION}
RefreshTokenValidity      ${REFRESH_TOKEN_VALIDITY}
RegisteredRedirectUris    ${REDIRECT_URI}
                          .
                          .
                          .
ResourceIds               ${RESOURCE_ID}
                          .
                          .
                          .
Scopes                    ${SCOPE}
                          .
                          .
                          .
```

※ ClientSecret은 SHA256으로 암호화되어 유출되지 않습니다.

조회할 수 없는 클라이언트를 확인했다면 아래와 같은 문구가 나옵니다.

```text
The client not found.
```

#### 클라이언트 목록 조회

등록된 클라이언트 목록은 다음 명령을 통해 확인할 수 있습니다.

```text
$ coinstack-signon client list \
    --privatekey ${ADMIN_PRIVATEKEY}
```

조회가 완료되면 다음과 같은 문구가 나옵니다.

```text
Client ID            Description
${CLIENT_ID}         ${DESCRIPTION}
.                    .
.                    .
.                    .
```

※ ${ }가 20자가 넘어갈 경우 ... 으로 처리합니다.

등록한 클라이언트가 정상적으로 조회되지 않는다면 다음을 의심해 볼 수 있습니다.

#### 클라이언트 등록 실패

클라이언트 등록 실행 중 어떤 문제\(버그나 잘못된 사용법\)로 인해 클라이언트 등록에 실패할 수 있습니다. 보통의 경우, 로그 레벨을 지정해서 동작에 관련된 로그를 남기고 이를 통해 문제를 확인할 수 있습니다.

로그 레벨을 조정하기 위한 파라미터는 --log입니다.

로그 레벨을 trace로 바뀌기 위한 명령은 두 가지 방식이 있습니다.

**사용자 입력으로 로그 확인**

```text
$ coinstack-signon client create \
    --log trace \
    --privatekey ${ADMIN_PRIVATEKEY} <<EOF
{
  "clientId": "${CLIENT_ID}",
  "clientSecret": "${CLIENT_SECRET}",
  "authorizedGrantTypes": ["${GRANT_TYPE}", ...],
  "scopes": ["${SCOPES}", ...],
  "registeredRedirectUris": ["${REDIRECT_URI}", ...],
  "authorities": ["${AUTHORITY}", ...],
  "autoApproves": ["${SCOPE}", ...]
  "resourceIds": ["${RESOURCE_ID}", ...],
  "accessTokenValidity": "${ACCESS_TOKEN_VALIDITY}",
  "refreshTokenValidity": "${REFRESH_TOKEN_VALIDITY}",
  "description": "${DESCRIPTION}",
  "additionalInformation": {"${ADDITIONAL_INFO_KEY}":"${ADDITIONAL_INFO_VALUE}"}
}
```

**파일로부터 로그 확인**

```text
$ cat ${CLIENT_INFO_FILE} | \
  coinstack-signon client create \
    --log trace \
    --privatekey ${ADMIN_PRIVATEKEY}
```

#### 블록에 트랜잭션 미포함

Coinstack Node의 마이닝 주기와 같은 설정 때문에 즉각적으로 블록이 생성되지 않을 수 있습니다. 따라서, 노드의로그나 설정을 통해 확인해 보는 것이 좋습니다. Node의 설정은 본 문서에서 다루지 않습니다.

### 클라이언트 삭제

사용하지 않는 클라이언트는 삭제하는 것이 더 높은 보안 수준을 유지합니다. 기존의 클라이언트를 삭제하기 위해서는 클라이언트의 ID를 알고 있어야 합니다. 필요하다면 클라이언트를 조회하도록 합니다.

클라이언트를 삭제하기 위한 명령은 다음과 같습니다.

```text
$ coinstack-signon client remove \
    --privatekey ${ADMIN_PRIVATEKEY} \
    ${CLIENT_ID}
```

