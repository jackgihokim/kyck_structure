# kyck_structure

## 서비스 구성
<img width="717" alt="kyck_diagram" src="https://user-images.githubusercontent.com/61036148/87271118-39038480-c50d-11ea-8e9b-cf1d788cb277.png">
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

> ### AWS S3
> AWS Region : Seoul (ap-northest-2)   
> Access type : Public   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	kks-profiles-pub   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	kks-seasons-pub   
> Access type : Private   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	kks-cat-animation-img   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	kks-cat-music-img   
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-	kks-cat-youtuber-img   
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


