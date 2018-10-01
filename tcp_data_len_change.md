# :speech_balloon: tcp_data_len_change

Comment: TCP Data 영역을 수정하라(길이 포함).<br>
Argument Format : tcp_data_len_change \<from string\> \<to string\> <br>
Table command : iptables -A OUTPUT -j NFQUEUE, iptables -A INPUT -j NFQUEUE

## :green_book: 데이터 영역을 수정 하기 위한 방법
  - 데이터(payload)가 존재 하는지 확인한다.
  - 데이터 영역내에 수정 할 대상의 문자열이 있는지 확인한다.
  - 새로운 패킷을 만들어 줘야 한다. 데이터 영역이 늘어나거나 줄어 들 수 있기 때문이다.

## :green_book: 사용되는 요소

- STL Map<key,value> : Key는 하나의 플로우를 저장 한다. Value는 이전 플로우가 가지고 있는 길이를 저장한다.
- INPUT, OUTPUT 체인중 패킷 헤더를 조사해 각각의 Map 트리를 가지게 한다.
- 나는 문자열만 수정 할 것이기 때문에 수집 조건은 ACK 패킷과 TCP 데이터의 길이가 0이 아닐 때 Map에 삽입한다. 

## :green_book: 부족한 개념

- IP Class, TCP Class 간의 관계를 어떻게 둬야 될지..
- 인스턴스에 대한 여러가지 초기화 방법.
- 연산자 오버로딩

## 구현 요소

- 가상헤더를 어떻게 깔끔 하게 넣어서 체크섬을 계산 할 까..? // 해제시점을 잘 모르겠음.. 자꾸 터짐

## 질문

- IP 헤더의 `TotalLength`를 수정된 데이터 만큼 연산하여 설정 후 보내면 가능한가..
- 연산자 오버로딩에서의 왼쪽 피연산자를 동적 할당된 영역으로 받는 경우.
- 와이어 샤크와 NetFilter로 인한 패킷이 잡히는 시점
