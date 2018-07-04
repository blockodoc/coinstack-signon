# 서버 환경 설정

본 장에서는 Coinstack SignOn의 환경 설정에 관한 전반적인 내용을 설명합니다.

Coinstack SignOn 서버를 설정하는 방법은 다음과 같습니다.

### Coinstack Node 연동 설정

Coinstack SignOn는 Coinstack Node에 데이터를 저장하고, 읽어서 인증 및 권한 관련 기능을 수행합니다. 사용자는 Coinstack SignOn 서버에서 사용할 Coinstack Node와 관련된 정보를 설정해야 합니다.

#### 대화형 명령을 통해 설정하기

Coinstack SignOn는 사용자가 손쉽게 Coinstack Node와 연동할 수 있도록 대화형 명령 스크립트를 제공합니다.대화형\(interactive\) 명령을 통한 설정은 다음과 같은 순서로 진행할 수 있습니다.

**명령 실행**

다음 명령을 수행하면 대화형 모드를 시작합니다.

```text
$ coinstack-signon server configure
```

**Coinstack Node의 Endpoint 입력**

```text
Input the coinstack node endpoint [http://localhost:3000]
>
```

**서버의 포트 입력**

```text
Input the server port [8080]
>
```

**관리자의 주소 입력**

```text
Input the administrator's address [1KkM2NLZEK9SL4sChJVgg45k3godj7Tnix]
>
```

**서버의 개인키 생성**

서버의 개인키는 서버의 성능과 관련되어 있습니다. 병렬 처리를 위해 여러 개의 키를 사용하기 때문에 성능이 높은 서버일수록 더 많은 개인키를 생성해야 합니다.

```text
Input the number of private key [1]
>
```

서버의 개인키는 다음과 같은 명령어로도 생성할 수 있습니다.

```text
$ coinstack-signon key create
```

구체적인 인자는 help key 명령어를 통해 확인하실 수 있습니다.

```text
$ coinstack-signon help key
  SYNOPSIS
      coinstack-signon key create [[--log|-l] <loglevel>] [--number|-n] [--address|-a]

  DESCRIPTION
      Create and print out a private key randomly.

  OPTIONS
      --address, -a
        Print out not only private key but also address. You can
        separate them with whitespace.

      --number, -n
        The number of private keys to generate.

      --log, -l
        To known something wrong, turn on logger. You can
        specify log level as next:
        error, warn, info, debug, info, custom
        The log level specify log configuration file in conf
        directory.(ex logback-cli-error.xml) You can customize
        any level as your needs.

  EXAMPLES
      $ coinstack-signon key create -n 20 --address
```

| 인자 | 설명 |
| --- | --- | --- | --- |
| --address, -a | 어드레스 출력 |
| --number, -n | 개인키 개수 |
| --log, -l | 로그 |

* **설정 파일 생성**

```text
Process to generate the configuration file? [y/n]  y
```

※ 관리자의 주소와 서버의 개인키를 따로 설정하는 이유는[ Coinstack SignOn](../../undefined-1/coinstack-signon/)을 참조합니다.

#### 일괄 처리 명령을 통해 설정하기

Coinstack SignOn는 사용자 입력 없이 서버 설정을 할 수 있는 명령 스크립트를 제공합니다. 일괄 처리\(batch\) 처리 명령을 통한 설정은 다음과 같은 순서로 진행할 수 있습니다.

**설정을 위한 값 지정**

다음 명령을 수행하면 디폴트 설정값을 변경할 수 있습니다.

```text
$ export NODE_ENDPOINT=${NODE_ENDPOINT}
$ export ADMIN_ADDRESS=${ADMIN_ADDRESS}
$ export SIGNON_SERVER_PORT=${SIGNON_SERVER_PORT}
$ export N_SERVER_PRIVATEKEYS=${N_SERVER_PRIVATEKEYS}
```

설정 환경 변수는 다음과 같습니다.

| 환경 변수명 | 의미 |
| --- | --- | --- | --- | --- |
| NODE\_ENDPOINT | 연동할 Coinstack Node의 Endpoint |
| ADMIN\_ADDRESS | Coinstack SignOn 관리자의 주소 |
| SIGNON\_SERVER\_PORT | Coinstack SignOn 서버의 포트 |
| N\_SERVER\_PRIVATEKEYS | Coinstack SignOn 서버의 개인키의 개수 |

**설정 파일 생성**

다음 명령을 수행하면 설정 파일이 자동 생성됩니다.

```text
$ coinstack-signon server configure --batch
```

#### 설정 파일을 직접 수정하기

사용자는 설정 파일을 직접 수정해서 설정을 적용할 수 있습니다. 설정은${INSTALL\_PATH}/conf/coinstack.yaml에 저장됩니다. 설정 파일은 yaml 형식을 따르며, 각 설정 항목에 관련된 내용은 다음과 같습니다.

| 설정 항목 | 의미 |
| --- | --- | --- | --- | --- |
| coinstack.endpoint | 연동할 Coinstack Node의 Endpoint |
| signon.admin.address | Coinstack SignOn 관리자의 주소 |
| signon.server.port | Coinstack SignOn 서버의 포트 |
| signon.server.privatekeys | Coinstack SignOn 서버의 개인키 |

**coinstack.yaml**

```yaml
coinstack:
  endpoint: http://localhost:3000

signon:
  admin:
    address: 1KkM2NLZEK9SL4sChJVgg45k3godj7Tnix
  server:
    port: 8080
    privatekeys: |
      ${SERVER_PRIVATEKEY}
      .
      .
      .
```

