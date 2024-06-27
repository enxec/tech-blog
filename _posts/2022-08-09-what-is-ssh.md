---
title: "[Linux] SSH개념과 SSH서버 설정"
description: 
author: Enxec
date: 2022-08-09
categories: [Linux]
tags: [linux, 리눅스, SSH, SSH 설정, SSH 명령어]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/linux-logo.png
  lqip: 
  alt: 
---

## SSH의 개념
---
SSH는 원격 시스템에 로그인하여 명령을 실행하는 프로그램으로 기본적인 사용법은 telnet과 유사하다. telnet은 클라이언트와 서버간의 데이터 전송 시에 일반 텍스트 형태로 패킷을 전달하여 패킷의 내용을 유출당할 수 있지만, SSH는 패킷 전송 시 암호화시키므로 안전하게 전송할 수 있다. SSH는 SSH1과 SSH2 두가지 버전으로 나뉘는데, SSH2는 SSH1을 개선한 것으로 호환은 되지만 기본적으로 호환성을 포함하고 있는 것이 아니므로 SSH1을 지원하려면 SSH1서버도 같이 운영해야 한다. SSH2는 이중-암호화 RSA 키 교환을 비롯하여 다양한 키-교환 방법을 지원한다.

<br>

## SSH 특징
---
- 패킷을 암호화하여 다른 원격 로그인 프로그램인 telnet이나 rlogin에 비해 안전하다.
- rlogin처럼 패스워드 입력 없이 로그인이 가능하다.
- rsh처럼 원격 셸을 지원한다.
- 원격 복사(scp)를 지원한다.
- 안전한 파일 전송(sftp)을 지원한다.

<br>

## SSH 설치 패키지
---
리눅스에서는 SSH의 공개 버전인 openssh를 사용하는데, SSH 서버와 SSH 클라이언트 프로그램으로 나누어져 있다. 레드햇 계얼 리눅스에서는 3개의 패키지로 구분해서 배포하고 있다.

<table style="text-align: center;">
    <th style="width: 150px; text-align: center; background-color: #51559D">패키지명</th>
    <th style="text-align: center; background-color: #51555D;">설명</th>
    <tr>
        <td>openssh</td>
        <td style="text-align: left;">openssh 서버와 openssh 클라이언트에 모두 필요한 핵심적인 파일들이 들어 있는 패키지로 ssh-keygen 명령이 들어 있다.</td>
    </tr>
    <tr>
        <td>openssh-server</td>
        <td style="text-align: left;">openssh 서버 관련 패키지로 데몬인 sshd, sftp 서버 등이 들어 있다.</td>
    </tr>
    <tr>
        <td>openssh-clients</td>
        <td style="text-align: left;">openssh 클라이언트 관련 패키지로 ssh, scp, sftp 등의 명령어가 들어 있다.</td>
    </tr>
</table>

<br>

## SSH 서버 설정
---
SSH 서버의 환경 설정 파일은 /etc/ssh/sshd_config이고 실행 명령은 'systemctl start sshd.service' 이다. sshd_config 파일의 주요 항목을 분석해보면 다음과 같다.
- ```bash
  Port 22
  ```
  >SSH 서버가 사용하는 포트를 지정한다.
- ```bash
  AddressFamily any
  ``` 
  >접속하는 IP 주소 버전을 지정하는 옵션으로 any는 IPv4와 IPv6 주소 체계 모두 접속이 가능하다. 앞에 있는 주석을 제거하고 any 대신에 inet이라고 설정하면 IPv4 주소만 접속 가능하고 inet6라고 설정하면 IPv6 주소만 접속이 가능하다.
- ```bash
  ListenAddress 0.0.0.0
  ListenAddress ::
  ```
  >sshd 데몬이 허가하는 주소를 설정하는 영역으로 여러 개의 네트워크 인터페이스가 있을 경우 IP대역과 포트를 지정할 수 있다. 'ListenAddress 192.168.1.1:22' 형태로 지정하면 되고, 콜론(:)은 IPv6 주소체계에서 사용하면 된다.
- ```bash
  Protocol 2
  ```
  >해당 항목은 SSH 프로토콜 버전을 지정하는 것으로 최초의 파일에 작성되어있지 않은 내용이다. 추가하면 적용된다.
