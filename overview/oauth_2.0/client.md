# 클라이언트\(Client\)

OAuth 2.0의 가장 큰 특징은 다양한 상황에 대해서 사용할 수 있는 인가 프레임워크라는 점입니다. 이를 위해 클라이언트의 종류를 분류하고, 이에 따른 다양한 인가 증명 방법을 수행합니다.

### 클라이언트의 종류

클라이언트는 크게 클라이언트 상에 자격 증명 수단을 유지해도 안전한지에 따라 다음과 같이 분류됩니다.

#### confidential

자격 증명을 위한 값을 유지해도 안전한 클라이언트. 자격 증명에 제한적으로 접근할 수 있도록 설계된 애플리케이션.

#### public

자격 증명을 위한 값을 유지하면 안 되는 클라이언트. 브라우저, 모바일 앱 등.

### 수용 가능한 클라이언트

OAuth 2.0은 제안 시점에 다음과 같은 클라이언트들을 염두에 두고 설계되었습니다.

#### Web application

웹서버 상에서 실행되는 웹 애플리케이션은 **confidential** 클라이언트입니다. 리소스 오너는 브라우저\(User-agent\)를 통해 표현되는 웹 페이지를 통해 클라이언트에 접근할 수 있습니다. 클라이언트 자격 증명뿐만 아니라 발행되는 모든 토큰을 서버에 저장할 수 있습니다.

#### User-agent-based application

브라우저에서 실행되는 SPA\(Single Page Application\) 같은 애플리케이션은 **public** 클라이언트입니다. 서버로부터 코드가 다운로드되어서 실행되고 실행 데이터와 결과가 브라우저상에 존재하기 때문에 손쉽게 접근할 수 있습니다.

#### Native application

사용자의 컴퓨터나 모바일 장치에 설치되어서 단독으로 실행되는 애플리케이션은 **public** 클라이언트입니다. 프로토콜 데이터와 자격 증명 값에 접근할 수 있기 때문에 문제가 됩니다.
