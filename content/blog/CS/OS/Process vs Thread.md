---
title: '[CS] Process vs Thread' 
date: 2020-10-17 18:10:36
category: 'CS'
draft: false
---


### 프로세스: 실행 중인 프로그램

프로그램을 실행하면 프로세스는 프로그램에 기록된 작업을 수행한다.
메모리 공간에 프로세스가 올라가게 되고 프로세스는 크게 4가지 섹션으로 나눌 수 있다.

프로세스의 4가지 섹션
- 스택: 메서드, 함수 매개변수, 반환 주소, local 변수 등 임시 데이터 포함
- 힙: 런타임에 프로세스에 동적으로 할당된 메모리 영역 
- 텍스트: 프로그램 카운터(현재 활동)와 프로세서 레지스터의 내용 포함
- 데이터: global, static 변수 포함 

스택에서는 local 변수, 데이터섹션 에서는 global, static 변수를 포함한다고하는데 이 차이를 알아보자

#####  local 변수, global ,static 변수 차이

2가지 관점에서 3가지 변수의 차이를 알아보자.

- scope: 접근 가능한 영역
- stoarge duration: 변수가 만들어지고 사라지는 시점

###### local 변수
같은 블록 또는 하위 scope에서 접근 가능, 코드의 실행영역이 해당 scope를 벗어나면 local 변수는 사라진다.

###### global 변수
최 상단 scope에서 정의 되었기 때문에 모든 scope에서 접근 가능하다. scope가 최 상단이기 때문에 프로그램이 끝날때 까지 사라지지 않는다.

###### static 변수
같은 블록 또는 하위 scope에서 접근가능, 프로그램이 끝날때 까지 사라지지 않는다.

[local vs global vs static](https://stackoverflow.com/questions/13415321/difference-between-static-auto-global-and-local-variable-in-the-context-of-c-a)

###### 레지스터

레지스터는 메모리 계층의 최상위에 위치하며, 가장 빠른 속도로 접근 가능한 메모리이다. 

대부분의 현대 프로세서는 메인 메모리에서 레지스터로 데이터를 옮겨와 데이터를 처리한 후 그 내용을 다시 레지스터에서 메인 메모리로 저장하는 로드-스토어 설계를 사용한다.

###### 프로그램 카운터
프로그램 카운터는 CPU 내부에 있는 레지스터 중의 하나로서, 다음에 실행될 명령어의 주소를 가지고 있어 실행할 기계어 코드의 위치를 지정한다. 

### 프로세스 제어 블록(PCB): 프로세스들을 관리하는 운영체제의 데이터 구조


프로세스의 상태, 권한, ID, CPU 레지스터, 스케줄링 정보 등등의 정보를 저장하고 프로세스 전환에 사용된다.

[tutorialspoint 프로세스](http://tutorialspoint.com/operating_system/os_processes.htm#:~:text=Advertisements-,Process,be%20implemented%20in%20the%20system.)

### 스레드: 프로세스의 실행 단위
스레드는 하나의 프로세스 내에서 자원을 공유한다.
다음에 실행할 명령어를 갖는 프로그램 카운터, 현재 작업 변수를 보유하는 시스템 레지스터 , 실행 내역을 포함하는 스택으로 구성되어 있다.

또한 멀티 스레딩을 통해 독립적인 작업 수행으로 수행능력을 향상시킬 수 있다.

[tutorialspoint 스레드](https://www.tutorialspoint.com/operating_system/os_multi_threading.htm#:~:text=A%20thread%20is%20a%20flow,which%20contains%20the%20execution%20history.&text=Each%20thread%20represents%20a%20separate%20flow%20of%20control.)
