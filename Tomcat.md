> # Linux Tomcat 설치

- [참고 링크1](https://community.venzia.es/apache-ssl-lets-encrypt-tomcat/)

- version : Tomcat7
  - java 호환 버전 : java 1.6 이상
  - [http/2 : tomcat7은 지원하지 않음](https://isabeller.tistory.com/23)
- 설치
  - [Tomcat 설치 페이지](https://tomcat.apache.org/download-70.cgi)
  ```terminal
  # wget https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz
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

> # AJP

>> ## AJP 사용 이유

- AJP는 웹 서버로 유입되는 트레픽을 위임하기 위한 프로토콜
- tomcat으로 요청 트래픽을 위임하지만, tomcat이 살아있는지 확인할 수 있는 모니터링 기능 지원
- 즉, apache를 통하여 L7 Loadbalance 기능을 구현인듯 싶음 ([참고 링크](https://velog.io/@makeitcloud/%EB%9E%80-L4-load-balancer-vs-L7-load-balancer-%EB%9E%80))
  - L4
    - 주로 Lound Robin 방식의 로드밸런싱 수행
    - TCP/UDP 정보로 로드밸런싱 수행
    - 속도가 빠르며, 효율이 높음
  - L7
    - L1 ~ L6 까지의 기능을 전부 가지고 있음
    - TCP/UDP 정보로 로드밸런싱 가능
    - HTTP, FTP, SMTP 등의 정보를 열람하여 섬세한 전송 가능
    - DDOS 등 비정상적인 트래픽을 걸러낼 수 있음
    - 상대적으로 L4보다 저렴
- 성능적 차이 및 확장 가능한 기능
  - Tomcat는 Web 서버와 WAS 서버로 모두 이용 가능
  - Tomcat은 Apache보다 상대적으로 느림
  - 초기는 이러한 문제를 해결하기 위하여 Apache 서버가 정적 페이지를 처리, Tomcat이 동적 페이지를 처리하기 위해 사용
  - 현재는 초기와 달리 Tomcat만으로도 커버가 가능하지만 Apache에서만 사용 가능한 유용한 기능을 사용하기 위함
  - [참고 링크](https://noobnim.tistory.com/26)
- [참고 링크1](https://sydesjokes.com/page-215/apache-tomcat-2/)

>> ## AJP를 사용하기 위한 설치
- Apache2 설치
  ```terminal
   # sudo apt-get install apache2
  ```
- Tomcat과 연동을 위한 `mod-jk` 설치
  ```terminal
  # apt-get install libapache2-mod-jk
  ```

>> ## Apache2와 Tomcat과 연결 설정

- Apache 설정 : `/etc/apache2/workers.properties` 작성
  ```properties
  workers.tomcat_home=[~/apache-tomcat-7.0.109 - apache path]
  workers.java_home=[/usr/lib/jvm/jdk1.6.0_45 - java path]
  
  # Define 1 real worker ajp13
  worker.list=testAJP
  
  # Set properties for tomcat1 (ajp13)
  ## worker.[worker.list에 적은 worker 명을 기입]
  ## worker.testAJP.type : Protocol
  ## worker.testAJP.lbfactor : 해당 worker로 부하분산을 비율을 정의
  worker.testAJP.port = 8009
  worker.testAJP.host = 34.64.140.160
  worker.testAJP.type = ajp13
  worker.testAJP.lbfactor = 1
  ```
- [Apache 설정 : `/etc/apache2/sites-available/000-default.conf` 수정](https://httpd.apache.org/docs/2.4/ko/vhosts/examples.html)
  - 12번줄 주석처리
  - 13 ~ 14번줄 작성 
  ```conf
             # DocumentRoot는 tomcat에서 호스팅되는 경로, 보통은 ROOT에 놓지만 webapps의 다른 프로젝트가 있다면 해당 경로로 기입
  12         # DocumentRoot /var/www/html
  13         DocumentRoot /root/wisenut/sf-1v5.3/apache-tomcat-7.0.109/webapps/ROOT/
             # JkMount와 연동 설정
             #  - /* : 접근할 수 있는 경로를 의미하며, 모든 접근은 /*를 놓고, 특정 경로 하위만 허용하고 싶으면 /[path...]/.../[*]로 기입
             #  - testAJP는 /etc/apache2/workers.properties 에서 정의된 workder명을 기입, worker를 여러개 두고 있다면 worker의 개수만큼 정의
  14         JkMount /* testAJP
  ```
- mod-jk 설정 : `/etc/apache2/mods-available/jk.conf` 수정
  ```terminal
  # a2dismod jk
  # cp /etc/apache2/mods-available/httpd-jk.conf /etc/apache2/mods-available/jk.conf
  # ln -f -s /etc/apache2/mods-available/jk.conf httpd-jk.conf
  # a2enmod jk
  # apache2ctl configtest
  ```
  - [mod-jk란?](https://java.ihavenomoney.co.kr/?page_id=275)
    - Apache와 Tomcat 사이를 중계
  - 23번 라인 주석처리, 24번라인 작성
  ```conf
  23     # JkWorkersFile /etc/libapache2-mod-jk/workers.properties
  24     JkWorkersFile /etc/apache2/workers.properties
  ```
- tomcat 설정 : `~/apache-tomcat-7.0.109/conf/server.xml` 수정
  - [참고 링크1](https://nirsa.tistory.com/131)
  - [참고 링크2](https://secunmblog.prtg.kr/entry/PRTG-PORT-Monitoring-Apache-Tomcat-CVE-2020-1938)
  - 65번 라인을 주석처리, 또는 HTTP Protocol로 수신도 하고 싶다면 그냥 두자
  - 85 ~ 88번라인 주석 해제 후 86라인 수정 및 89번라인 추가
    - 86번라인에 기입되어 있던 것의 의미
      - `::1` : 루프백 IP
        - 루프백IP란 자기 자신을 의미하는 IP를 의미, 연결 설정은 `hosts`파일에서 되어있음
          - IPv4 사용 가능 범위의 아이피 : `127.0.0.0/8`, 일반적으로 `127.0.0.1`을 사용
          - IPv6 사용 가능 범위의 아이피 : `0:0:0:0:0:0:0:1` 하나만 사용 <br />(왜그럴까? 찾아봤지만 알수 없었지만 유추하자면, IPv6의 탄생 배경에 있지 않을까한다. IPv6는 IPv4의 IP 대역의 고갈을 염두하여 만들어졌기 때문에 사용가능한 IP의 범위를 크게하기 위해서 하나만 있어도 될 루프백에 많은 아이피를 할당 범위로 놓지 않은 것같다.<br />물론 우려와 다르게 Private IP의 활용으로 기존 우려처럼 IPv4의 범위 초과의 문제에 직면하지 않았고, 현재 IPv6는 크게 사용되지 않는걸로 알고 있다.)
  ```xml
  ...
  65     <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
  ...
  85     <Connector protocol="AJP/1.3"
  86                address="0.0.0.0"
  87                port="8009"
  88                redirectPort="8443"
  89                secretRequired="false" />
  ...
  ```
- 모든 설정이 끝났다면 tomcat과 apache 재시작
  ```terminal
  # systemctl restart tomcat
  # systemctl restart apache2
  ```

> # SSL/TLS

- 설치
  - certbot
    ```terminal
    # add-apt-repository ppa:certbot/certbot
    # apt install certbot python3-certbot-apache
    ```
  - letsencrypt
    ```terminal
    # apt-get install -y letsencript
    ```

- certbot을 사용하여 SSL/TLS 인증서 발급
  - SSL/TLS를 사용 주의사항
    - SSL/TLS는 IP에 매칭되지 않고, DNS로 맵핑되기 때문에 공인된 IP 구역에서 조회할 수 있는 DNS가 필요
    - DNS를 발급받기 위해서는 발급 업체를 통해야되기 때문에 개인이 DNS를 발급받기 어려움, [무료로 DNS를 만들어 이용할 수 있는 링크](https://duckdns.org)
  - 명령어 : `certbot certonly --webroot -w [DocumentRoot] -d [DNS]`
    ```terminal
    # certbot certonly --webroot -w /root/wisenut/sf-1v5.3/apache-tomcat-7.0.109/webapps/ROOT/ -d orgtest.duckdns.org
    ```
  - 결과 출력 화면<br /><img src="https://user-images.githubusercontent.com/81629923/119249794-82653b80-bbd6-11eb-8a11-e80986f27568.png" width="500px" />
    - 유의사항
      - 인증서(암호화) 파일이 생성된 경로 확인 `/etc/letsencrypt/live/[DNS]/...`<br /><img src="https://user-images.githubusercontent.com/81629923/119250016-197ec300-bbd8-11eb-98b2-cf440644b2ff.png" width="500px" />
      - letsencrypt or certbot은 유효기간이 3개월이며, 이를 알려고이 있음

>> ## Apache2에 SSL/TLS 적용

- 설정 파일 수정
  - apache2 SSL/TLS 적용 : `/etc/apache2/sites-available/default-ssl.conf`
    ```conf
    ...
    DocumentRoot /root/wisenut/sf-1v5.3/apache-tomcat-7.0.109/webapps/ROOT/
    JkMount /* testAJP
    ...
    #   SSL Engine Switch:
    #   Enable/Disable SSL for this virtual host.
    SSLEngine on
    
    #   A self-signed (snakeoil) certificate can be created by installing
    #   the ssl-cert package. See
    #   /usr/share/doc/apache2/README.Debian.gz for more info.
    #   If both key and certificate are stored in the same file, only the
    #   SSLCertificateFile directive is needed.
    SSLCertificateFile      /etc/letsencrypt/live/orgtest.duckdns.org/cert.pem
    SSLCertificateKeyFile   /etc/letsencrypt/live/orgtest.duckdns.org/privkey.pem
    SSLCertificateChainFile /etc/letsencrypt/live/orgtest.duckdns.org/chain.pem
    ```
    - `default-ssl.conf` 파일을 apache2에 활성화
      ```terminal
      # sudo a2enmod ssl
      # a2ensite default-ssl.conf
      ```
    - 참고, `*.conf` 파일을 apache2에 비활성화
      ```terminal
      a2dissite [*.conf]
      ```
    - apache2 재기동
      ```terminal
      # systemctl restart apache2
      ```
  - [apache2 `http > https`로 자동 리다이렉트](https://www.hyoyoung.net/46)
    - [참고 링크1](http://httpd.apache.org/docs/2.2/en/mod/mod_rewrite.html)
    - [참고 링크2](https://blogger.pe.kr/824)
    - [테스트 링크1](https://htaccess.madewithlove.be/)
    - `/etc/apache2/sites-available/000-default.conf` 추가 설정
    ```conf
    ...
    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteCond %{HTTPS} off
        RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
    </IfModule>
    ...
    ```
    
>> ## Tomcat에 SSL/TLS 적용

- [참고 링크1](https://xinet.kr/?p=1655)
- [참고 링크2](https://geekflare.com/tomcat-ssl-guide/)
- [참고 링크3](https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html)
- [참고 링크4](http://blog.naver.com/PostView.nhn?blogId=siva6&logNo=221600284617)
- [참고 링크5](https://www.ibm.com/docs/ko/mpf/7.1.0?topic=ssl-configuring-apache-tomcat)
- [참고 링크6](https://www.ucert.co.kr/wiki/w/Tomcat_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EC%84%A4%EC%A0%95)
- java1.6 버전은 TLSv 1.1만 지원하고, TLSv1.2는 지원하지 않기 때문에 경고 문구가 

> # HTTP/2 사용

>> ## Apache

- [관련 파일 설치 및 재실행](https://stackoverflow.com/questions/37103927/why-does-apache2-module-http2-not-exist-on-ubuntu-16-04)
  ```terminal
  # add-apt-repository -y ppa:ondrej/apache2
  # sudo apt-key update
  # sudo apt-get update
  # apt-get --only-upgrade install apache2 -y
  # a2enmod http2
  # systemctl restart apache2
  ```

- 결과 화면<br /><img src="https://user-images.githubusercontent.com/81629923/119490208-8c826800-bd97-11eb-8fab-3a422d97d5cb.png" width="500px" />
