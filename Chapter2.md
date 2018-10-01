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
