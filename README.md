# kyck_structure

## 서비스 구성
<img width="717" alt="kyck_diagram" src="https://user-images.githubusercontent.com/61036148/87271118-39038480-c50d-11ea-8e9b-cf1d788cb277.png">


## Back-End

### 킥킥 Service / Main
> AWS EC2 Instance : t3.medium
> OS : Ubuntu 18.04 LTS
> Proxy Server : NginX v1.14.0
    -	Domain Name : www.wizborn.kr
    -	AWS Elastic IP : Using
    -	AWS Route 53 set : A Record, CNAME
    -	TLS Certificate : Let’s Encrypt with Certbot
    -	Protocol : HTTPS
> Main Server : Node.JS v8.12.0
    -	Protocol : HTTP
    -	Process Manager : PM2 v3.2.2
    -	Framework : Express v4.16.4
    -	Authentication : JSON Web Token with Passport & One-Time Password
    -	ODM : Mongoose v5.3.11

