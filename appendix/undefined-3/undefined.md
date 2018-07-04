# 기능 테스트

OAuth 2.0 RFC 스펙을 만족하는지 확인하는 테스트 스크립트를 제공합니다. 테스트 스크립트는 Bash 기반으로 작성되어 있습니다. OAuth 2.0 RFC 스펙은 다음의 URL에서 확인하실 수 있습니다.

[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749)

### 실행

다음과 같이 실행할 수 있습니다.

```text
$ coinstack-function-test
```

실행 시 다음과 같은 화면을 확인하실 수 있습니다. 블록체인에 클라이언트, 사용자가 등록되어 있지 않다면 **최초 한번**은 y를 눌러서 사용자, 클라이언트를 등록해야 합니다.

```text
Do you wanna setup the client, user for function test [y|n]?
>
```

y를 누르면 관리자의 개인키를 입력하는 창이 출력됩니다.

```text
Input the administrator's private key [default : KyQxDqRvUYAKnMHN3PvHwQUwpQvtu5Bz2MDARaNBZiTsdTRAaj5e]
>
```

관리자의 개인키를 입력하고 엔터를 치면 기능 테스트를 위한 클라이언트, 사용자를 생성 후 기능 테스트를 실행합니다. 그냥 엔터를 칠 경우 디폴트 값으로 설정 후 진행합니다.

```text
Do you wanna setup the client, user for function test [y|n]?
>  y
Input the administrator's private key [default : KyQxDqRvUYAKnMHN3PvHwQUwpQvtu5Bz2MDARaNBZiTsdTRAaj5e]
>  
Setup processing..

scripts/2/CH2_1TC1.sh.............................SUCCESS
scripts/2/CH2_3TC1.sh.............................SUCCESS
scripts/2/CH2_3_1TC1.sh...........................SUCCESS

...

scripts/6/CH6_3TC2.sh.............................SUCCESS
scripts/6/CH6_3TC3.sh.............................SUCCESS
scripts/6/CH6_4TC1.sh.............................SUCCESS
success/all : 58 / 58
```

n을 누르면 기능 테스트를 바로 수행합니다.

```text
Do you wanna setup the client, user for function test [y|n]?
>  n
scripts/2/CH2_1TC1.sh.............................SUCCESS
scripts/2/CH2_3TC1.sh.............................SUCCESS
scripts/2/CH2_3_1TC1.sh...........................SUCCESS

...

scripts/6/CH6_3TC2.sh.............................SUCCESS
scripts/6/CH6_3TC3.sh.............................SUCCESS
scripts/6/CH6_4TC1.sh.............................SUCCESS
success/all : 58 / 58
```

만약 클라이언트, 사용자를 등록하지 않고 진행 시에 다음과 같이 29개만 성공합니다.

```text
Do you wanna setup the client, user for function test [y|n]?
> n
scripts/2/CH2_1TC1.sh.............................FAILURE
scripts/2/CH2_3TC1.sh.............................FAILURE
scripts/2/CH2_3_1TC1.sh...........................FAILURE

...

scripts/6/CH6_3TC2.sh.............................FAILURE
scripts/6/CH6_3TC3.sh.............................FAILURE
scripts/6/CH6_4TC1.sh.............................FAILURE
success/all : 29 / 58
```

