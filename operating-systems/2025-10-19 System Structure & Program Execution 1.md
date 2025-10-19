## 컴퓨터 시스템 구조
<img width="50%" height="50%" alt="Slide 4_3 - 1" src="https://github.com/user-attachments/assets/77d32867-115a-4061-8e3b-084ce22d3674" />
## Mode bit

- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치 필요 → mode bit
- 모드
    - 1 : 사용자 모드 - 사용자 프로그램 수행
        - I/O 접근, 운영체제 메모리 접근 등의 일 수행 X
    - 0 : 모니터 모드 : OS 코드 수행
        - (= 커널 모드, 시스템 모드)
- 즉, 보안을 해칠 수 있는 중요한 명령어(특권 명령)은 모니터 코드에서만 수행 가능
- Interrupt나 Exception 발생 시 하드웨어가 mode bit를 0으로 바꿈
- 사용자 프로그램에게 CPU를 넘기기 전에 mode bit를 1로 세팅

## Timer

- 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴
- 타이머는 매 클럭 틱 때마다 1씩 감소
- 타이머 값이 0이 되면 **타이머 인터럽트** 발생
- CPU를 특정 프로그램이 독점하는 것으로부터 보호

## Device Controller

### I/O device controller

- 해당 I/O 장치 유형을 관리하는 일종의 작은 CPU
- 제어정보를 위해 control register, staus register 가짐
- local buffer(일종의 data register)를 가짐
- I/O는 실제 device와 local buffer 사이에서 일어남
- Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림

## DMA Controller

- 메모리 접근이 가능함
    - 즉, CPU와 DMA controller 모두 메모리 접근 가능
    - 동시 접근 등의 상황 발생 시 memory controller가 순서 등을 관리해줌
- I/O에 의해 너무 많은 인터럽트가 발생하는 것을 막아줌
- I/O 과정
    - DMA Controller가 중간 중간의 로컬 버퍼 내용을 메모리로 복사함
    - 최종 완료 시에는 인터럽트를 CPU에 한 번 보냄

## 입출력(I/O)의 수행

- 모든 입출력 명령은 특권 명령
- 사용자프로그램은 어떻게 I/O를 할까?
- **시스템콜(System Call)**
    - 사용자 프로그램은 운영체제에게 I/O 요청
    - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
    - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    - 올바른 I/O 요청인지 확인 후 I/O 수행
    - I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

## 인터럽트(Interrupt)

- 인터럽트 당한 시점의 레지스터와 prgram counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
- Interrupt(넓은 의미)
    - Interrupt (하드웨어 인터럽트)
    - Trap(소프트웨어 인터럽트)
        - Exception : 프로그램이 오류를 범한 경우
        - systemcall : 프로그램이 커널 함수를 호출하는 경우

### 인터럽트 관련 용어

인터럽트를 발생시키는 곳에 따라 어떻게 인터럽트를 처리할지가 달라지고 이러한 메뉴얼을 정리해서 그때마다 맞는 처리를 하는 것이 필요함

- 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴 : 해당 인터럽트를 처리하는 커널함수

## 시스템콜

- 사용자프로그램이 운영체제 서비스를 받기 위해 커널 함수를 호출하는 것
