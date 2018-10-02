# Chapter2 - System Architecture(시스템의 구성도)


## 용어

Architecture<br>
Components<br>
Context<br>
FrameWork<br>
Monolithic<br>
Third party<br>
WHQL - Windows Hardware Quality Labs<br>
KMCS - Kernel-Mode Code Signing<br>
SMP - Symmetric MultiProcessing (대칭형 멀티프로세싱)<br>
SMT - Simultaneous Multi-Threaded (병렬 멀티스레드)<br>
Topology - 형태<br>
heterogeneous - 여러 다른 종류들로 이뤄진<br>
유휴 : [명사] 쓰지 아니하고 놀림.<br>
取捨選擇 [취사선택] : 취할 것은 취(取)하고, 버릴 것은 버려서 골라잡음<br>

- 커널모드 코드는 이식성을 위해 C로 작성 되어있다. 그러나 객체지향 언어의 고유 특징을 차용하고 있다.
예를 들어, 타 객체의 구조체를 직접 수정하지 않고 특정 인터페이스를 통해 구조체에 접근하는 함수를 호출하여 수정한다.

- 하이퍼바이저 : 엄밀히 말하면 커널과 같은 Ring 0에서 실행한다고 한다. 하지만 특수한 명령어인(VT-x, SVM)을 사용한다.
이러한 명령어로 자신을 커널로부터 격리 시킬 수 있다고 한다.


## 윈도우의 가장 기본적인 구조

유저 프로세스 : 일반적인 유저모드 프로세스를 말한다.
서비스 프로세스 : 서비스 컨트롤 관리자(SCM)에 의해서 실행되는 프로세스들
시스템 프로세스 : SMSS(세션 관리자), LSASS, CSRSS 같은 프로세스들
환경 서브시스템 : 호환성을 위한 프로세스인거 같은데 잘 모르겠다.
익스큐티브 : NtOsKernel에서 Ex로 시작하는 계열의 함수 인 것 같다.
윈도우 커널 : 스레드 스케줄링, 인터럽트 처리, 스레드 스케줄링을 위한 루틴과 객체를 제공하며 저수준 OS로 구성되있다 함.
디바이스 드라이버 : 유저의 I/O을 하드웨어의 I/O으로 요청으로 바꿔주거나 네트워크 드라이버 같은 비 하드웨어 드라이버도 포함 한다 함.
하드웨어 추상화 계층 : 하드웨어의 특성으로 인한 커널요소들의 차이점을 분리 시켜준다 캄
NTOSKERNEL(System) : 실질적인 Nt계열 함수들이 구현되어 있는곳.
Ntdll.dll : 시스템 콜로 NTOSKERNEL내의 함수를 호출함


## 대칭형 멀티프로세싱

- `Multitasking`은 하나의 프로세서에서 여러 스레드를 동시에 스레드가 실행 되어 보이는 것처럼 하는 기술.
- `MultiProcessing`은 2개 이상의 프로세서에서 여러 스레드를 실질 적으로 동시에 실행하는 기술.
윈도우는 Symmetrice MultiProcessing

물리 코어 : 실제로 가지고 있는 프로세서의 수
논리 프로세서 : 가상으로 가지고 있는 프로세서의 수 (스레드가 스케쥴 될 수 있다는 것을 ), 그리고 각각의 논리 프로세서는 자신만의 상태를 갖지만, 실행 엔진과 온보드 캐시는 물리 코어랑 공유 한다.
