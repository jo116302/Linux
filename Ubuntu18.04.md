> Linux Version 확인
  ```
  # lsb_release -a
  ```

> useradd & adduser & groupadd
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

> 시간 동기화
  - 현재 Date 확인
    ```
    # date
    ```
  - 시간 동기화
    ```
    # ls /usr/share/zoneinfo/
    # sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
    ```

> X Window 설치
  - 업데이트 필요
    ```
    # sudo apt-get upgrade
    # sudo apt-get update
    ```
  - X Window 설치
    ```
    // X Window 요소 모두 설치
    # sudo apt-get install ubuntu-desktop
    // X Window 요소 최소한의 설치
    # sudo apt-get install --no-install-recommends ubuntu-desktop
    // X Window 
    # startx
    ```
  - XRDP 설치[참고 링크](https://marcokhan.tistory.com/248)
    ```
    # sudo apt-get install xrdp
    # sudo apt-get install xorgxrdp
    # sudo apt-get install xorg-video-abi-23
    ```

> firewall
  - 방화벽 상태 확인
    ```
    # sudo ufw status verbose
    ```
  - 방화벽 활성화 및 비활성화
    ```
    // 방화벽 활성화
    # sudo ufw enable 
    // 방화벽 비활성화
    # sudo ufw disable
    ```
  - 방화벽 규칙 확인
    ```
    # sudo ufw show raw 
    ```
  - 방화벽 규칙 변경
    ```
    // 특정 PORT 통신 허용
    # sudo ufw allow [PORT]/[TCP/UDP]
    // 특정 PORT 통신 거부
    # sudo ufw deny [PORT]
    ```
