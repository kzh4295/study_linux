# 서버에 mysql 설치
	1.	mysql 다운로드에서 os 환경을 레드 햇 계열 - 센토스 7 & os version은 (uname -a)으로 cpu 확인( x86, 64bit 확인)
	2.	yum local install 링크주소 ( >> 리파지토리를 로컬에 설치하는 것임>> server, dependency, mariadb 다운확인)
	3.	vi /etc/my.cnf
# 스왑파일(.swp)
vi 에디터 스왑파일은 파일을 편집하는 과정에서 예기치않게 종료한 경우 생성,
이떄 원본 파일이 스왑파일로 변경된 것이 아니라 원본파일은 그대로 있고 스왑파일이 생성되는것 

오류 환경: mysql 설치 후 vi 에디터로 내용을 편집하다가 실수로 종료 
>>오류 해결: 스왑 파일명을 찾고 (ls -a 아니면 ll -all)서버에 [root@mydeal ~]# rm /etc/.my.cnf.swp 입력
                                ￼
         configure mysql server 설정 복붙 
                Cf) utf8 : 웬만한 각국 언어 출력 가능, mb4 : utf8이 원래 3바이트인데 4바이트로 쓰겠다 지정
                에러 발생시, 3.과 5.의 순서를 변경해서 했더니 오류 x
	4.	설치 후 ps - ef | grep mysql로 프로세스 확인
	5.	systemctl start mysql
	6.	grep ‘temporary password’/var/log/mysqld.log
       >> var의 log 밑에는 system log들이 모여있음
       Cf) cat 파일 : 파이 전체 내용 다 보여줘
            Grep 단어: 파일 내용 중 단어만 보여줘
 7. mysql - u root -p로그인
 8.	show database;
	CREATE DATABASE mydealdb;
 10.	 user 생성 및 권한 부여 
     >>root계정은 위험하니까, 서비스 유저 mydeal
     CREATE USER 'mydeal'@'%' IDENTIFIED WITH mysql_native_password BY 'xxx';     >> mydeal : 유저이름// % : 출발지 ip 
     GRANT ALL PRIVILEGES ON mydealdb.* TO 'mydeal'@'%' WITH GRANT 
                                 (*.*)db명.table명

	11.	quit으로 프로세스 종료