- ```bash
  HostKey /etc/ssh/ssh_host_key
  ```
  >SSH1의 호스트키 위치를 지정한다.
- ```bash
  HostKey /etc/ssh/ssh_host_rsa_key
  HostKey /etc/ssh/ssh_host_dsa_key
  HostKey /etc/ssh/ssh_host_ecdsa_key
  HostKey /etc/ssh/ssh_host_ed25519_key
  ```
  >SSH2의 암호화 방식인 RSA, DSA 등의 호스트키 위치를 지정한다.
- ```bash
  KeyRegenerationInterval 1h
  ```
  >서버의 키는 한번 접속이 이루어진 뒤에 자동적으로 다시 만들어진다. 다시 만드는 목적은 나중에 호스트의 세션에 있는 키를 캡처해서 암호를 해독하거나 훔친 키를 사용하지 못하도록 하기 위함이다. 값이 0이면 키는 다시 만들어지지 않는다는 의미이고, 기본 단위는 초 단위이다. m은 분(minute), h는 시(hour)를 뜻한다.
- ```bash
  ServerKeyBits 1024
  ```
  >서버 키의 비트수를 정의한다.
- ```bash
  SyslogFacility AUTHPRIV
  ```
  >syslog 관련하여 facility를 지정하는 항목으로 SSH를 통한 접속은 AUTHPRIV로 처리하여 기록한다.
- ```bash
  LogLevel INFO
  ```
  >로그레벨을 지정 항목으로 기본값은 INFO이다. 그 외의 값으로 QUIET(기록하지 않음), FATAL(치명적인 오류), ERROR, VERBOSE, DEBUGS 등이 있다.
- ```bash
  LoginGraceTime 2m
  ```
  >유저가 로그인에 실패했을 경우 서버가 연결을 끊는 시간이다. 값이 0이면 제한이 없다.
- ```bash
  PermitRootLogin yes
  ```
  >root 로그인 허용 여부를 결정하는 것이다. yes는 root로 접속이 가능하다는 의미로 보안을 강화하기 위해서는 no로 바꾼다.
- ```bash
  StrictModes yes
  ```
  >로그인을 허용하기 전에 파일 모드, 사용자 홈 디렉토리 소유권, 원격 호스트의 파일들을 SSH 데몬이 체크할 수 있도로고 해주는 설정이다.
- ```bash
  MaxAuthTries 6
  ```
  >접속할 때 재시도할 수 있는 최대횟수를 지정하는 설정이다.
- ```bash
  MaxSessions 10
  ```
  >네트워크 주소 당 최대 연결할 수 있는 세션수를 지정한다.
- ```bash
  RSAAuthentication yes
  ```
  >RSA 인증을 사용할 것인지를 설정한다.
- ```bash
  PubkeyAuthentication yes
  ```
  >공개키를 통한 인증을 허가할 것인지를 설정한다.
- ```bash
  AuthorizedKeysFile      .ssh/authorized_keys
  ```
  >클라이언트에서 생성한 공개키를 저장할 파일명을 설정한다.
- ```bash
  RhostsRSAAuthentication no
  ```
  >프로토콜 1 기반 RSA를 이용할 때 rhost나 /etc/hosts.equiv 파일이 있으면 이것을 사용해 인증을 할 수 있도록 하는 항목으로 기본값은 no이다.
- ```bash
  HostbasedAuthentication no
  ```
  >rhost나 /etc/hosts.equiv를 이용하여 접속할 때 공개키를 이용하여 접속을 허가할 것인지를 설정할 때 사용한다. 기본값은 no이다.
- ```bash
  IgnoreUserKnownHosts yes
  ```
  >RhostsRSAAuthentication이나 HostbasedAuthentication을 사용한 인증 시에 각 사용자의 홈 디렉토리에 있는 .ssh/known_hosts 무시할 것인지를 설정한다. 기본값은 no이다.
- ```bash
  IgnoreRhosts yes
  ```
  >RhostsRSAAuthentication이나 HostbasedAuthentication을 사용한 인증 시에 .rhosts 파일을 무시할 것인지를 설정한다. 기본값인 yes는 .rhosts 파일을 무시한다.
