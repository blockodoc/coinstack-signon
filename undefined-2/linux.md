# Linux에서 설치 및 제거

### 설치 배포판

설치 배포판은 다음의 형태로 얻을 수 있습니다.

* \*.tar.gz
* \*.zip
* \*.bin

본 문서에서는 \*.bin을 사용하여 설치하는 방법에 대해서 설명합니다. \*.zip/\*.tar.gz는 \*.bin 를 사용할 수 없는 제한된 환경에서 설치할 수 있도록 제공되는 파일입니다. 해당 배포판은 다음의 위치에서 얻을 수 있습니다.

| 버전 | URL |
| --- | --- |
| v1.1 | [https://nexus.blocko.io/repository/blocko-product-repository/coinstack-signon-1.1-installer.bin](https://nexus.blocko.io/repository/blocko-product-repository/coinstack-signon-1.0-installer.bin) |

#### 설치 배포판 가져오기

설치 배포판은 nexus.blocko.io 서버에 저장되어 있습니다. 배포판은 curl이나 wget 명령으로 가져올 수 있습니다.

**curl의 경우**

```text
$ curl -o coinstack-signon-installer.bin\
    https://nexus.blocko.io/repository/blocko-product-repository/coinstack-signon-1.1-installer.bin
```

**wget의 경우**

```text
$ wget -O coinstack-signon-installer.bin\
    https://nexus.blocko.io/repository/blocko-product-repository/coinstack-signon-1.1-installer.bin
```

### 설치 준비

설치를 위해서는 unzip 명령이 필요합니다. 상황이 여의치 않아 unzip을 사용할 수 없다면, tar.gz 배포판을 준비하는 것이 좋습니다.

#### unzip 설치

설치 프로그램은 unzip 명령이 사용가능하지 확인하고 필요하면 unzip 설치를 자동으로 진행하지만, unzip 설치를위해서는 관리자 권한\(sudo\)이 필요합니다. 만일, 설치 계정이 관리 권한을 갖지 않는다면, 미리 unzip을 설치해야만 원활히 진행할 수 있습니다.

**redhat 계열**

```text
# yum install -y unzip
```

**debian 계열**

```text
# apt-get install -y unzip
```

그 외 리눅스 시스템에 대해서는 해당 관리자에게 설치 문의/요청하시기 바랍니다.

### 설치

#### 설치 프로그램 실행하기

프로그램의 설치는 배포판을 실행함으로써 수행됩니다. 따라서, 해당 파일에 실행 권한을 주어야 합니다.

**실행 권한 주기**

```text
$ chmod +x coinstack-signon-installer.bin
```

**설치 프로그램 실행**

```text
$ coinstack-signon-installer.bin
```

**명령행 자동완성 스크립트 추가**

명령행 자동완성 스크립트는 ~/.bash\_completion.d/coinstack-signon.completion 안에 설정됩니다.

이를 실행되게 하기 위해서는 다음의 명령어를 ~/.bash\_completion안에 추가하여야 합니다.

```text
for bcfile in ~/.bash_completion.d/* ; do . $bcfile; done
```

위의 과정이 끝나면 다음의 명령어를 실행하여, bash를 재실행합니다.

```text
$ exec bash
```

### 제거

프로그램의 제거하기 위해서는 해당 디렉토리를 삭제하기만 하면 됩니다.

```text
$ rm -rf $INSTALL_PATH
```

