> # Linux Tomcat 설치

- version : Tomcat7
- java 호환 버전 : java 1.6 이상
- 설치
  - [Tomcat 설치 페이지](https://tomcat.apache.org/download-70.cgi)
  ```terminal
  # wget https://mirror.navercorp.com/apache/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz
  ```
- Tomcat 기본 경로
  ![image](https://user-images.githubusercontent.com/81629923/117994325-6b734d80-b37b-11eb-92f4-53be1e14f579.png)
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
