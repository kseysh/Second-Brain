## 소스/대상 확인 비활성화
EC2는 자신에게 오지 않는 트래픽을 차단한다.
NAT는 트래픽을 중계해야 하므로 자신에게 오지 않는 트래픽을 받아서 private subnet에 보내줄 수 있어야 한다.
## 패킷 포워딩 설정
### IP 포워딩 활성화
시스템이 패킷을 전달할 수 있도록 설정
`sysctl net.ipv4.ip_forward = 1` 로 변경 (패킷을 전달할 수 있도록 하는 설정)
```
# 루트 권한 획득
sudo -i

# 현재 설정 확인 (0이면 비활성, 1이면 활성)
sysctl net.ipv4.ip_forward

# 설정 파일 수정하여 영구 적용
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf

# 변경 사항 즉시 적용
sysctl -p
```
### 네트워크 인터페이스 이름 확인
iptables 설정을 위해 외부와 연결된 인터페이스 이름 가져오기
```
ip route get 8.8.8.8
```
### IP 마스커레이딩 설정
private Subnet에서 온 패킷이 이 인스턴스의 공인 IP를 달고 나가도록 설정 (NAT)
```
# ens5 부분을 위에서 확인한 본인의 인터페이스 이름으로 변경해야함
iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE
```
### 라우팅 테이블 수정
