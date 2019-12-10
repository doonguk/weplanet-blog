
## User

### id, who

유닉스는 다중사용자 시스템

```bash
$ id #identify , 식별하다
```

<img width="1440" alt="스크린샷 2019-11-16 오후 1 33 18" src="https://user-images.githubusercontent.com/39187116/68988109-b5a66c00-0875-11ea-85c8-6536a444a5c9.png">

uid는 user의 id, gid는 group의 id 즉 내가 staff라는 group에 속해 있다는 뜻

```bash
$ who # 현재 이 시스템에 누가 접속해 있는지 알 수 있음.
```

<img width="517" alt="스크린샷 2019-11-16 오후 1 41 05" src="https://user-images.githubusercontent.com/39187116/68988173-c4415300-0876-11ea-9f02-e48551f5e4c4.png">

현재 내 맥북에는 hong 이라는 유저만 접속 해 있다.

### 관리자와 일반 사용자 ( super user(root) vs user )

<img width="311" alt="스크린샷 2019-11-16 오후 1 46 14" src="https://user-images.githubusercontent.com/39187116/68988245-7d079200-0877-11ea-9c26-44bf68a98816.png">

- Shell에서 $는 일반사용자를 나타내고 #은 관리자( root) 를 나타낸다.

- <code>su [option] [username]</code> : 사용자를 변경하거나( A사용자 -> B사용자 ), 관리자로 변경하고 싶을때 사용
  - <code>su - root</code>: 관리자로 변경 ( 초기 비밀번호를 설정하지 않았다면 <code>sudo -s</code>  로 설정 )
  - 웬만한 작업은 <code>sudo</code> 를 사용하자.
- 몇몇 OS는 root 사용자에 대한 권한을 막아놨음. unlock하는 방법은?

```bash
$ sudo passwd -u root # root를 unlock
```

- 반대로 OS의 root 사용자를 lock 하는 방법은 ?

```bash
$sudo passwd -l root # root를 lockd
```

<code>l</code> 옵션을 이용해서 root 사용자 계정에 접근 하는 것을 막을 수 있다. ( *Mac에서는 passwd 에 대한 옵션이 일반 unix 계열 OS 와 다르다..* )

- 관리자와 일반 사용자의 home directory

```bash
$ sudo - root
$ cd ~ # 현재 사용자의 home directory로 이동
$ pwd
/root # 일반 unix 계열 OS는 /root(root 디렉토리의 root 디렉토리.. /+ root ), Mac은 /var/root
$ exit # 일반 사용자로 전환!
$ cd ~
$ pwd
/home/hong # 일반 unix 계열 OS는 /home/[user] 로 존재, Mac은 /Users/[user]로 존재
```



### 사용자 추가하기

- <code>sudo useradd -m [name]</code> : name 이라는 사용자 추가, -m은 home directory도 같이 만들어줌
  - Mac은 일반 unix 계열과 다르다. https://apple.stackexchange.com/questions/4814/can-user-accounts-be-managed-via-the-command-line/4981#4981 를 참고 하면된다.
- <code>su - [username]</code> : 입력한 username으로 사용자 변경

```bash
$ sudo useradd -m donguk
$ sudo passwd donguk # 추가한 유저 초기 비밀번호 설정
...
$ su - donguk # donguk으로 사용자 변경
```

- 사용자에 sudo 권한 사용할 수 있게 하기

```bash
$ sudo usermod -a -G sudo [username] # sudo 권한을 사용할 수 있는 유저에서 입력해야 한다.
```

<code>- a (append)</code> 옵션은 어떤 그룹에 사용자를 추가 하는 옵션, 이 옵션은 <code>-G(groups)</code> 옵션과 함께 사용해야 한다. <code>-G</code> 어떤 그룹이라는 개념에 사용자를 grouping 하는 옵션. 즉 sudo 를 사용하는 그룹에 유저를 추가한다~ 느낌.

## Group

user(소유자) 도 other도 아닌 어떤 사용자들을 그룹화 해보자

```bash
# test1 user와 test2 user를 developer 그룹으로 묶는 예제

# /var 안에 developer를 만들고 이 폴더의 그룹 권한을 developer로 지정해보자
$ cd /var # 변화가 많은 파일들이 모여있는 디렉토리
$ mkdir developer
Permission denied # 1
$ sudo mkdir developer
$ cd developer
$ echo 'hi my group' > 'group.txt'
Permission denied # 2
$ groupadd developer
Permission denied
$ sudo !! # 3
sudo groupadd developer
$ nano /etc/group # 4
$ usermod -a -G developer test1 # 5
Permission denied
$ sudo !!
$ sudo usermod -a -G developer test2
$ exit 
$ ssh [address] # 다시 접속 해야 수정된 사항 반영!
$ cd /var/developer
$ sudo chown root:developer . # 6
$ echo 'hi my group' > 'group.txt'
Permission denied # 같은 그룹이지만.. 그룹에 쓰기 권한이 없다.
$ sudo chmod g+w . # 현재 디렉토리의 그룹 권한에 w 권한 부여
$ echo 'hi my group' > 'group.txt'
$ ls -al # group.txt 확인 가능
```

1. /var 폴더의 소유자는 

![image](https://user-images.githubusercontent.com/39187116/68993037-9419a480-08b6-11ea-8899-ba18bb93f149.png)

. 부분을 확인 해보면 root 이다. 따라서 /var 디렉토리에 mkdir 프로그램을 실행 하기 위해서는 access mode의 other부분에 쓰기(w) 권한이 있어야 한다.

첫번째 방법은 root 사용자로 로그인 ( <code>su - root</code>) 하여 chmod 명령어를 통해 디렉토리 권한을 바꿔주는 방법(<code>chmod o=rwx /var</code>)이 있지만, 보안상 으로도 좋지 않고 귀찮은 방법 이다. 따라서 현재 사용자에 <code>sudo</code> 프로그램을 이용하여 mkdir 프로그램을 실행하자

2. 1번과 같은 other에 속한 사용자는 w 권한이 없어서 파일을 생성 할 수 없다. 하지만 이 폴더의 그룹을 developer로 변경 후 권한을 부여 한다면 developer에 속한 사용자는 /var/developer 폴더 안에서 자유롭게 읽고 쓰고 실행 하는 권한을 가질 수 있다.
3. <code>groupadd [group name]</code> 을 이용하여 그룹을 추가 하려고 하였지만, 이 명령어는 일반 사용자의 권한으로는 실행 할 수 없다.  sudo 권한을 이용해야 한다. **!! 를 이용하면 이전 명령어를 수행하게 할 수 있다.**
4. <code>/etc/group</code> 파일은 unix 계열의 시스템에서 그룹에 관한 정보를 담고 있는 파일이다. 여기서 그룹 정보를 확인 할 수 있다.
5. <code>usermod</code> 는 user modify의 약자이다. <code>usermod</code> 는 -a ( append ) -G (group) 옵션과 함께 사용자를 그룹에 지정할 수 있다.
6. <code>chown [owner]:[group-name] [directory]</code> : chown  프로그램은 현재 디렉토리의 소유자와 그룹을 변경 할 수 있다. 위 에서는 developer 디렉토리의 group을 developer로 변경한 것 이다.
