> useradd & adduser

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
