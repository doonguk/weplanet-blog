## IO Redirection

<img width="506" alt="스크린샷 2019-11-09 오전 12 26 33" src="https://user-images.githubusercontent.com/39187116/68488114-b5134180-0287-11ea-8d1b-62849422f230.png">

- IO Redirection ( Input Ouput Redirection ) : input 과 output의 방향을 바꾼다

  - UNIX 계열의 시스템은 Standard Input( Command ) 이 주어지면 Unix process를 거쳐 Standard output을 내보냄
  - <code>ls -l 1> result.txt</code> : ls -l 의 결 과물을 result.txt에 저장. Standard ouput의 출력 방향을 바꿔주는 방법 ( <code>></code> 가 수행, 1은 생략 가능하다.  )

  - Standard error은 Standard Input이 process에서 오류가 발생하면 생김 
  - Standard error은 redirection 할 수 없다. 하지만 <code>2> </code> 를 사용 한다면 error도 redirection 가능하다.

  ```bash
  #현재 디렉토리에 test.txt가 없을때
  $ rm test.txt // 오류 발생
  $ rm test.txt > result.txt //오류가 result.txt에 저장되지 않음 ( Standard error를 redirection )
  $ rm test.txt > result.txt 2> result2.txt // result2.txt에 오류가 저장됌
  ```

  - <code>cat </code> : 파일 읽기 
  - <code>cat < hello.txt </code> 명령어가 Standard Input으로 파일을 받는다면 파일의 내용을 Standard Output으로 내보낸다  < 는 input을 redirection 한 것이다.

  ```bash
  $ head -n1 linux.txt // -n1 은 Command line Arguments, linux.txt는 Standard Input
  $ head -n1 < linux.txt //위와 같음
  $ head -n1 < linux.txt > result.txt //input redirection과 output redirection 모두 일어남
  //linux.txt 의 첫번째 행을 result.txt에 저장
  ```

  

- input 과 output 흘러 나가는 이런 형태를 **IO stream** 이라고 한다.

## 추가적인 기능

```bash
$ ls -al > result.txt // Standard output 을 result.txt에 저장
$ ls -al > result.txt // 결과가 result.txt에 덮어 씌워짐
$ ls -al >> result.txt // 결과가 result.txt에 누적됌 ( append )

$ ls -al > /dev/null //실행결과를 /dev/null로 redirection, 화면에도 파일에도 출력x, 쓰레기통 같은곳임
```

## Shell

<img width="490" alt="스크린샷 2019-11-09 오후 3 11 44" src="https://user-images.githubusercontent.com/39187116/68523950-5390b880-0303-11ea-9a27-045c2c85612e.png">

- harware - 컴퓨터의 기계적인 부분들 ( 메모리, 하드디스크 ,SSD, CPU 등등..)

- Kernel - 하드웨어를 제어하는 프로그램 ( 운영체제에서 코어역할을 한다.)
- Shell - 우리가 입력한 명령을 Kernel이 이해할수 있게 kernel에게 전달해줌. Kernel은 이를 하드웨어에 이해할 수 있게 전달한다. 

- <code>echo </code>$0 : 현재 사용중인 쉘 확인
- 결국 쉘은 사용자의 Standard input(키보드)을 받아서 커널이 이해할 수 있게 커널에 전달하고, 커널은 이를 하드웨어에 전달하여 처리하고, 이 결과를 쉘이 받아서 Standard output이나 Standard error로 사용자에게 전달하여 상호작용하는 소프트웨어 이다.

<br/>

## Shell Script

- <code>/bin directory</code> : 유닉스 계열의 컴퓨터에 기본적으로 존재하는 프로그램들이 위치한 directory

```bash
$ nano backup
...backup 파일 start
#!/bin/bash 
if ! [ -d bak ]; then //현재 디렉토리에 bak라는 directory가 존재하지 않는다면?
	mkdir bak
fi //if를 거꾸로 쓰면 조건문이 끝났다는 것을 컴퓨터에 알림
cp *.log bak // 현재 디렉토리의 log로 끝나는 파일들을 bak에 복사
```

- <code>#!/bin/bash</code> :  이 줄 밑에 작성된 코드들이 /bin/bash 라는 프로그램을 통해서 해석되어야 한다. 라는 약속 ( /bin/bash 는 example )
- <code>./[filename]</code> : 현재 경로의 파일 실행

<img width="473" alt="스크린샷 2019-11-09 오후 4 00 37" src="https://user-images.githubusercontent.com/39187116/68524432-1e3b9900-030a-11ea-9ce9-6f909a6b1334.png">

- 실행 권한이 없기 때문에 <code>'./backup' is not executable by this user (또는 Permisssion denied)</code> 라는 Standard error가 나왔다.(<code>ls -l</code> 로 나온 output을 보면 backup 파일에 x 권한이 없음.)  실행 권한을 주기 위해서는 <code>chmod ( change mode )</code> 명령어에 <code>+x (executable)</code> 권한을 추가 해줬다.