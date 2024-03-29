> # Linux Version 확인
  ```
  # lsb_release -a
  ```

> # useradd & adduser & groupadd
  - user 생성
    ```
    // adduser : 실행시 기본 계정 정보를 자동으로 생성
    # adduser [username]
    // useradd : 계정만 생성, 기타 계정 정보를 수동으로 생성 및 설정
    # useradd [username]
    ```
  - group 생성
    ```
    # groupadd [groupname]
    ```
    
> # 설치된 서비스 확인

  ```terminal
  dpkg -l | grep [Search]
  ```

> # 시간 동기화
  - 현재 Date 확인
    ```
    # date
    ```
  - 시간 동기화
    ```
    # ls /usr/share/zoneinfo/
    # sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
    ```

> # X Window 설치
  - 업데이트 필요
    ```
    # sudo apt-get upgrade
    # sudo apt-get update
    ```
  - X Window 설치([참고 링크](https://s-engineer.tistory.com/224))
    ```
    // X Window 요소 모두 설치
    # sudo apt-get install ubuntu-desktop
    // X Window 요소 최소한의 설치
    # sudo apt-get install --no-install-recommends ubuntu-desktop
    // X Window 
    # startx
    ```
  - XRDP 설치([참고 링크](https://marcokhan.tistory.com/248))
    ```
    # sudo apt-get install xrdp
    # sudo apt-get install xorgxrdp
    # sudo apt-get install xorg-video-abi-23
    ```

> # vsftp install

  - vsftp 설치
    ```terminal
    # sudo apt-get install vsftpd
    ```
  - root 사용자 로그인 가능하게 설정
    - 아래 파일에서 root 주석 처리
    ```terminal
    # vi /etc/ftpusers
    ```
  - 쓰기 권한 부여
    - `write_enable=YES`로 수정
    ```terminal
    # vi /etc/vsftpd.conf
    ```

> # firewall
  - 방화벽 상태 확인
    ```terminal
    # sudo ufw status verbose
    ```
  - 방화벽 활성화 및 비활성화
    ```terminal
    // 방화벽 활성화
    # sudo ufw enable 
    // 방화벽 비활성화
    # sudo ufw disable
    ```
  - 방화벽 규칙 확인
    ```terminal
    # sudo ufw show raw 
    ```
  - 방화벽 규칙 변경
    ```terminal
    // 특정 PORT 통신 허용
    # sudo ufw allow [PORT]/[TCP/UDP]
    // 특정 PORT 통신 거부
    # sudo ufw deny [PORT]
    ```
  - 현재 사용하고 있는 포트
    ```terminal
    # netstat -tulpn
    ```

> # SSH 설치

- SSH 설치
  ```terminal
  # sudo apt-get install openssh-server
  # systemctl start ssh
  # systemctl enable ssh
  ```

- ROOT 계정 로그인 허용
  ```terminal
  # sudo vi /etc/ssh/sshd_config
  # systemctl restart ssh
  ```
  
  - `/etc/ssh/sshd_config` 수정 사항
    - `PermitRootLogin`으로 주석처리되어있는 약 34번 라인을 35번 라인과 같이 추가
    ```touch
    34  #PermitRootLogin prohibit-password
    35  PermitRootLogin yes
    ```

> # Java 1.6 설치
  - [Java 1.6 다운로드 받기](https://www.oracle.com/java/technologies/javase-java-archive-javase6-downloads.html)
  - Java 설치
    ```
    # sudo chmod +x jdk-6u45-linux-x64.bin
    # ./jdk-6u45-linux-x64.bin
    # sudo mv ./jdk1.6.0_45/ /usr/lib/jvm/
    # sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.6.0_45/bin/java 1
    # sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.6.0_45/bin/javac 1
    # sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.6.0_45/bin/javaws 1
    # sudo update-alternatives --config java
    # sudo update-alternatives --config javac
    # sudo update-alternatives --config javaws
    # java -version
    ```
    - [alternatives란?](https://skyoo2003.github.io/post/2017/03/17/what-is-alternatives-command)
      - 심볼릭 링크를 생성/제거/관리/조회 기능을 제공
      - 옵션
        - '--install' : 심볼릭 링크 생성
        - '--remove' : 심볼릭 링크 제거
        - '--auto' : 심볼릭 링크 자동화
       

https://seongjin.me/docker-swarm-introduction-nodes/
