# kyck_structure

## 서비스 구성
<img width="717" alt="kyck_diagram" src="https://user-images.githubusercontent.com/61036148/87640935-6442d980-c782-11ea-97c6-6e0b15323530.png">
해당 서비스는 기본적으로 유튜브 동영상을 사용하며 자체 필터링/정리 후 등록한 동영상 정보를 활용하여 머신러닝 추천과 동영상 시청 패턴, 누리과정 학습 등에 대한 조언을 부모에게 해주는 리포트 기능을 포함하는 아동용 동영상 플랫폼입니다.

기본적으로 AWS내에서 NginX를 제외한 서버들의 통신은 Private IP만 사용하고 외부로부터의 접근은 SSH로만 가능하도록 구성했습니다.

<br>


## Back-End

> ### Service : Main
> AWS EC2 Instance : t3.medium   
> OS : Ubuntu 18.04 LTS   
> Proxy Server : NginX v1.14.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Domain Name : __   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- AWS Elastic IP : Using   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- AWS Route 53 set : A Record, CNAME   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- TLS Certificate : Let’s Encrypt with Certbot   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Protocol : HTTPS   
> Main Server : Node.JS v8.12.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Protocol : HTTP   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Process Manager : PM2 v3.2.2   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Framework : Express v4.16.4   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Authentication : JSON Web Token with Passport & One-Time Password   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- ODM : Mongoose v5.3.11   
>
> 모바일 앱에 대한 서버이며Proxy server로 있는 NginX는 HTTPS, 메인 서버인Node.js는 HTTP를 사용합니다.   
> SSL/TLS 인증서 발급은 Let’s Encrypt, 등록과 자동갱신 설정은 Certbot을 사용했습니다.   
> Youtube API를 사용하여 동영상 정보를 가져와 활용하고 FCM을 사용하여 사용자에게 Push 메세지를 보냅니다.   
> 로그인과 인증은 Legacy 방식을 사용하지 않고 사용자의 전화번호로OTP(One-Time Password)를 발행하고 인증이 되면 JWT(JSON Web Token)를 발급하도록 되어있습니다.   
> 전체 사용자의 동영상 시청 데이터 관련 주기적 연산 작업(예: 시간당 전체 평균 등)은 Cron Job을 통해 하였습니다.   
> Flask 서버로 추천 데이터를 요청하고 받은 데이터와 계속적으로 업데이트 되는 시청 패턴과 학습 관련 데이터를 클라이언트로 보냅니다.   
<br>

> ### Service : Recommender Engine
> AWS EC2 Instance : t3.medium   
> OS : Ubuntu 18.04 LTS   
> Web Server : NginX v1.14.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	AWS Elastic IP : Using   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Protocol : HTTP   
> Recommender Server : Flask v1.0.2   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Python version : v3.6.8   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Protocol : HTTP   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	WSGI HTTP Server : Gunicorn v19.7.1   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Process Manager : Supervisor v3.3.1-1.1   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	ML Package : scikit-learn v0.21.1   
<br>

> ### Admin
> AWS EC2 Instance : t3.medium   
> OS : Ubuntu 18.04 LTS   
> Proxy Server : NginX v1.14.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Domain Name : __   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	AWS Elastic IP : Using   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	AWS Route 53 set : A Record, CNAME   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	TLS Certificate : Let’s Encrypt with Certbot   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Protocol : HTTPS   
> Main Server : Node.JS v8.12.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Protocol : HTTP   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Process Manager : PM2 v3.2.2   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Framework : Express v4.16.4   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Authentication : crypto & express-session with Passport   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	ODM : Mongoose v5.3.11   
<br>

> ### MongoDB Atlas
> AWS Region : Seoul (ap-northest-2)   
> Cluster Tier : M10   
> MongoDB version : v4.0.16   
> Replica Set : 3 nodes   
> Backups : Active   
<br>


## Front-End (Client)

> ### Hybrid Mobile App
> Framework :   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Angular 6   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Ionic 4   
> Cordova : v8.1.2   
> Platforms :   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Android : v8.0.0   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	iOS : v4.5.5   
> Authentication : One-Time Password & JSON Web Token from Server   
<br>

> ### Admin Website
> Framework :   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Angular 6   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	Ionic 4   
> Authentication : Legacy Login & User information from Server Session   


