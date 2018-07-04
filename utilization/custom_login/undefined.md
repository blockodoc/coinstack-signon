# 환경 구성

Nginx를 이용해서 사용자 정의 로그인 페이지를 화면에 표기하고 Coinstack SignOn 구동을 원활하게 하기 위해서는 환경 구성을 완료해야 합니다. 본 장에서는 Nginx 설정 파일 구조와 Coinstack SignOn 구동을 위한 환경을 구성하는 방법에 관해서 설명합니다.

### N**ginx**

HTTP, Reverse proxy 등의 서버를 구동할 수 있는 오픈 소스 웹서버 프로그램으로,

Coinstack SignOn에서 리소스 서버와 SignOn 서버의 Reverse proxy 서버로 활용될 수 있습니다.

자세한 사항은 [https://nginx.org/en/](https://nginx.org/en/)을 참조하시기 바랍니다.

### N**ginx 설치**

설치는 Coinstack SignOn이 설치된 OS에서 해야 하며, 이에 대한 명령어는 다음과 같습니다.

#### redhat 계열

redhat 계열은 Nginx에 대한 Repository를 제공하지 않기 때문에, 다음과 같이 저장소를 생성해야 합니다.

```text
# vi /etc/yum.repos.d/nginx.repo
```

**/etc/yum.repos.d/nginx.repo**의 내용을 다음과 같이 작성합니다.

${OS\_VERSION}에는 OS의 버전을 작성하면 됩니다. \(CentOS 7일 경우, ${OS\_VERSION}은 7\)

```text
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/${OS_VERSION}/$basearch/
gpgcheck=0
enabled=1
```

다음의 명령어를 통해 Nginx를 설치합니다.

```text
# yum update -y
# yum install -y nginx
```

#### debian 계열

debian 계열은 Nginx에 대한 Repository를 제공합니다.

따라서 별도로 저장소를 생성할 필요 없이, 다음의 명령어를 통해 Nginx를 설치합니다.

```text
# apt-get update
# apt-get install -y nginx
```

### N**ginx 구동**

Nginx 구동을 제어하는 명령어는 다음과 같이 **2가지**로 구분됩니다.

```text
# systemctl start nginx 
# systemctl stop nginx 
# systemctl restart nginx
```

```text
# service nginx start
# service nginx stop
# service nginx restart
```

