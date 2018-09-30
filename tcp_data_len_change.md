# :speech_balloon: tcp_data_len_change

Comment: TCP Data 영역을 수정하라(길이 포함).<br>
Argument Format : tcp_data_len_change \<from string\> \<to string\> <br>
Table command : iptables -A OUTPUT -j NFQUEUE, iptables -A INPUT -j NFQUEUE

## :green_book: 데이터 영역을 수정 하기 위한 방법
  - 데이터(payload)가 존재 하는지 확인한다.
  - 데이터 영역내에 수정 할 대상의 문자열이 있는지 확인한다.
  
  IP 헤더의 `TotalLength`를 수정된 데이터 만큼 가감을 연산하여 정상 패킷으로 만들어 준다하면 가능하지 않을까?<br>

## :green_book: 사용되는 요소

- STL Map<key,value> : 키를 하나의 플로우 클래스로 지정, value는 SEQ, ACK 구조체로 저장한다.
- INPUT, OUTPUT 체인 중 패킷 헤더를 조사해 각각의 Map 트리를 가지게 한다.

## :green_book: 부족한 개념

- IP Class, TCP Class 간의 관계를 어떻게 둬야 될지.. 상속?
- 인스턴스에 대한 여러가지 초기화 방법.
- 연산자 오버로딩에서의 왼쪽 피연산자를 동적 할당된 영역으로 받는 방법.

## 구현 요소

- 가상헤더를 어떻게 깔끔 하게 넣어서 체크섬을 계산 할 까..?
