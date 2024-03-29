> 컴퓨터 구조, 프로세스 모니터링, 백그라운드 실행

### 컴퓨터 구조

- Storage
  - SSD, HDD 같은 저장공간을 의미
  - 가격이 낮고, 용량이 크지만 **저장하고 읽는데 시간이 오래걸린다**
- Memory
  - RAM
  - Storage와 반대의 특성을 지닌다. ( 가격이 높고, 용량 작지만 **저장하고 읽는데 소요되는 시간이 적음**)

- Processor ( * Process 아님 )
  - CPU ( 중앙처리장치 )
  - 동작속도가 빠름 ( Storage의 저장하고 읽는 속도보다 빨라~ )
  - 우리가 프로그램을 실행 시킨다는건 Storage에 저장되어 있는 프로그램을 읽어서 Memory에 적재시킨다. 그리고 이를 CPU가 읽어서 실행 시키는 것이다.
  - 또 다른 예시로, <code>mkdir</code> 같은 Command는 /bin 디렉토리(  Storage ) 에 파일로 저장 되어있다. ( 파일로 저장된 그것? 을 프로그램이라고 한다. ) 그리고 사용자가 Standard input과 함께 <code>mkdir</code> 프로그램을 실행 시키면, 이는 메모리에 적재 되고 CPU ( Processor )가 실행 시키게 된다. 추가적으로 **CPU에 처리되고 있는 상태. 즉 실행중인 프로그램을 process라고 한다.**



### 프로세스 모니터링

- <code>ps</code> : 현재 컴퓨터에 실행중인 프로세스 확인

  추가적으로 <code>aux</code> 옵션을 준다면 백그라운드에 실행중인 프로세스 까지 확인 가능하다.

  - 특정 단어의 프로세스만 확인하고 싶다 ? <code>ps aux | grep apache</code> 이와 같이 파이프 ( | )를 설정하여 원하는 프로세스 확인 가능하다. 
  - 특정 프로스를 확인한 후 <code>kill [pid]</code> 를 이용하여 프로세스를 죽일 수 있다.

- <code>top</code> : ps 와 비슷하게 프로세스 리스트 확인

- <code>htop</code> : <code>top</code> 프로그램의 상위호환 ( homebrew 를 통하여 설치 가능 )

  - MEM : 물리적인 메모리 사용량 % 확인
  - RES : 실제적인 메모리 사용량 ( NOT % ) 확인

  <img width="1397" alt="스크린샷 2019-11-13 오후 1 33 32" src="https://user-images.githubusercontent.com/39187116/68733512-3ff29400-061a-11ea-985a-5f187573ccc0.png">

  - 왼쪽 상단에 1,2,3,4 는 CPU 각각을 의미함 (4개의 **코어**)
  - 오른쪽 상단 Load average의 첫번째 숫자  : 위 사진에서는 2.75인데 이 의미는 4개의 코어중 약 2.75개의 코어가 사용되고 있고 나머지는 놀고(?) 있다는 뜻 이다.

<br/>

### 백그라운드 실행

예를들어 우리가 인터넷 서핑을 하다가 , 한글 문서를 클릭하면  ? 인터넷 브라우저는 백그라운드로 실행 되고, 한글 문서가 포워그라운드로 실행된다.

- <code>ctrl + z</code> : 프로세스 백그라운드 실행
- <code>fg</code> : 백그라운드로 작업하던 프로세스 다시 실행
- <code>jobs</code> : 현재 백그라운드로 존재하는 프로세스 목록 확인

<img width="374" alt="스크린샷 2019-11-13 오후 1 50 43" src="https://user-images.githubusercontent.com/39187116/68734151-9e207680-061c-11ea-9a5f-b3767cc3f747.png">

- 2 번 job으로 돌아가고 싶으면 <code>fg %2</code> ( test2 )
- n번 job으로 돌아가고 싶으면 <code>fg %n</code>
- n번 job을 제거하고 싶으면 <code>kill %n</code> ( 정상적인 종료에 해당 ) > 강제로 제거 하고 싶다면? <code>kill -9 %n</code>

- <code>&</code> : 특정 프로그램을 시작과 동시에 백그라운드로 실행 시킨다.
  - 예시 : <code>ls -R / > result.txt 2> error.log &</code>
  - 오래걸리는 작업을 백그라운드로 실행 시키고 작업을 이어갈수 있는 장점이 있다.