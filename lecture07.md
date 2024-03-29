# 第7回課題

- 感想
- 自分の環境の脆弱性について考える
- 自分の環境の脆弱性への対策

---

## 感想

前回に引き続き様々なAWSサービス名が登場したので  
それぞれの役割について整理するのに時間がかかると感じた。  
ただ、セキュリティに関する問題は日々更新され  
内容を把握するためのコストがかなり必要ではあるけれど  
AWSサービスに頼り切るのではなく「OWASP Top 10」などの  
情報共有サイトを日々確認し自分たちで実施できる防護策はすべて実施した上で  
補助的にAWSサービスを利用するという意識が大切だと感じた。

---

## 自分の環境の脆弱性について考える

- HTTP通信接続：第5回の課題以降、ALB経由でEC2上のｻﾝﾌﾟﾙｱﾌﾟﾘｹｰｼｮﾝに接続しているがHTTPSではなくHTTP通信を利用している。
  - HTTPとHTTPSについて：HTTPとHTTPSは暗号化されているかいないかの違いがあり後者のHTTPSが暗号化された通信である。HTTPSはSSL/TLSという通信プロトコルを利用してHTTP通信を行う。SSL/TLSによりHTTPのリクエストやレスポンスが暗号化されるためHTTPよりも安全な通信ができる。SSLサーバー証明書をサーバーにインストールすることで利用できる。
- 認証情報の流出：課題をGithubで公開しているがAWSのマネジメントコンソールのスクショなどの証跡画像を多数使用しているため情報流出のリスクがある。

---

## 自分の環境の脆弱性への対策

### 「HTTP通信接続」の脆弱性への対策

#### HTTPS通信を行う

1. AWS Certificate Manager(ACM)にてパブリックSSL証明書を作成する。
2. 第5回で作成したALBにHTTPS:443のリスナーを追加し、セキュアリスナーの設定でACMで作成したSSL証明書を選択する。
3. Route53（DNSサービス）でホストゾーンを作成し、ALBのドメイン名を指定したAレコードを作成する。
4. HTTP:80に接続する通信について、ALBリスナーの設定から「URLにリダイレクト」の設定でHTTPS:443にリダイレクトするよう設定する。

#### 「認証情報の流出」の脆弱性への対策

アナログな手法ではあるがアップロードするスクショの内容を必ず確認し、認証につながる部分については表示されないように避ける、もしくは加工して表示されないようにする。
また、WEBでの公開は情報流出のリスクがあることを念頭に置いて注意するよう意識する。
