---
title: 생활코딩 리눅스 강좌(기초-1)
date: 2019-11-07 00:11:83
category: linux
---

명령어로 컴퓨터를 제어하는 방식을 **CLI ( Command Line Interface )** 라고 한다.

- <code>ls </code> 현재 디렉토리의 파일목록 확인 ( 자세하게 보기는 <code>ls -l</code>)
  - <code>ls -l</code> 했을때 맨 앞에 d 붙어 있으면 디렉토리, 아니면 파일
  - 유닉스나 리눅스에서는 .으로 시작하는 파일은 기본적으로 감춰진 파일, 확인 하려면? <code>ls -a</code>
  - .으로 시작하는 파일 출력하면서 자세하게 보려면 <code>ls -al</code> 또는 <code>ls -la</code> ( 옵션이 합쳐진 경우 순서 상관 없다.)
  - 위와 같이 옵션을 합쳐주면 된다.( 추가적으로 file size에 따른  sort 하고 싶으면 <code>ls -alS</code> )
- <code>pwd</code> : 현재 위치하고 있는 디렉토리 확인
  - 현재 위치가 / 경우 root 디렉토리
- <code>mkdir [name]</code> : ( 현재 위치에서 ) 디렉토리 만들기
  - <code>mkdir -p [name/name/name]</code> : 필요한 부모 디렉토리를 만들면서 자식 디렉토리를 만듬 ( 여러개 한번에 만듬 )
- <code>touch [name]</code> : 비어있는 파일 만들기 
- <code>cd [directory name]</code> : change directory의 약자, 디렉토리 이동에 쓰임
  - <code>cd ..</code> : 현재 디렉토리의 부모 디렉토리로 이동 ( 자신의 위치에 따라서 상대적으로 이동이 달라짐 )
  - <code>cd [절대경로]</code> : 어디에 위치하든 해당 절대경로로 이동
- <code>rm [file name]</code> : 파일 삭제
  - 디렉토리를 삭제하고 싶다면? <code>rm -r [directory name]</code> : <code>[directory name]</code> 하위에 있는 모든 파일, 폴더 삭제 ( recursive)
- 명령어 <code>-- help</code> : 해당 명령어의 사용법을 볼 수 있다.
- <code>man [궁금한 명령어]</code> : 해당 명령어의 사용법을 알 수 있다. ( <code>help</code> 보다 조금 상세  )
  - 위아래 화살표 키로 스크롤 조절 가능
  - <code>/[명령어]</code> : 윈도우의 컨트롤 + F 기능, 검색 이후 키보드 <code>n</code> 을 이용하여 네비게이션 가능
  
  - <code>cp [source] [target dir]</code> : source 파일을 target dir에 복사
  - <code>ls -l [dir name]</code> : <code> [dir name]</code> 안에 있는 파일 정보 확인
  
  - <code>mv [source] [target dir]</code> : source 파일을 target dir로 이동시킨다. 
    - <code>[target dir]</code> 와 현재위치가 같다면 파일이름을 **변경** 시킨다. 
  - <code>sudo</code> : ( super user do ) 관리자 권한 > Super User or Root User
  - <code>nano</code> : (초급자?) 편집기
    - ^ 기호는 <code>Ctrl</code> 을 의미
    - <code>Cut text, UnCut text</code>  : 잘라내기, 붙여내기 기능
  - homebew : 프로그래머들이 사용하는 앱스토어 느낌( 명령을 통해서 행위를 하는 프로그램을 다룸), 패키지 매니저
    - <code>htop</code> : 컴퓨터에서 사용되고 있는 자원을 확인해주는 패키지 
    - <code>brew upgrade</code> : 파라미터로 패키지 명을 받으면 해당 패키지 최신버전으로 업그레이드, 파라미터 없다면 모두 업그레이드
    - <code>brew update</code> : 다운 받는 패키지들을 최신 버전으로 받을 수 있게 해줌 ( 패키지 다운로드 전에 해주자! ) 
  
  - <code>wget [file url] </code> : 파일 다운로드 기능, <code>-O</code> 옵션으로 이름 붙이기 가능
  
  - <code>git clone [open source url] [지정할 파일명]</code> : open source clone