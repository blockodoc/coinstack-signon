# 서버 코인 충전
기본적으로 블록체인에서 각 블록 여러 트랜잭션\(거래\)들로 구성되며, 트랜잭션\(거래\)은 입력들과 출력들로 구성되어있습니다.

### 거래 정보 예제

| Inputs | Outputs |
| --- | --- | --- | --- |
| Input\_0 | Output\_0 |
| Input\_1 | Output\_1 |
| Input\_2 | &nbsp; |

이러한 거래에서 아직 사용되지 않은 출력은 UTXO\(Unspent transaction output\)이라고 하며 다른 거래의 입력으로 사용될 수 있습니다. 이미 사용된 출력은 다른 거래에 포함될 수 없고, 만일 다른 두 개 이상의 거래 입력으로 포함된다면 이중 지불\(Double spent\)이 발생하였다고 합니다. 블록 체인의 거래는 이러한 규칙 아래에서 입력의 주소에대한 개인키로 거래에 서명함으로써, 무결성을 보장하게 됩니다.

앞서 설명한 것처럼, Coinstack SignOn은 관리자 주소와 서버 주소들을 사용하기 때문에 주소에 코인을 충전해야합니다. 서버의 성능에 따라 다르지만, 동시에 처리되는 요청을 처리하기 위해 서버당 20개 이상의 주소를 확보해야합니다. Coinstack SignOn은 이러한 주소에 코인을 충전하는 번거로움을 덜어주기 위해 코인 충전을 위한 명령을제공합니다.

### 코인 충전하기

코인을 충전하기 위해서는 다음과 같은 명령을 사용합니다.

```text
$ coinstack-signon coin recharge --privatekey ${COIN_SENDER_PRIVATEKEY}
```

이 명령은 설정에 있는 Admin과 Server Privatekey, Peer Address에 1코인을 충전해 줍니다. 명령을 실행하면,충전한 주소들을 출력해 줍니다.

만일, 더 많은 코인을 충전하기를 원한다면, 다음과 같이 인자로 충전할 코인량을 지정할 수 있습니다.

```text
$ coinstack-signon coin recharge --privatekey ${COIN_SENDER_PRIVATEKEY} 30
```

### 자주 발생하는 문제

#### 코인 부족

송금을 하려고 하는 주소에 충분한 코인이 없다면 다음과 같은 예외가 발생합니다.

> Insufficient fund
>
> io.blocko.coinstack.exception.InsufficientFundException: Insufficient fund
>
> ```text
> at io.blocko.coinstack.AbstractTransactionBuilder.preBuild\(AbstractTransactionBuilder.java:283\)
>
> at io.blocko.coinstack.TransactionBuilder.buildTransaction\(TransactionBuilder.java:25\)
>
> at coinstack.signon.command.RechargeCoin.execute\(RechargeCoin.java:54\)
>
> at coinstack.signon.exec.AbstractLauncher.run\(AbstractLauncher.java:80\)
>
> at org.springframework.boot.SpringApplication.callRunner\(SpringApplication.java:781\)
>
> at org.springframework.boot.SpringApplication.callRunners\(SpringApplication.java:771\)
>
> at org.springframework.boot.SpringApplication.run\(SpringApplication.java:335\)
>
> at org.springframework.boot.builder.SpringApplicationBuilder.run\(SpringApplicationBuilder.java:137\)
>
> at coinstack.signon.exec.RechargeCoinLauncher.main\(RechargeCoinLauncher.java:30\)
> ```
>
> Fail to execute the command

#### 수수료 부족

블럭 체인에 데이터를 기록하기 위해서는 수수료를 지급해야 합니다. 이 수수료는 트랜잭션에 기록되는 데이터의 량에 따라 일정 이상의 금액을 지정해야 합니다. 만일 여러 인풋과 여러 아웃풋이 트랜잭션에 포함된다면 트랜잭션의 크기가 커지게 됩니다. 만일 데이터 기록량에 필요한 수수료보다 적은 수수료를 지정하면, 다음과 같은 예외를 발생시키며 이 트랜잭션을 거부합니다.

> The request could not be processed. : transactionfd42522745408006eb75702cb3725a1aa8a9a8c2c2691627e4c11a4d63f2e436 has 10000fees which is under the required amount of 59255
>
> io.blocko.coinstack.exception.CoinStackException: The request could not be processed. :transaction fd42522745408006eb75702cb3725a1aa8a9a8c2c2691627e4c11a4d63f2e436has 10000 fees which is under the required amount of 59255
>
> ```text
> at io.blocko.coinstack.util.JsonToModel.parseError\(JsonToModel.java:29\)
>
> at io.blocko.coinstack.backendadaptor.CoreBackEndAdaptor.processError\(CoreBackEndAdaptor.java:180\)
>
> at io.blocko.coinstack.backendadaptor.CoreBackEndAdaptor.requestPost\(CoreBackEndAdaptor.java:144\)
>
> at io.blocko.coinstack.backendadaptor.CoreBackEndAdaptor.sendTransaction\(CoreBackEndAdaptor.java:419\)
>
> at io.blocko.coinstack.CoinStackClient.sendTransaction\(CoinStackClient.java:916\)
>
> at coinstack.signon.command.RechargeCoin.execute\(RechargeCoin.java:55\)
>
> at coinstack.signon.exec.AbstractLauncher.run\(AbstractLauncher.java:80\)
>
> at org.springframework.boot.SpringApplication.callRunner\(SpringApplication.java:781\)
>
> at org.springframework.boot.SpringApplication.callRunners\(SpringApplication.java:771\)
>
> at org.springframework.boot.SpringApplication.run\(SpringApplication.java:335\)
>
> at org.springframework.boot.builder.SpringApplicationBuilder.run\(SpringApplicationBuilder.java:137\)
>
> at coinstack.signon.exec.RechargeCoinLauncher.main\(RechargeCoinLauncher.java:30\)
> ```
>
> Fail to execute the command

