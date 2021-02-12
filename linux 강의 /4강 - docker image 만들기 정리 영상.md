엔진엑스컨프 도커에 배시알씨로데이트 같은 거 변경
Ip 쓰기 싫어서 mydela로 변경
￼
피씨는 호스트 os, 도커는 테스트 서버(mydealdev 컨테이너)
ncp는 실제 서비스 서버
알씬크 이유 내 피씨에서 코딩한 것을 서버에 올려야 돌아가니 파일 주고 받을 때 여기 파일을 여기로 가져오겟다
서버에 올리려면  출발지 도착지 순서로 셸 스크립트를 짜면 된다
vi .bashrc는 도커에 내가 들어왔을때 주로 하는 것들을 저장해 놓는다
. .bashrc : .(실행) .bashrc 
ln -s /home/workspace/www/mydeal www : 도커 마이딜 디비 건테이너에서 자주가는 /home/workspace/www/mydeal위치를 링크걸어 두겟다
그냥 pwd는 /usr/bin/pwd이며 /bin/pwd와 다름
6분 부터 이해 되지 않음


