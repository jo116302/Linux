> Java
- 기본적으로 Open JDK 1.8 설치가 되어있음
  ```
  java -version
  ```
- Open JDK 설치 경로 확인
  ```
  which java
  ```
- Open JDK 파일 명 변경
  ```
  mv /bin/java /bin/java_old
  ```
- Java 1.8 설치
  - [v 1.8 Download_Link](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
  ```
  # tar -zxvf jdk-8u281-linux-x64.tar.gz
  # mv ./jdk1.8.0_281/ /usr/local/lib
  # vi /etc/profile
  
  // /etc/profile 수정
  JAVA_HOME=/usr/local/lib/jdk1.8.0_281
  JRE_HOME=$JAVA_HOME/jre
  PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
  export JAVA_HOME
  export JRE_HOME
  
  // /etc/profile 수정사항 적용
  # source /etc/profile
  ```
  - 환경 변수 설정할 때 에러
    1. java: error while loading shared libraries: libjli.so: cannot open shared object file: No such file or directory
        ```
        # find / -name 'libjli.so'
        
        // find에서 찾은 libjli.so의 경로를 java-libjli.conf를 생성하여 기입 후 저장
        # vi /etc/ld.so.conf.d/java-libjli.conf
        # ldconfig
        ```
  [설치 링크](https://copycoding.tistory.com/290#google_vignette)
