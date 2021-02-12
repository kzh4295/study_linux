	•	yum repolist : yum으로 설치된 list 보기
	•	yum install epel-release -y : extra package 확장팩(기본 패키지 외에 많이 사용하는 것들 리파지토리로 등록)
	•	yum install whois -y
	•	

Nginx 세팅
	•	무료 도메인 : cf) 프리놈, 가비아
	•	도메인 만든 곳에서 dns 등록
	•	nslookup 도메인 주소 : ns(name server : 도메인 이름과 IP의 상호변환을 가능하게 해주는 서버) lookup(찾다)                                                        
      도메인 주소의 ip주소를 찾기
      >> 현재 나는 nslookup kzh.topician.com으로 하였음(mydeal.topician.com은 선생님          
         거니까 사용하지 말자!!)
	•	ps -ef | grep nginx로 프로세스 확인
	•	nginx.conf의 경로 파악해서 위치 이동 (cd /etc/nginx)
	•	cat nginx.conf : nginx의 디폴트 conf확인
      >>error_log : nginx의 오류는 /var/log/nginx/error.log warn여기에서 해결
        좀비 pc 되면 kill pid
        Pid는 현재 cat /var/run/nginx.pid
worker_connections 1024 : 동시 접속자 1024까지 허용
log_format main에서 remote_addr : 사용자 ip
access_log : 접근자들 확인 가능
gzip  on; :html용량 많이 차지해서 압축사용하면 패킷 비용감소
include /etc/nginx/conf.d/*.conf; : 상단의 include    /etc/nginx/mime.types;여기 안들어 간 것들은 conf.d디렉터리의 *.conf파일에 저장 될 예정

	•	vi /etc/nginx/mime.types : nginx에 기본적으로 가지고 있는 확장자들 확인 가능, 추가도 가능
	•	tail … 여기는 잘 모르겟다..(3-4강의 11분 ~13:37)
	•	curl http://kzh.topician.com
      >> 301 : redirect 발생
      Cf) 200 : ok / 304 : not modified / 404 : not found / 500 : 서버 에러

	•	ll conf.d/ : conf 파일(설정파일) 보기
	•	cat conf.d/default.conf : conf.d 디렉터리에 있는 default.conf파일의 내용을 보여줘
                  >> server{
      location / : kzh.topician.com/ 슬래시 뒤에 무엇이 오든 하단의 root(=도큐먼트 루트?)와 index로 들어와
	•	ll /usr/share/nginx/html (root) 해서 내용봤을때 index.html이 기본파일
	•	vi /usr/share/nginx/html/index.html
	•	error_page를 보고 싶으면 error_page   500 502 503 504  /50x.html; : error나면 500 502 503 504 에러를 보여줘
	•	vi /usr/share/nginx/html/50x.html
	•	location ~ \.php$ { :php 서비스를 하고 싶을 때 php proxy
	•	       #    proxy_pass   http://127.0.0.1; :flask에서 proxy_pass라는 말을 사용 할 예정
	•	vi conf.d/mydeal.conf : 나의 conf파일 생성
     >> server_name : 나의 도메인 주소
        nginx는 끝에 ;
        try_files $uri /index.html; : uri(도메인 다음 부분)가 없는 데 부르면 index.html로 가도록 걸어줘라
        (왜? 클라이언트가 요청을 해도 서버에는 없는 내용이어서 찾지 못하니 리액트(라우터 기능)가 찾을 수 있도록)
	•	ll /var : var 밑에 www가 있으면 html이 모여있는 디렉터리구나
	•	mkdir -p /var/www/mydeal : -p는 var 밑에 www도 없는 경우 mydeal까지 만들기 귀찮으니까 -p 뒤에 나온 대로 경로 만들수 있도록 해줌 >> ll /var/www 입력해 mydeal 있는지 확인
	•	echo “this is MyDeaL Homepage” > /var/www/mydeal/index.html : 화면에 띄우는 것이 아니라 > 파일에 내용을 넘겨줌
	•	ll /var/www/mydeal하고 재시작해야 반영됨 (무조건 configuration 수정은 재시작 되어야함)
	•	nginx -t : nginx야 테스트 한번 해줘 >> syntax is ok & test is successful 뜨면 반영 잘된 것
	•	systemctl reload nginx : 서비스로 진행 중인 것은 systemctl으로 띄우는게 낫다 , restart보다 configuration수정은 reload로 하는 것이 낫다
	•	Let's Encrypt는 ?(= certbot)
	•	보안 웹사이트를 위한 인증서의 수동 생성, 유효성 확인, 디지털 서명, 설치, 갱신 등 종전의 복잡한 과정을 없애주는 자동화된 프로세스를 통해 전송 계층 보안 암호화를 위해 무료 X.509 인증서를 제공하는 인증 기관이다.
	•	yum install certbot python2-certbot-nginx -y : 2개 설치
	•	systemctl stop nginx : nginx 프로세스가 진행중이면 certbot도 80포트를 사용하여 에러가 나니 nginx를 중지
	•	certbot —standalone -d kzh.topician.com certainly : name server를 등록해줘야 인증서 설치 가능 
	•	cf) 최대 90일 지나면 인증서 만료
	•	vi conf.d/mydeal.conf해서 복붙함
      >>> 80포트 = http >  return  301 https://$host$request_uri; : https로 리턴해라, host: 도메인, 
          443포트는 암복호화 해야해서 Let’s Encrypt의 키체인 2개 (도메인은 내것으로 변경)를 하단에 붙여 준다.
	•	systemctl start nginx (journal 오류는 conf 파일이 잘못되서 그런 것)
	•	certbot renew : 인증서 갱신
	•	crontab -l > 분 시 일 월 초 / /tmp/.nu.log 2 > 2 : 표준 출력 에러
	•	which certbot으로 full 경로를 입력해야 헤매지 않는다
	•	30 06 * * * /usr/sbin/nsight_updater > /tmp/.nu.log 2> /tmp/.nu_err.log 
	•	: 6시 30분에 표준 출력 에러는 /tmp/.nu_err.log 여기로 보내라
	•	0 5 1 */2 * /usr/bin/certbot renew >> /etc/nginx/cert.log 2>&1
	•	:(분, 시, 일, 월, 주) 짝수달 1일 5시에 certbot을 갱신해라 /etc/nginx/cert.log 2 (에러가 발생하면 표준출력이 없으니 씹어라(&1) 왜? /etc/nginx/밑에 var.log에 두어야 하는데 certbot은 표준 출력 발생시키지 않으니 오류가 발생하면 씹어라
	•	crontab -e : 등록 (크론탭은 vi로 여는 것이 아니라 -e로 연다)
	•	



