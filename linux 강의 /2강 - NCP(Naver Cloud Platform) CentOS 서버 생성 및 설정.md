	-	서비스 서버> ncp
	-	개발 서버> 피씨에 VM으로 도커 설치
	-	레디스는 메모리 디비
	-	ssd는 칩,  hdd는 딱딱
	-	vpc는 내부로 들어간다
	-	아카이브스토리지는 압축해서 들어간다
	-	ACG (Access Control Group) = 방화벽
	-	접속용 ip ! = 서비스용 ip (공인 ip)

# TCP VS UDP
-Transmission Control Protocol
-User Datagram Protocol
￼

# 접근소스(출발지)
	•	0.0.0.0/0 모든 곳 접속

# port       
	-	22          ssh
	-	80          http >> 외부에서도 들어오게 함 (웹서버)
	-	443         https
	-	3306        Mysql
	-	33060       nosql (mysqlx api)

* 포트 포워딩 안하면, 외부에서 못들어옴 해당 포트가 있어야만 접속 가능 
