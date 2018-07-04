# 서버 로그

서버에서 발생하는 다양한 이벤트와 정보들은 로깅이라는 형태로 기록할 수 있습니다. 하지만, 모든 이벤트와 정보를남기게 되면 서버 성능이 저하되고, 로그를 적게 남기면 문제가 발생했을 때 원인을 파악하기 어렵습니다. 따라서, 개발 및 테스트 환경과 운영 환경처럼 서로 다른 로그 정책을 적용해야 합니다.

### 로그 확인하기

서버 로그를 확인하는 명령은 다음과 같습니다.

```text
$ coinstack-signon server log
```

만약, 다음과 같은 문구가 출력된다면. 서버가 실행 중인지 확인하시고 다시 시도해주시기 바랍니다.

```text
No process detected
```

로그 확인을 그만 두고 싶으시면, +C 를 통해 현재 상태에서 빠져나갈 수 있습니다.

해당 로그 파일들은 **${INSTALL\_PATH}/log** 디렉터리 아래 **server-YYYY-mm-dd.log** 형식으로 저장되어 집니다.

### Logback

Coinstack SignOn는 자바로 만들어져 있으며, Logback이라는 로거를 사용하고 있습니다. 서버에서는 **${INSTALL\_PATH}/conf/logback-server.xml** 파일을 통해 설정을 변경할 수 있습니다. 기본적으로 제공되는 로그 설정은 다음과 같습니다.

```markup
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>%d{HH:mm} %highlight(%-5level) %cyan(%logger{15}) - %msg%n</Pattern>
    </layout>
  </appender>

  <appender name="DAILY" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <!-- encoders are assigned the type
         ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <!-- daily rollover -->
        <fileNamePattern>${COINSTACK_SIGNON_LOG}/server-%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>200</maxHistory>
    </rollingPolicy>
    <encoder>
      <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="WARN">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="DAILY" />
  </root>

  <!-- Exception notifiaction for services -->
  <logger name="org.springframework.security.oauth2.provider.endpoint" level="info" />

  <!-- Keep silent for warning and error logging because it is already expected -->
  <logger name="org.springframework.security.oauth2.provider.token.store.JwtAccessTokenConverter" level="none" />
  <logger name="org.springframework.context.annotation.ConfigurationClassPostProcessor" level="error" />

</configuration>
```

본 장에서는 다음과 같은 내용을 변경하는 방법을 설명합니다.

* 로그 레벨
* 로그 메시지 포맷

### 로그 레벨

로그를 설정하는 대표적인 방법으로 로그의 레벨을 조정하는 방법이 있습니다. 로그 레벨의 종류는 다음과 같습니다.

* ERROR
* WARN
* INFO
* DEBUG
* TRACE

INFO레벨로 지정하면, ERROR, WARN, INFO 레벨의 메시지가 출력되며, TRACE레벨로 지정하면 ERROR,WARN, INFO, DEBUG, TRACE레벨의 메시지가 출력됩니다. 기본적으로 INFO 레벨로 지정되어 있습니다.

### 로그 메시지 포맷

로그 메시지에 추가적으로 필요한 정보가 있거나 필요없는 정보는 삭제할 수 있습니다. 로그 메시지의 형태는 layout에 의해서 처리됩니다. 본 문서에서는 가장 많이 사용되는 PatternLayout을 이용한 메시지 포맷에 대해서 설명합니다.

#### Pattern

패턴은 로그 메시지를 출력하는 형태를 지정합니다. 패턴은 특정한 동작을 지정하는 %로 시작하는 지시어와 상수 문자열로 나뉩니다. 지시어의 종류는 다음과 같습니다.

| 지시어 | 의미 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| c{length} / lo{length} / logger{length} | 메시지를 출력한 로거의 이름 |
| C{length} / class{length} | 로그를 출력한 클래스의 이름 |
| contextName / cn | 로그 컨텍스트의 이름 |
| d{pattern} / date{pattern} / d{pattern, timezone} / date{pattern,timezone} | 로그 메시지를 남긴 일시 |
| F / file | 로그를 남긴 자바 파일 이름 |
| caller{depth} / caller{depthStart..depthEnd} / caller{depth,evaluator-1, ... evaluator-n} / caller{depthStart..depthEnd,evaluator-1, ...evaluator-n} | 로그 메시지를 남긴 시점의 callstack 정보 |
| L / line | 로그를 남긴 자바 파일의 라인 번호 |
| m / msg / message | 로그 메시지 |
| M / method | 로그를 남긴 메서드 |
| n | 줄바꿈 문자.\(\n or \r\n\) |
| p / le / level | 로그 레벨 |
| r / relative | 서버가 시작한 이후 로그를 남긴시간 |
| t / thread | 로그를 남긴 스레드의 이름 |
| X{key:-default} / mdc{key:-default} | MDC에 저장된 "key"에 대한 값 |
| ex{depth} / exception{depth} / throwable{depth} / ex{depth,evaluator-1, ..., evaluator-n} / exception{depth, evaluator-1, ...,evaluator-n} / throwable{depth, evaluator-1, ..., evaluator-n} | 로그에 부가적으로 담겨있는 예외 |
| xEx{depth} / xException{depth} / xThrowable{depth} /xEx{depth, evaluator-1, ..., evaluator-n} / xException{depth,evaluator-1, ..., evaluator-n} / xThrowable{depth, evaluator-1, ...,evaluator-n} | ex와 동일한 정보인데, 추가적으로 클래스의 패키징 정보를 가지고 있습니다. |
| nopex / nopexception | 예외 정보를 출력하지 않음 |
| marker | 로거를 남길 때, 지정한 마커 |
| property{key} | "key"속성에 대한 값. 속성은 설정 파일이나 JVM 옵션을 통해전달되거나 OS 환경 변수를 참조합니다. |
| replace\(p\){r, t} | p문자열에서 정규표현식 r을 찾아서 t로 치환 |
| rEx{depth} / rootException{depth} / rEx{depth, evaluator-1, ...,evaluator-n} / rootException{depth, evaluator-1, ..., evaluator-n} | 로그의 예외 정보에서 가장 원인이 되는 예외 |

좀 더 상세한 내용을 위해서는 [https://logback.qos.ch/manual/layouts.html](https://logback.qos.ch/manual/layouts.html)를 참조하시기 바랍니다.

