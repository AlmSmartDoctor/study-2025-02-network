### Story3 액세스 회선으로 이용하는 PPP와 터널링

- 액세스 회선(ADSL, FTTH 등)으로 흘러온 패킷은 **엑세스 회선을 운영하는 사업자**의 BAS에 도착
- 엑세스 회선을 운영하는 사업자는 LGU+, KT, SKB 등등이 있음
- 내가 이 사업자의 가입자인지 확인하는 역할(본인 확인 및 설정값 통지)을 BAS에서 수행

- BAS는 이 역할의 수행을 위해 PPPoE 구조를 사용한다.
- PPPoE는 보통의 전화 회선을 이용한 다이얼업(1990년대부터 2000년대 초반에 사용한 전화선을 이용한 방식)접속으로 이용하는 PPP구조를 발전시킨 것
---
#### PPP 구조란?
- Point-to-Point Protocol
![KakaoTalk_20250326_233950690_01](https://github.com/user-attachments/assets/82e406a5-ea09-46f1-ad30-7fedc3e667a7)
- PPP 메시지에 사용자의 아이디, 패스워드가 담겨있음
- RADIUS라는 프로토콜을 이용하여 인증서버(RAS)에서 가입자가 맞는지 확인이 되면 IP 주소 등의 설정 정보가 반송됨
- 이 정보를 사용자 측에서 IP주소에 저장
- 인터넷 관리 업체들의 사용자 관리 요구 및 이더넷과 PPP의 결합이 필요해짐 -> PPPoE 등장
![KakaoTalk_20250326_233950690](https://github.com/user-attachments/assets/a06bd3ef-d242-45b8-b23e-87709f11d889)
![KakaoTalk_20250326_234341062](https://github.com/user-attachments/assets/cb8fae74-3199-4593-a310-b1a58f7919f1)
---
#### 엑세스 회선의 전체적인 동작 요약
1. 인터넷 진입용 라우터 진입
   - PPPoE, PPP 헤더가 부가됨
   - BAS의 MAC주소를 찾는 Discovery구조 실행
2. ADSL/ONU, DSLAM/OLT를 거쳐 BAS로 진입하여 패킷이 셀 및 전기신호로 분리되다가 다시 패킷을 이룸
3. BAS에서 PPP를 읽고 가입자인지 확인되면 IP주소, DNS 서버의 IP 주소, 기본 게이트웨이를 반환
4. 인터넷 진입용 라우터의 글로벌 IP주소 할당, 사용자가 수동으로 설정하지 않는 이상 DNS 서버의 IP 주소 할당, 경로표의 기본 게이트웨이 설정
  - 만약 라우터-BAS가 1 대 1로 연결되어있다면 "언넘버드"로, 차피 라우터가 무조건 BAS로 흐를 것임이 명확하기에 포트에 IP를 기록하지 않는다.
  - 이 경우 게이트웨이의 IP 주소를 통지하지 않음

참고로 모든 패킷에 PPPoE가 붙는게 아니고 세션이 유효할때는 붙지 않음

- BAS에서 받아온 글로벌 IP는 인터넷 접속용 라우터에 할당됨
- 이 라우터에 연결된 PC는 글로벌 IP에 대한 프라이빗 IP로 변환됨

- PPPoE 패킷 구조
![KakaoTalk_20250327_001518242](https://github.com/user-attachments/assets/9b1373af-1d34-4a34-bf0d-719f9e8300fe)
---
##### PPPoE 이외의 방식인 PPPoA
- PPPoA는 MAC헤더와 PPPoE 헤더를 붙이지 않고 PPP 메시지를 전달함
- 이더넷에서는 MAC주소 기반이기 때문에 라우터-ADSL모뎀 일체화 모델이 필요함
- 헤더가 없기에 MTU가 길어짐

##### 사내 LAN에서 DHCP 구조
- 사용자명과 패스워드를 확인하지 않고 TCP/IP의 설정 정보를 통지하는 구조
- MTU가 길어짐
---
### Story4 프로바이더 내부

**엑세스 회선을 빠져나온 패킷은 인터넷에 연결됨**
![KakaoTalk_20250327_003616697](https://github.com/user-attachments/assets/8289f67e-0cef-4668-ad62-d373b1e5c2bf)
- 각 엑세스회선에서 나온 패킷은 엑세스 회선에 맞는 라우터에 연결됨

프로바이더의 라우터의 전송 능력: **테라비트/초** 넘음
사용자의 인터넷 접속 라우터: **100메가비트/초**
- 따라서 엑세스 회선과 연결되는 라우터는 처리 능력이 낮아도 됨

이후에는 3장에서 나온것처럼 패킷이 다른POP(Point Of Presence)나 NOC(Network Operation Center)로 중계되어서 서버에 도달

프로바이더의 내부는 대부분 광섬유 케이블로 이루어지는데 프로바이더 사업자는 이를 임차하여 매출을 올리기도 함
ex)KT 스카이라이프는 KT의 인터넷망을 임대