- ```bash
  PasswordAuthentication yes
  ```
  >패스워드를 이용한 인증을 허용한다. 기본값이 yes이다.
- ```bash
  PermitEmptyPasswords no
  ```
  >패스워드를 인증할 때, 서버가 비어있는 패스워드를 인정할 것인가에 관한 설정이다. 기본값은 no이다.
- ```bash
  ChallengeResponseAuthentication no
  ```
  >Challenge-Response 인증 관련 설정이다.
- ```bash
  KerberosAuthentication no
  KerberosOrlocalPasswd yes
  KerberosTicketCleanup yes
  KerberosGetAFSToken no
  KerberosUseKuserok yes
  ```
  >Kerberos 인증 관련 설정이다.
- ```bash
  GSSAPIAuthentication yes
  GSSAPICleanupCredentials yes
  ```
  >GSSAPI 인증 관련 설정이다.
- ```bash
  UsePAM yes
  ```
  >PAM을 통해 제어할 것인지를 설정한다.
- ```bash
  AllowTcpForwarding yes
  ```
  >TCP를 통한 포워딩을 허가한 것인지를 설정한다.
- ```bash
  GatewayPorts no
  ```
  >포워딩된 포트에 대하여 클라이언트의 접속을 허가할 것인지를 설정한다.
- ```bash
  X11Forwarding yes
  ```
  >원격에서 X11 포워딩을 허용하는 것이다. 이 옵션을 yes로 설정하면 xhost보다 안전한 방법으로 원격에 있는 X 프로그램을 사용할 수 있다.
- ```bash
  X11DisplayOffset 10
  ```
  >X11 포워딩이 될 때 디스플레이 번호를 지정해준다.
- ```bash
  X11UseLocalhost yes
  ```
  >X11 포워딩 서버에 접속할 때 localhost 관련 설정이다.
- ```bash
  PrintMotd yes
  ```
  >SSH 로그인 시에 /etc/motd 파일의 내용을 프린트되도록 설정한다.
- ```bash
  PrintLastLog yes
  ```
  >로그인 시 지난번 로그인 기록을 보여줄 것인지를 설정한다.
- ```bash
  TCPKeepAlive yes
  ```
  >클라이언트의 접속이 끊어졌는지를 체크하기 위해 일정 시간 메시지를 전달하도록 하는 설정이다.
- ```bash
  UseLogin no
  ```
  >로그인 세션을 사용할 것인지를 설정한다.
- ```bash
  UsePrivilegeSeparation yes
  ```
  >네트워크 트래픽 발생 시 권한이 없는 자식 프로세스에 의해 생성된 권한을 분리할 것인지를 설정한다.
- ```bash
  PermitUserEnvironment no
  ```
  >사용자 홈 디렉토리의 .ssh/environment나 .ssh/authorized_keys안의 environment= 옵션을 사용하게 할 것인지를 설정한다.
- ```bash
  Compression delayed
  ```
  >압축을 허가할 것인지를 설정한다.
- ```bash
  ClientAliveInterval 0
  ```
  >클라이언트로부터 전송되는 데이터가 없을 경우 서버는 메시지를 보내게 되는데, 이 때의 시간을 지정한다. 0이면 메시지를 보내지 않는다.
- ```bash
  ClientAliveCountMax 3
  ```
  >클라이언트에게 전달하는 메시지 횟수를 지정한다.
- ```bash
  UseDNS yes
  ```
  >클라이언트의 호스트 주소를 IP 주소로 해석할 것인지를 설정한다.
- ```bash
  PidFile /var/run/sshd.pid
  ```
  >SSH데몬의 PID를 기록하는 파일이다.
- ```bash
  Banner none
  ```
  >배너 관련으로 로그인 전에 보여주는 메시지 파일을 지정하는 부분이다. 예를 들면 텔넷은 /etc/issue 또는 /etc/issue.net 파일을 사용하는데, 이러한 파일을 지정하는 것이다.
- ```bash
  Subsystem     sftp    /usr/libexec/openssh/sftp-server
  ```
  >sftp는 프로토콜 버전 2에서 사용되는 것으로 ssh와 같이 ftp의 보안을 강화하기 위해 사용되는 보안 ftp 프로그램이다. sftp서버를 사용하기 위한 항목이다.


