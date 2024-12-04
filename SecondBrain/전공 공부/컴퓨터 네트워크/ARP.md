## ARP operation
![[Pasted image 20241204025111.png|500]]
IP 주소를 가지고 목적지 IP주소를 갖고 있는 기기에게 응답을 요청함(broad cast 주소로 전송된다)
그러면 IP주소를 가진 System B는 응답으로 MAC주소를 응답한다.
## ARP packet
![[Pasted image 20241204025559.png|500]]
ARP -> IP주소를 주면 IP를 사용하는 MAC주소를 주는 protocol
RARP -> MAC주소를 주면 MAC 주소를 사용하는 IP주소를 주는 protocol
ARP request는 broadcast이고, ARP reply는 unicast이다.
## Encapsulation of ARP packet
![[Pasted image 20241204025202.png|500]]

## ARP가 필요한 4가지 상황
![[Pasted image 20241204132926.png]]
- Case 1
	- Sender와 Receiver가 같은 네트워크에 있는 상황
- Case 2
	- host가 패킷을 다른 네트워크에 전달하는 상황
- Case 3
	- router가 패킷을 다른 네트워크에 전달하는 상황
- Case 4
	- router가 같은 네트워크에 있는 host에게 패킷을 전달하는 상황
![[Pasted image 20241204133556.png|500]]
A가 B에게 패킷을 보내려는 상황
ARP Request를 보낼 때 MAC주소를 1로 채워 broadcast로 보낸다.
0x0806을 보고 ARP인 것을 확인하고 0x0001을 보고 ARP request인 것을 확인한다.
 ARP reply는 MAC 부소가 채워지고 unicast로 보내진다.
 0x0002를 보고 ARP reply인 것을 확인한다.

## ARP components
![[Pasted image 20241204134938.png|500]]
output module: ARP packet이 output으로 나오는 모듈
- IP packet을 보고 테이블 찾아봐서 resolved가 되면 IP packet을 보내준다.
- state가 F라면 P로 놓고 queue에 요청을 넣고 ARP request를 보낸다.
- state가 P라면 queue에만 넣어둔다.
input module: ARP packet을 input으로 받는 모듈
- ARP packet을 받아 Table 업데이트(P면 R로 R이면 time-out 업데이트)
- P -> R로 변한다면 queue에 있는 값을 모두 꺼내서 응답하는 모듈
## sudo code
### Output module
```
ARP_Output_Module ( )
{
    IP 소프트웨어로부터 IP 패킷을 수신할 때까지 Sleep.

    IP 패킷의 목적지에 해당하는 항목을 찾기 위해 캐시 테이블을 확인.

    If (항목이 발견됨)
    {
        If (상태가 RESOLVED)
        {
            항목에서 하드웨어 주소 값을 추출.
            패킷과 하드웨어 주소를 데이터 링크 계층으로 보냄.
            Return
        } // end if

        If (상태가 PENDING)
        {
            패킷을 해당 큐에 삽입(Enqueue).
            Return
        } // end if
    } // end if

    If (항목이 발견되지 않음)
    {
        상태를 PENDING으로 설정하고 ATTEMPTS를 1로 설정하여 캐시 항목을 생성.
        큐를 생성.
        패킷을 삽입(Enqueue).
        ARP 요청을 보냄.
        Return
    } // end if
} // end module

```
### Input module
```
ARP_Input_Module ( )
{
    ARP 패킷(요청 또는 응답)이 도착할 때까지 Sleep.

    해당 항목을 찾기 위해 캐시 테이블을 확인.

    If (발견됨)
    {
        entry를 업데이트.(state를 R로 바꿈, Queue와 attempt를 지움)
        If (상태가 PENDING)
        {
            While (큐가 비어 있지 않음)
            {
                하나의 패킷을 Dequeue.
                패킷과 하드웨어 주소를 보냄.
            } // end if
        } // end if
    } // end if

    If (발견되지 않음) // request를 처음 받은 상황
    {
        항목을 생성.
        테이블에 항목을 추가.
    } // end if

    If (패킷이 요청인 경우)
    {
        ARP 응답을 보냄.
    } // end if

    Return
} // end module

```
### Cache-Control module
```
ARP_Cache_Control_Module ( )
{
    주기적인 타이머가 만료될 때까지 Sleep.

    캐시 테이블의 모든 항목에 대해 반복 수행
    {
        If (상태가 FREE)
        {
            Continue
        } // end if

        If (상태가 PENDING)
        {
            attempts 값을 1 증가.
            If (attempts가 최대값보다 크면) // max까지 보내봤는데 해결되지 않음 
            {
                상태를 FREE로 변경.
                해당 큐를 삭제.
            } // end if
            else
            {
                ARP 요청을 보냄.
            } // end else
            continue
        } // end if

        If (상태가 RESOLVED)
        {
            time-out 값을 1 감소.
            If (time-out이 0 이하로 감소하면)
            {
                상태를 FREE로 변경.
                해당 큐를 삭제.
            } // end if
        } // end if
    } // end repeat
    Return
} // end module

```
### cache table
![[Pasted image 20241204135033.png|500]]
- State
	- R(Resolved): IP주소와 MAC주소가 매핑되어 있음
	- P(Pending): IP주소로 MAC주소를 알아보고 있는 상황
	- F: 빈칸일 때
