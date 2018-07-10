# Postman 테스트

Coinstack SignOn의 OAuth 2.0 인증 서비스 흐름을 확인하는 Postman 테스트를 제공합니다.

테스트 예제의 Postman 버전은 6.0.10입니다.

### OAuth 2.0 설정

Postman의 Authorization TYPE을 OAuth 2.0으로 다음 화면처럼 설정한 뒤,

액세스 토큰을 발급받기 위해 Get New Access Token 버튼을 클릭합니다.

![](../../.gitbook/assets/appendix/test/postman_test1.png)

### 액세스 토큰 발급

액세스 토큰을 발급받기 위해 등록된 클라이언트 정보에 따라 양식을 다음 화면처럼 설정한 뒤, Request Token 버튼을 클릭합니다

![](../../.gitbook/assets/appendix/test/postman_test2.png)

Request Token 버튼을 통해 Token을 요청하면 로그인 페이지로 리다이렉트됩니다.

![](../../.gitbook/assets/appendix/test/postman_test3.png)

이후 등록된 사용자의 ID/Password로 로그인, 사용자 허가의 과정을 수행합니다.

![](../../.gitbook/assets/appendix/test/postman_test4.png)

이후 다음 화면처럼 액세스 토큰에 대한 정보를 확인할 수 있습니다.

![](../../.gitbook/assets/appendix/test/postman_test5.png)

액세스 토큰의 유효성 검사를 위해 Use Token 버튼을 클릭합니다.

![](../../.gitbook/assets/appendix/test/postman_test6.png)

### 액세스 토큰 유효성 검사

#### check\_token.json

액세스 토큰 유효성 검사를 손쉽게 하기 위해 다음 파일을 **Import**하시기 바랍니다.

```text
{
    "id": "b4b1a30f-e756-949c-b665-13df6af2276c",
    "name": "oauth",
    "description": "",
    "order": [
        "ff1fda8d-7c1d-e954-bb35-309b87795198"
    ],
    "folders_order": [],
    "folders": [],
    "owner": "3456127",
    "hasRequests": true,
    "requests": [
        {
            "id": "ff1fda8d-7c1d-e954-bb35-309b87795198",
            "headers": "Content-Type: application/x-www-form-urlencoded\nAuthorization: Basic dHJ1c3RlZDpzZWNyZXQ=\n",
            "headerData": [
                {
                    "key": "Content-Type",
                    "value": "application/x-www-form-urlencoded",
                    "description": "",
                    "enabled": true
                },
                {
                    "key": "Authorization",
                    "value": "Basic dHJ1c3RlZDpzZWNyZXQ=",
                    "description": "",
                    "enabled": true
                }
            ],
            "url": "localhost:8080/oauth/check_token",
            "queryParams": [],
            "preRequestScript": null,
            "pathVariables": {},
            "pathVariableData": [],
            "method": "POST",
            "data": [
                {
                    "key": "token",
                    "value": "",
                    "description": "",
                    "type": "text",
                    "enabled": true
                }
            ],
            "dataMode": "urlencoded",
            "tests": null,
            "currentHelper": "normal",
            "helperAttributes": {},
            "time": 1524212770984,
            "name": "check_token",
            "description": "",
            "collectionId": "b4b1a30f-e756-949c-b665-13df6af2276c",
            "responses": []
        }
    ]
}
```

발급받은 액세스 토큰의 유효성을 검사하기 위해서 다음 화면처럼 check\_token URL을 입력합니다.

그리고 Headers 탭에서 Content-Type 에 application/x-www-form-urlencoded로 데이터 포맷을 정합니다.

이후, Authorization에 ${CLIENT\_ID}:${CLIENT\_SECRET}를 **Base64로 Encoding**한 뒤,

Authorization의 Value에 **Basic ${ENCODED\_TEXT}**를 다음 화면처럼 입력합니다.

#### Base64 Encoding/Decoding 방법

**Encoding**

```text
echo -n '${CLIENT_ID}:${CLIENT_SECRET}' | base64
```

**Decoding**

```text
echo -n '${ENCODED_TEXT}' | base64 -d | xargs
```

![](../../.gitbook/assets/appendix/test/postman_test7.png)

Body 탭에서 x-www-form-urlencoded를 데이터 타입으로 지정한 후, 다음과 같이 입력합니다.

token의 value에 입력한 값은 발급받은 액세스 토큰 값입니다. Send 버튼을 클릭합니다.

![](../../.gitbook/assets/appendix/test/postman_test8.png)

이후 다음과 같이 액세스 토큰의 유효성을 확인할 수 있습니다.

![](../../.gitbook/assets/appendix/test/postman_test9.png)

