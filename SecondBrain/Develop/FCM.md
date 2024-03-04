# FCM ( Firebase - Cloud - Messaging )
![[Pasted image 20240301205054.png]]
- 무료로 메시지를 안정적으로 전송할 수 있는 교차 플랫폼 메시징 솔루션

FCM이 제공하는 **주요 기능**
- 알림 메시지 또는 데이터 메시지보내기
- 다양한 메시지 타겟팅
- 클라이언트 앱에서 메시지 보내기

FCM을 이용해 각 유저들에게 푸시메시지 전송하는 방법
- TOKEN
- TOPIC
## TOKEN

![FCM-PUSH-04.png](https://zuminternet.github.io/images/portal/post/2023-02-06-FCM-PUSH/FCM-PUSH-04.png)

- 앱이 FCM 서버와 통신하기 위해 사용되는 **고유한 식별자**
- 앱은 서버와 통신할 때 토큰을 사용하여 **FCM 서버에서 앱을 식별**하고, 이를 통해 **메시지 전송**을 할 수 있습니다.
- FCM의 토큰은 **앱이 설치된 디바이스마다 고유**합니다. **앱이 설치된 디바이스를 추가하거나 삭제할때 토큰이 변경**될 수 있습니다. **( refresh )**
- 서버는 이러한 **FCM 토큰을 사용하여 특정 디바이스에 메시지를 전송**할 수 있습니다.

즉, Token은 Firebase에서 관리하는 **프로젝트별 접속하는 기기의 고유 ID**로 볼 수 있습니다.

## TOPIC

![FCM-PUSH-05.png](https://zuminternet.github.io/images/portal/post/2023-02-06-FCM-PUSH/FCM-PUSH-05.png)

- 토픽(Topic)은 **일종의 채널**로서, 이를 통해 **일련의 수신자들에게 메시지를 전송**할 수 있습니다.
- 구독 및 구독취소 요청 시, **FCM은 구독한 유저들을 내부적으로 관리**합니다.
- subscribe, unsubscribe 메서드를 통해 **구독과 구독 취소요청을 FCM에 전송**할 수 있습니다.
- 토픽을 통한 푸시 발송시, **토픽을 구독한 사용자들에게만 메시지를 전송**할 수 있도록 합니다.

FCM에서 토픽을 선정할때 **유의해야하는 사항**들은 다음과 같습니다.

- 토픽의 이름은 **알파벳과 숫자로만 구성**되어야 하며, **길이는 최대 256자까지** 설정 가능합니다.
- **특수문자나 공백은 사용이 불가**합니다.
- 토픽 이름은 **유일**해야 합니다. 같은 이름의 토픽이 이미 존재하면 **새로운 토픽을 생성할 수 없습니다.**
- 토픽 주제는 **한글로는 주제선정이 불가**합니다.

## 서버가 해야할 모범 사례

FCM의 공식 레퍼런스에서는 FCM을 사용하는 서버의 경우 **다음과 같은 사항을 지켜야함을 명시**

![FCM-PUSH-06.png](https://zuminternet.github.io/images/portal/post/2023-02-06-FCM-PUSH/FCM-PUSH-06.png)

- Firebase에서 **발급된 토큰의 경우 발급 이후의 토큰관리를 하고있지 않기에**, 이를 서버에서 **따로 관리**를 해줘야 함
- **토픽 또한, 서버에서 구독 이후 따로 관리**를 해줘야 함

.서버에서는 Firebase의 각 디바이스 TOKEN 값들과 FCM TOPIC을 구독한 이후 해당 값들을 **가지고 있어야 하고 관리해 줘야함**

# 참고
https://zuminternet.github.io/FCM-PUSH/


![[Pasted image 20240302205814.png]]