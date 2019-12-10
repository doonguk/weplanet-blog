### Daemon 의 개념

- 데몬에 해당하는 프로그램은 **항상**  켜져 있음.
- <code>ls, mkdir, cd</code> 이런 프로그램들은 데몬이 아님.
- 소위 server ( 예를들어 web server ) 라고 불리는 프로그램은 데몬에 해당한다.
- 이런 데몬에 해당하는 프로그램을 service 라고도 부른다.



### Daemon의 예시

```bash
# Mac Os 기준, mac system dameon이 위치한 directory는 System/Library/LaunchDaemons
# third-party daemon이 위치한 directory는 /Library/LaunchDaemons
# 밑에 있는 예시는 ubuntu 기준

$ sudo apt-get install apache2 # web server apache 설치
$ cd /etc/init.d # service(데몬의 목적을 가진) 프로그램들이 위치하는 디렉토리
$ sudo service apache2 start # apache start, service 프로그램은 실행 시키려면 'service' 사용
$ ps aux | grep apache2 # 현재 실행된 프로세스 중 apache2 포함된 프로세스 확인
$ sudo service apache2 stop # apache2 종료, service로 실행되는 프로그램은 대부분 start, stop이 있음
```



- 컴퓨터가 부팅될 때 자동으로 Start 시키려면?

```bash
$ cd /etc/rc3.d # CLI 방식으로 실행된 데몬들 있는 디렉토리 , GUI 방식으로 실행된 데몬은 rc5.d
$ ls -l 
```

<img width="972" alt="스크린샷 2019-11-14 오전 2 20 20" src="https://user-images.githubusercontent.com/39187116/68788374-9ac1d500-0686-11ea-9f1d-5cd7d57d648a.png">

맨 왼쪽에 <code>l </code> 이 의미 하는건 링크 이다. SO2apache2 는 윈도우의 바로가기와 비슷하다.(<code>./S02apache2</code> 커맨드로 실행 가능 ) -> 오른쪽에 있는 경로가 service가 실제로 위치한 디렉토리를 의미한다. 여기서 S02의 S가 의미하는 것은 CLI 방식으로 컴퓨터가 재부팅 될 때 자동으로 실행 된다는 뜻 이다.  그리고 01, 02, 03은 자동으로 시작되는 service들 사이의 우선순위 이다.  반대로 S 대신에 K가 붙은 service도 있는데 이 service는 컴퓨터가 재부팅 될 때 종료 된다.

따라서 **CLI 방식으로 재부팅 될 때 프로세스가 자동으로 실행되게 하고 싶다면 <code>rc3.d</code> 디렉토리 밑에 S로 시작하는 이름으로 링크를 걸면 된다.**


## CRON

#### CRON 기본 명령어

- 정기적으로 명령을 실행시켜주는 소프트웨어 ( ex 백업, 정기적인 데이터 전송, 정기적으로 인터넷으로 시간조정? )
- <code>crontab -e </code> : 정기적으로 실행 시키고 싶은 작업을 설정 할 수 있음. 

```bash
$ #m h dom(day of month) mon(month) dow(요일) command
$ */1 * * * * date >> date.log # 1분에 한번 시간, dom, mon, dow 는 무시, 만약 10 1 24 이라면 -> 매달 $ 24일 1시10분
```

- <code>crontab -l</code> : 처리한 cron 들을 확인 할 수 있다.
- <code>crontab -r </code> : cron 삭제

#### CRON 주기 설정

```bash
$ *        *         *        *         *
$ 분(0~59) 시간(0~23) 일(1~31)  월(1~12)   요일(0~6) # 일요일은 0, 1부터 월요일~
$ * * * * * ls -l # ls -l 을 매분 실행
$ 0,20,40 * * * * ls -l # 매월매일매시간 0분,20분,40분에  ls-l 실행
```

- <code>tail [file]</code> :  file의 꼬리 부분을 확인 할 수 있다.
  - **<code>tail -f [file]</code>** : file의 꼬리부분을 주시하다가 업데이트 되면 자동으로 화면을 업데이트 시켜준다.

```bash
$ *1 * * * * date >> date.log 2>&1 # 표준 에러를 표준 출력화
```

**출력에서 숫자 2는 '표준 에러' 이고 숫자 1은 '표준 출력'이다.** 위 코드는 표준 에러를 표준 출력으로 Redirection 한 것 이다. ( 그냥 1만 하면 1이라는 파일에 저장된다. &를 붙여줘야 한다! ) 

#### 적용 사례

회사에서 매일 아침에 서버의 tmp 폴더와 서버 프로젝트의 이미지 폴더를 지워야 하는 일이 있었는데, Shell sript를 이용하여 tmp, 이미지 폴더를 비워주는 sh 파일을 만들고 이를 크론으로 주기적으로 실행 하였다.

```bash
$ crontab -e
### crontab
$ 0 8 * * * /home/ubuntu/clearTemp.sh 2> error.log # 매일 아침 8시마다 자동으로 비워주기 오류있으면 error.log 에 로깅
```

추가적으로 Mac 에서 1분 마다 echo 'crontab test' 명령어를 수행하게 하였는데, 쉘에 바로 나타나지 않고 /var/mail/[사용자] 디렉토리에 로깅 된다는 것을 알았다.


<br/>

## 쉘을 시작할 때 실행

- <code>alias [별명] = '[command]'</code> :  command에 단축키(별명?)을 붙여준다.

```bash
$ alias ..='cd ..' # .. 는 cd .. 기능을 수행한다.
$ alias c='clear' # c는 clear 기능을 수행.
```

- <code>.bashrc</code> : 사용자 루트 디렉토리 경로에 있는  <code>.bashrc</code> 를 수정하면 쉘 ( bash )이 실행 될 때 무언가를 수행하게 할 수 있다.
  - bash shell은 쉘이 실행 될 때 <code>.bashrc</code> 를 실행하기 때문.
- <code>.bashrc</code> 에 alias 프로그램으로 커맨드를 단축어로 지정해 놓으면 편하다!
- fish shell 은 조금 다르다.
```bash
$ cd ~/.config/fish
$ vi config.fish # 이곳에 추가 해주면 된다.

