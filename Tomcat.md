> # Linux Tomcat 설치

- version : Tomcat7
- java 호환 버전 : java 1.6 이상
- 설치
  - [Tomcat 설치 페이지](https://tomcat.apache.org/download-70.cgi)
  ```terminal
  # wget https://mirror.navercorp.com/apache/tomcat/tomcat-7/v7.0.109/bin/apache-tomcat-7.0.109.tar.gz
  ```
- 실행
  - 아래 명령어 후 `http://[IP]:8080/`로 접속 가능
  ```terminal
  # cd apache-tomcat-7.0.109/bin
  # ./startup.sh
  ```
