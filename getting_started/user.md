# 사용자

OAuth 2.0 서비스를 사용하기 위해서는 사용자를 등록해야 합니다. 이 사용자는 Coinstack SignOn 서버의 인증서비스를 사용할 수 있습니다.

자세한 내용은 [OAuth 2.0](../overview/oauth_2.0/)을 참조하시기 바랍니다.

### 관리자

사용자를 등록하기 위해서는 관리자의 개인키가 필요합니다. 관리자가 아니면 사용자를 등록, 삭제할 수 없습니다.

### 사용자 등록

사용자는 CLI\(Command Line Interface\)를 이용하여 등록할 수 있습니다.

사용자를 등록하는 명령은 다음과 같습니다.

#### 사용자 입력으로 등록하기

사용자 입력을 통한 사용자 등록은 다음과 같은 명령으로 가능합니다.

```bash
$ coinstack-signon user create \
    --privatekey ${ADMIN_PRIVATEKEY} <<EOF
{
  "username": "${USER_NAME}",
  "password": "${PASSWORD}",
  "authorities": ["${AUTHORITY}", ...]
}
EOF
```

#### 파일로부터 등록하기

파일을 통한 사용자 등록은 다음 절차에 따라 진행합니다.

* 사용자 정보 파일인 ${USER\_INFO\_FILE}을 생성하여 등록할 사용자 정보를 저장합니다.

```bash
[{
  "username": "${USER_NAME}",
  "password": "${PASSWORD}",
  "authorities": ["${AUTHORITY}", ...]
}, ...]
```

* 사용자 정보 파일을 통해 사용자 정보를 등록합니다.

```bash
$ coinstack-signon user create \
    --privatekey ${ADMIN_PRIVATEKEY} \
    --file ${USER_INFO_FILE}
```

#### 사용자 속성

| 이름 | 설명 | 필수 유무 | 예제 |
| --- | --- | --- | --- |
| username | 사용자 아이디 | 필수 | "username":"user" |
| password | 사용자 비밀번호 | 필수 | "password":"password" |
| authorities | 인증 후 얻게 되는 권한들 | 필수 | "authorities":\["USER", "TRUSTED\_CLIENT"\] |

### 사용자 조회

등록된 사용자는 다음 명령을 통해 확인할 수 있습니다.

```bash
$ coinstack-signon user check ${USER_NAME}
```

조회가 완료되면 아래와 같은 문구가 나옵니다.

```bash
Authorities               ${AUTHORITY}
                          .
                          .
                          .
Password                  ${PASSWORD}
Username                  ${USER_NAME}
```

※ Password는 SHA256으로 암호화되어 유출되지 않습니다.

조회할 수 없는 사용자를 확인했다면 아래와 같은 문구가 나옵니다.

```text
The user not found.
```

등록한 사용자가 정상적으로 조회되지 않는다면 다음을 의심해볼 수 있습니다.

#### 사용자 등록 실패

사용자 등록 실행 중 어떤 문제\(버그나 잘못된 사용법\)로 인해 사용자 등록에 실패할 수 있습니다. 보통의 경우, 로그레벨을 지정해서 동작에 관련된 로그를 남기고 이를 통해 문제를 확인할 수 있습니다.

로그 레벨을 조정하기 위한 파라미터는 --log입니다.

로그 레벨을 trace로 바꾸기 위한 명령은 두 가지 방식이 있습니다.

**사용자 입력으로 로그 확인**

```bash
$ coinstack-signon user create \
    --log trace \
    --privatekey ${ADMIN_PRIVATEKEY} <<EOF
{
  "username": "${USER_NAME}",
  "password": "${PASSWORD}",
  "authorities": ["${AUTHORITY}", ...]
}
```

**파일로부터 로그 확인**

```bash
$ cat ${USER_INFO_FILE} | \
  coinstack-signon user create \
    --log trace \
    --privatekey ${ADMIN_PRIVATEKEY}
```

#### 블록에 트랜잭션 미포함

Coinstack Node의 마이닝 주기와 같은 설정 때문에 즉각적으로 블록이 생성되지 않을 수 있습니다. 따라서, 노드의로그나 설정을 통해 확인해보는 것이 좋습니다. Node의 설정은 본 문서에서 다루지 않습니다.

### 사용자 삭제

사용하지 않는 사용자는 삭제하는 것이 더 높은 보안 수준을 유지합니다. 기존의 사용자를 삭제하기 위해서는 사용자의 ID인 ${USER\_NAME}을 알고 있어야 합니다. 필요하다면 사용자를 조회하도록 합니다.

사용자를 삭제하기 위한 명령은 다음과 같습니다.

```bash
$ coinstack-signon user remove \
    --privatekey ${ADMIN_PRIVATEKEY} \
    ${USER_NAME}
```