<br>

## 관련 명령어
### SSH 서버 접속
리눅스에서는 openssh-clients 패키지를 설치하면 SSH 명령을 이용해서 접속할 수 있고, 윈도우에는 SecureCRT, Xshell, putty 등과 같은 프로그램을 이용해야 한다.

- 사용법
  - ssh [option] 호스트명 or IP 주소
    >SSH는 서버로 접속할 때에 같은 계정으로 접속을 시도하므로 클라이언트와 서버의 계정이 같으면 생략해도 된다.
  - ssh 계정이름@호스트네임
    >-l 옵션 대신에 사용하는 방법으로 클라이언트 계정과 서버의 계정이 다르면 계정 이름을 명시해야한다.
  - ssh 호스트네임 명령
    >rsh와 같이 원격 셀을 사용할 수 있는데, 접속 없이 명령만 내릴 때 유용하다.

- 주요 옵션
  - -l
    >다른 계정으로 접속할 때 사용한다. 이 옵션 대신 서버 주소 앞에 @를 붙여 사용할 수도 있다.
  - -p
    >ssh 서버의 포트번호가 22번이 아닌 경우 -p 옵션을 사용해서 바뀐 포트를 지정할 떄 사용한다.

- 사용 예

```bash
# 203.247.40.246 서버로 클라이언트의 계정과 같은 계정으로 
# 접속을 시도한다. 관련 키 설정을 위한 질문이 나오면 yes를
# 입력하고 원격지 서버의 패스워드를 입력하면 접속된다.
ssh 203.247.40.246
```
```bash
# 192.168.1.1 서버로 클라이언트의 계정과 다른 계정인 test로
# 접속을 시도한다.
ssh -l test 192.168.1.1
```
```bash
# 192.168.1.1 서버에 test라는 계정으로 접속을 시도한다.
ssh test@192.168.1.1
```
```bash
# 192.168.1.1 서버의 ssh 포트 번호가 180번인 경우에 사용한다.
ssh -p 180 192.168.1.1
```
```bash
# 원격에서 ezlc.org에 test라는 계정으로 접속하여 data라는
# 디렉토리를 생성한다.
ssh -l test ezlc.org mkdir data
```

### 인증키를 이용한 SSH 서버 접속
SSH 서버를 접속할 때 패스워드를 입력하여 접속하지만 인증키를 이용하여 접속할 수도 있다.
SSH 클라이언트에서 ssh-keygen 명령을 이용해서 비밀키와 공개키를 생성하고, SSH 서버에 공개키를 복사하면 된다.

![인증키를 이용한 SSH server 접속 가상 설명도](/posts/20220809/ssh-keygen-process.png "인증키를 이용한 SSH server 접속 가상 설명도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">인증키를 이용한 SSH server 접속 가상 설명도</div> 

- 사용법
  - ssh-keygen [option]

- 주요 옵션
  - -t
    >사용할 암호화 알고리즘을 지정하는 옵션으로 rsa, dsa등을 사용할 수 있다. ssh2 버전에는 지정하지 않으면 rsa를 사용한다.

- 사용 예

```bash
# DSA를 이용해서 인증키를 생성한다.
ssh-keygen -t dsa
```
```bash
# RSA를 이용해서 인증키를 생성한다. id_rsa와 id_rsa.pub 두개의 
# 파일이 생성되며, 패스워드를 별도로 설정하지 않으면 서버에 접속
# 할 때 패스워드 없이 로그인이 가능하다. 생성된 파일 중에 공개키
# 파일에 해당하는 id_rsa.pub 파일을 원격지 서버의 홈 디렉토리 안에 
# .ssh/authorized_keys로 복사해두면 된다.

# RSA를 이용한 인증키 생성
ssh-keygen

# 원격지에 .ssh 디렉토리 생성
ssh 203.247.40.248 mkdir -m 700 .ssh

# scp를 활용해 공개키 복사
scp .ssh/id_rsa.pub 203.247.40.248:.ssh/authorized_keys
```

---

읽어주셔서 감사합니다. 😊

__Reference__  
CentOS7으로 리눅스 마스터 1급 정복하기 - 정성재 