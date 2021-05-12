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
  - webapp : 웹 어플리케이션 폴더
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
