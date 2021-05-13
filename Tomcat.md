> # Linux Tomcat 설치

- version : Tomcat7
- java 호환 버전 : java 1.6 이상
- 설치
  - [Tomcat 설치 페이지](https://tomcat.apache.org/download-70.cgi)
  ```terminal
  # wget https://mirror.navercorp.com/apache/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz
  ```
- [Tomcat 디렉토리 구조](https://jokerkwu.tistory.com/117)<br />
  - bin : tomcat을 실행 및 종료하는 `*.sh` 파일 존재
  - conf : 서버 전체 설정 파일 폴더
  - lib : Library가 저장되는 경로
  - logs : 로그 저장
  - temp : 임시 저장용 폴더
  - webapps : 웹 어플리케이션 폴더
  - work : jsp파일을 Servlet파일로 변환한 `*.java`, `*.class`파일이 저장
  ![image](https://user-images.githubusercontent.com/81629923/117994325-6b734d80-b37b-11eb-92f4-53be1e14f579.png)
  - [참고 링크1](https://itwarehouses.tistory.com/8)
- 실행
  - 아래 명령어 후 `http://[IP]:8080/`로 접속 가능
  ```terminal
  # cd apache-tomcat-7.0.109/bin
  # ./startup.sh
  ```
- 자동 실행
  - service에 등록해주기
    ```terminal
    # vi /etc/init.d/tomcat
    # sudo chmod 755 /etc/init.d/tomcat
    # sudo update-rc.d tomcat defaults
    # systemctl start tomcat
    # systemctl enable tomcat
    ```
  - `/etc/init.d/tomcat` 입력
    ```sh
    #!/bin/bash
    
    case $1 in
    start)
    sh /root/Downloads/apache-tomcat-7.0.109/bin/startup.sh
    ;;
    stop)
    sh /root/Downloads/apache-tomcat-7.0.109/bin/shutdown.sh
    ;;
    restart)
    sh /root/Downloads/apache-tomcat-7.0.109/bin/shutdown.sh
    sleep 2
    sh /root/Downloads/apache-tomcat-7.0.109/bin/startup.sh
    ;;
    esac
    exit 0
    ```

> # Tomcat 설정

>> ## 설정 파일

- `~/apache-tomcat-7.0.109/conf/server.xml`
- `~/apache-tomcat-7.0.109/conf/context.xml`

>> ## Tomcat 경로 확인

- 기본 `webapp` 폴더로 경로가 잡혀있음
  - [`~/apache-tomcat-7.0.109/conf/server.xml`](https://indienote.tistory.com/496)
  ```xml
  ...
  <!-- 
    shutdown 요청을 받기 위한 listen Port 설정, admin port 이다.
      - port 값이 -1인 경우 shutdown 포트 기능을 사용하지 않음
      - server는 하나 이상의 service 객체를 가지고 있음
   -->
  <Server port="8005" shutdown="SHUTDOWN">
  ...
  <!-- 
    shutdown 요청을 받기 위한 listen Port 설정
      - port 값이 -1인 경우 shutdown 포트 기능을 사용하지 않음
      - service는 하나 이상의 container를 공유하는 connectors의 집합
   -->
  <Service name="Catalina">
  ...
  <!--
    url을 입력했을 때 url의 통신하는 정보를 나타냄
      - connector는 요청을 받고, 응답을 리턴하는 endpoint
      - connector는 수신된 요청을 container에게 요청을 전달
   -->
  <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
  ...
  <!--
    Engine은 적절한 Host(virtual host)로 처리를 넘기는 entry point
   -->
  <Engine defaultHost="localhost" name="Catalina">
  ...
  <!--
    default virtual host 정의
   -->
  <Host appBase="webapps" autoDeploy="true" name="localhost" unpackWARs="true">
  ...
  ```
- 기본으로 잡혀있는 경로
  - `~/apache-tomcat-7.0.109/webapps/ROOT` : 최상위 Root url 경로
  - `~/apache-tomcat-7.0.109/webapps/[폴더 A]` : `http://[IP]:[PORT]/[폴더 A]`
  - Root 경로 내에 있는 tomcat 관련 페이지 삭제 후 프로젝트 삽입