### ex 1
IP 주소가 130.23.43.20이고 물리적 주소가 B2:34:55:10:22:10인 호스트가 IP 주소 130.23.43.25이고 물리적 주소가 A4:6E:F4:59:83:AB인 다른 호스트로 패킷을 보내려고 합니다. 두 호스트는 같은 이더넷 네트워크에 있습니다. 이더넷 프레임에 캡슐화된 ARP 요청 및 응답 패킷을 보여주세요.

해결 방법

그림 8.6은 ARP 요청 및 응답 패킷을 보여줍니다. 이 경우 ARP 데이터 필드는 28바이트이고, 개별 주소가 4바이트 경계에 맞지 않는다는 점에 유의하십시오. 그래서 이러한 주소에 대해 일반적인 4바이트 경계를 표시하지 않습니다. 또한 IP 주소는 16진수로 표시됩니다.

### ex 2
ARP 출력 모듈은 대상 주소 114.5.7.89를 가진 IP 데이터그램(IP 계층에서 보낸)을 수신합니다. 캐시 테이블을 확인한 결과, 해당 목적지에 대해 해제된 상태(R 상태)의 항목이 존재합니다. 하드웨어 주소 457342ACAE32를 추출하고, 데이터 링크 계층으로 패킷과 주소를 전송합니다. 캐시 테이블은 그대로 유지됩니다.

20초 후, ARP 출력 모듈은 대상 주소 116.1.7.22를 가진 IP 데이터그램(IP 계층에서 보낸)을 수신합니다. 캐시 테이블을 확인한 결과, 해당 목적지가 테이블에 존재하지 않습니다. 모듈은 테이블에 상태를 PENDING으로, 시도 값을 1로 설정하여 항목을 추가합니다. 이 목적지를 위한 새로운 큐를 생성하고 패킷을 큐에 넣습니다. 그런 다음, 이 목적지에 대한 ARP request을 데이터 링크 계층으로 전송합니다. 새로운 캐시 테이블은 표 8.6에 나와 있습니다.
![[Pasted image 20241204144723.png]]
  
15초 후, ARP input module은 대상 프로토콜(IP) 주소 188.11.8.71을 가진 ARP 패킷을 수신합니다. 모듈은 테이블을 확인하고 이 주소를 찾습니다. 항목의 상태를 RESOLVED로 변경하고 타임아웃 값을 900으로 설정합니다. 그런 다음, 대상 하드웨어 주소(E34573242ACA)를 항목에 추가합니다. 이제 큐 18에 접근하여 이 큐에 있는 모든 패킷을 하나씩 데이터 링크 계층으로 전송합니다. 새로운 캐시 테이블은 표 8.7에 나와 있습니다.
![[Pasted image 20241204144733.png|500]]

25초 후(60초가 됨), 캐시 제어 모듈은 모든 항목을 업데이트합니다. 처음 세 개의 해제된 항목에 대한 타임아웃 값은 60씩 감소합니다. 마지막 해제된 항목의 타임아웃 값은 25 감소합니다. 타임아웃 값이 0이 되어 마지막에서 두 번째 항목의 상태가 FREE로 변경됩니다. 세 개의 보류 중인 항목 각각에 대해 시도 필드 값이 1씩 증가합니다. 증가한 후, 하나의 항목(IP 주소 201.11.56.7)이 최대 값(5)을 초과합니다. 상태가 FREE로 변경되고 큐가 삭제되며 원래 목적지로 ICMP 메시지가 전송됩니다(자세한 내용은 9장을 참조하십시오). 표 8.8을 참조하십시오.
![[Pasted image 20241204144950.png|500]]