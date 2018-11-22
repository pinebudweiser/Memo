# Chapter2 - System Architecture(시스템의 구성도)


## 용어

Architecture<br>
Components - 구성요소<br>
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
subsystem - 하위 시스템<br>
유휴 : [명사] 쓰지 아니하고 놀림.<br>
取捨選擇 [취사선택] : 취할 것은 취(取)하고, 버릴 것은 버려서 골라잡음<br>
VTL - VirtualTrustLevel<br>
SLAT - SecondLevelAddressTranslation(https://blogs.technet.microsoft.com/koalra/2009/02/12/second-level-address-translation-slat-hyper-v-r2/ : 설명 진짜 잘 되있다.)<br>
Side by side : https://docs.microsoft.com/ko-kr/dotnet/framework/app-domains/assemblies-and-side-by-side-execution<
https://docs.microsoft.com/ko-kr/dotnet/framework/deployment/side-by-side-execution<br>
manifest : http://bsnippet.tistory.com/7<br>
ACPI - Advanced Configuration and Power Interface
APC - Asynchronous Procedure Call
DPC - Deferred Procedure call

- 커널모드 코드는 이식성을 위해 C로 작성 되어있다. 그러나 객체지향 언어의 고유 특징을 차용하고 있다.
예를 들어, 타 객체의 구조체를 직접 수정하지 않고 특정 인터페이스를 통해 구조체에 접근하는 함수를 호출하여 수정한다.

- 하이퍼바이저 : 엄밀히 말하면 커널과 같은 Ring 0에서 실행한다고 한다. 하지만 특수한 명령어인(VT-x, SVM)을 사용한다.
이러한 명령어로 자신을 커널로부터 격리 시킬 수 있다고 한다.


## 윈도우의 가장 기본적인 구조

유저 프로세스 : 일반적인 유저모드 프로세스를 말한다.<Br>
서비스 프로세스 : 서비스 컨트롤 관리자(SCM)에 의해서 실행되는 프로세스들<br>
시스템 프로세스 : SCM, Winlogon, WinInit<br>
환경 서브시스템 : SMSS, CSRSS<br>
익스큐티브 : NtOsKernel에서 Ex로 시작하는 계열의 함수 인 것 같다.<br>
윈도우 커널 : 스레드 스케줄링, 인터럽트 처리, 스레드 스케줄링을 위한 루틴과 객체를 제공하며 저수준 OS로 구성되있다 함.<br>
디바이스 드라이버 : 유저의 I/O을 하드웨어의 I/O으로 요청으로 바꿔주거나 네트워크 드라이버 같은 비 하드웨어 드라이버도 포함 한다 함.<br>
하드웨어 추상화 계층 : 하드웨어의 특성으로 인한 커널요소들의 차이점을 분리 시켜준다 캄<br>
NTOSKERNEL(System) : 실질적인 Nt계열 함수들이 구현되어 있는곳.<br>
Ntdll.dll : 시스템 콜로 NTOSKERNEL내의 함수를 호출함<br>

## 아키텍쳐 개요(100p)

## 이식성(104p)

## 대칭형 멀티프로세싱 (106p)

- `Multitasking`은 하나의 프로세서에서 여러 스레드를 동시에 스레드가 실행 되어 보이는 것처럼 하는 기술.
- `MultiProcessing`은 2개 이상의 프로세서에서 여러 스레드를 실질 적으로 동시에 실행하는 기술.<br>
윈도우는 Symmetrice MultiProcessing

물리 코어 : 실제로 가지고 있는 프로세서의 수<br>
논리 프로세서 : 가상으로 가지고 있는 프로세서의 수 (스레드가 스케쥴 될 수 있다는 것을 ), 그리고 각각의 논리 프로세서는 자신만의 상태를 갖지만, 실행 엔진과 온보드 캐시는 물리 코어랑 공유 한다.

## 확장성 (109p)

## 가상화 기반의 보안 아키텍처 개요 (118p)

- VTL0 : 일반적인 Ring3, Ring0
- VTL1 : 격리된 Ring3(IUM), Ring0(안전한 커널, securekernel, proxy kernel)
- VTL1은 VTL0의 메모리와 데이터를 마음대로 다룰 수 있다.

## 핵심 시스템 컴포넌트 (122p)

2권의 8장 - 주요 시스템이 사용하는 제어 메커니즘(객체 관리자, 인터럽트)<br>
2권의 11장 - 윈도우의 시작과 종료 절차를 설명<br>
2권의 9장 - 레지스트리, 서비스 프로세스, WMI 관리 메커니즘<br>

`Subsystem` 유형 확인 - PE IMAGER_OPTIONAL_HEADER->Subsystem, Visual Studio /SUBSYSTEM 링커 옵션 혹은 프로젝트의 Linker/System 속성 페이지 안의 Subsystem 항목<br>

윈도우에서는 GUI, CUI가 있지만 단 하나의 서브시스템으로 통합된다.<br>
GUI 또한 AllocConsole로 콘솔을 가질 수 있으며, Console또한 다이얼로그나 WndProc 생성으로 윈도우 창을 가질 수 있다.<br>

어플리케이션이 DLL내의 함수를 호출 할 때 3가지의 경우가 발생 할 수 있다.<br>
1. 시스템 호출이 필요 없는 경우(GetCurrentProcess, GetCurrentProcessId)
2. 시스템 호출이 필요한 경우 -> NTDLL에서 시스템 호출
3. 환경 서브시스템 프로세스에서 수행돼야 하는 어떤일을 요구하는 경우, DLL을 통하여 ALPC로 환경 서브시스템과 통신 - 응답이 올 때 까지 기다림 

NTDLL.dll - 일반적인 시스템 호출<br>
- LDRxxxx : 이미지 로더 관련 함수
- CSRxxxx : 서브시스템(CSRSS) 관련 함수
- RTLxxxx : 런타임 라이브러리 루틴
- DbgUixx : 유저모드 디버깅을 위한 지원
- ETWxxxx : 이벤트 트레이싱
- 문자열 표준 라이브러리 함수들(memcpy, strcpy등)
IUMDLL.dll - 자격증명이 활성화 된 경우에 시스템 호출<br>

NTOSKRNL -> SMSS(서브시스템에 의존적이지 않음) -> CSRSS

## 익스큐티브 (140p)

NTOSKRNL에서 나누는 상위와 하위계층은 언어에서 말하는 Low, High 비슷한 것 같다.<br>
익스큐티브는 NTOSKRNL에서 `상위계층` 이고 커널은 `하위계층`이다.

>> 함수 호출 유형
- `Export` 되어있고 유저 모드로부터 호출 가능한 함수들 : NTDLL에 Export table에 등록 되어 있는 함수들. 단 여기서 문서화 되어있지 않은 쿼리계열 함수(NTQueryInformationProcess)도 있다!
- 유저모드 프로세스에서 DeviceIoControl로 장치내 컨트롤 코드를 전송하는 경우
- WDK에 문서화 되어 있고 커널모드에서만 호출 가능한 함수들 : NTDLL에는 Export table에 등록 되어 있지 않고, NTOSKRNL에는 EXPORT되어있는 함수들 = IoCreateFile
- WDK에 문서화 되어 있지 않지만 커널모드에서 호출 가능한 함수들

익스큐티브 관련 함수들은 ExXXXX 규격을 따르며, 여러 컴포넌트들로 OS 메커니즘과 정책을 구현한다.<br>

## 커널 (144p)

커널은 NTOSKRNL에서 `하위 계층`이며 익스큐티브가 OS를 규정하는 다양한 틀이라면 커널은 그 재료이다.<br>
하지만 재료라 해서 모든 것들이 익스큐티브에 의해서 결정 되는 것은 아니며 하드웨어 아키텍쳐에 `종속`적인 내용들은 구현되있다.<br>
주로 C 코드로 작성 되어있으나, 특정 CPU 명령어나 레지스터에 대한 직접 접근이 필요한 경우에는 어셈블리언어를 사용하였다.<br>

>> 커널 객체
- 커널 객체는 익스큐티브 컴포넌트를 위한 구조체이다.
- 커널 객체는 익스큐티브 객체를 이루기 위해 여러 집합으로 뭉쳐질 수 있다.
- 컨트롤 객체, 디스패처 객체 이러한 요소들이 커널 객체인 뮤텍스, 이벤트, 세마포어, 타이머 같은 것으로 이루어져 있다

=> 최종적으로 익스큐티브는 커널 객체의 인스턴스를 생성하고 커널 관련 함수를 호출하여 익스큐티브 컴포넌트를 구현한다!!

- KPCR(KernelProcessControlRegion) : 프로세스 한정적 정보를 저장한다. IDT(IntrruptDispatcherTable), TSS(TaskStatementSegment), GDT(GlobalDescriptorTable)을 담고 있다.

x86 -> fs -> KPCR<br>
x64 -> gs -> KPCR<br>


