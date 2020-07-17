# Kyck-Kyck Structure

## 서비스 구성
<img width="717" alt="kyck_diagram" src="https://user-images.githubusercontent.com/61036148/87640935-6442d980-c782-11ea-97c6-6e0b15323530.png">
해당 서비스는 기본적으로 유튜브 동영상을 사용하며 자체 필터링/정리 후 등록한 동영상 정보를 활용하여 머신러닝 추천과 동영상 시청 패턴, 누리과정 학습 등에 대한 조언을 부모에게 해주는 리포트 기능 등을 포함하는 아동용 동영상 플랫폼입니다.

보안을 위해 AWS 내 구성은 NginX만 Public IP를 사용하고 그 외 서버들 간의 통신은 Private IP만 사용하였고 외부로부터의 접근은 SSH로만 가능하도록 하였습니다.

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
> 모바일 앱에 대한 서버이며 Proxy server로 있는 NginX는 HTTPS, 메인 서버인 Node.js는 HTTP를 사용합니다.   
> SSL/TLS 인증서 발급은 Let’s Encrypt, 등록과 자동갱신 설정은 Certbot을 사용했습니다.   
> Youtube API를 사용하여 동영상 정보를 가져와 활용하고 FCM을 사용하여 사용자에게 Push 메세지를 보냅니다.   
> 회원가입/로그인과 인증은 Legacy 방식을 사용하지 않고 사용자의 전화번호로 OTP(One-Time Password)를 발행하고 인증이 되면 JWT(JSON Web Token)를 발급하도록 되어있습니다.   
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
>
> 메인서버로 부터 요청이 오면 해당 사용자에 맞는 추천 동영상 데이터를 응답하는 Flask 서버입니다.   
> 서비스 초기 적은 데이터의 소규모 사용자에 맞춘 추천 시스템입니다.   
> 각 동영상 별 메타데이터를 활용한 레이블링(Labeling) 및 자체 지정한 특정 레이블링으로 이루어진 개별 동영상의 사용자 시청에 따른 데이터를 벡터화(Vectorization)한 후 KNN(K-Nearest Neighbors)을 이용하여 회기( Regression) 분석을 하는 것에 기초를 두었습니다.   
> 기본적으로 동영상 별 거리 측정은 유클리드 거리(Euclidean Distance)를 사용하고 특정 동영상을 중심으로 n개의 근접 동영상들, 그리고 시청 동영상의 시리즈 별 연계성을 고려하여 최종적으로 사용자에게 동영상을 추천하는 시스템입니다.   
> 좀 더 나은 퍼포먼스를 위해 Vectorizing한 시청 데이터를 계속적으로 DB에 업데이트하고 요청이 왔을 때 불러와 연산하는 방식을 취했습니다.   
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
>
> 유저는 Admin/Manager/Editor로 Role이 나누어져 있고 Passport를 사용하여 Legacy 방식으로 로그인하며 express-session을 사용하여 세션정보를 MongoDB(connect-mongo 사용)에 저장하여 관리하고 각 유저들의 관리자 페이지의서의 작업은 middleware를 통해 따로 저장해 놓습니다.   
> 동영상의 메타데이터 및 자체 작성 정보를 DB에 저장하고 관련 이미지를 S3로 업로드합니다.   
> 모바일앱에서 업데이트 될 유튜브 채널 및 동영상 관리, 이벤트 모달(Modal) 및 게시판의 데이터 관리 기능 등이 있습니다.   
<br>

> ### MongoDB Atlas
> AWS Region : Seoul (ap-northest-2)   
> Cluster Tier : M10   
> MongoDB version : v4.0.16   
> Replica Set : 3 nodes   
> Backups : Active   
>
> DB는 운영 인력 부족으로 직접 구성하지 않고 MongoDB의 SaaS인 Atlas를 이용하였습니다.   
> 3개의 노드로 구성된 Replica Set을 사용하였고 Sharding은 사용하지 않았습니다.   
> ODM(Object Data Mapping)으로 Mongoose를 사용하여 모든 모델들의 스키마(Schema)가 정해져 있고 메모리 대비 효율적인 퍼포먼스를 위해 하나의 모델 당 인덱싱(Indexing)은 최대 3-4개 정도로 한정하여 구성했습니다.   
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


