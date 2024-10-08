# 금융분야 마이데이터 표준API 규격

# 제3장. 마이데이터 참여자별 표준 API 처리 절차

# 3.1 사전준비

- 기관정보 등록(마이데이터사업자/정보제공자/중계기관/통합인증기관)

1. 종합포털의 절차에 따라 회원가입 및 기관정보(기관명, TLS 인증서 시리얼번호, 도메인명 등) 등록
- 종합포털은 기관코드를 발급(따라서 중계기관 및 중계기관을 이용하는 기관도 종합포털에 기관 등록 필요)
- TLS 인증서 시리얼번호 등록
- 기관간 상호인증, 데이터 암호화 송‧수신을 위해 기관들은 mutual TLS를 적용하여야 하는데, 이를 위해 각 기관이 공인된 CA기관으로부터 발급받은 TLS 인증서(EV등급)의 시리얼번호(Subject:serialNumber (OID:2.5.4.5)) 정보*를 종합포털에 등록
- * TLS 인증서 내 SERIALNUMBER 값(통상적으로 기관등록번호(사업자등록번호)이나 법인등록번호 등 그렇지 않은 경우도 존재)을 등록함으로써, TLS 인증서를 추후 재발급받더라도 SERIALNUMBER가 동일한 경우 종합포털에 SERIALNUMBER 정보 재등록 불필요
- API 제공자(서버)는 API 요청자(클라이언트)가 API 호출할 때마다 종합포털에 등록된 SERIALNUMBER와 API 요청자로부터 전달받은(TLS handshake) TLS 인증서 내 SERIALNUMBER를 비교·검증 필요
- * 다만, API 요청자는 API 제공자의 TLS 인증서 내 SERIALNUMBER 검증 불필요
- * API 제공자도 TLS 인증서 내 SERIALNUMBER 비교·검증을 생략 가능(일부 기관의 경우, SSL가속기에서 TLS상호인증을 수행하기 때문에 TLS 인증서 추출이 불가)하나, 이로 인한 사고 발생 시에는 API 제공자의 책임이 수반(위험수용)
2. 마이데이터사업자, API를 직접 구축한 정보제공자 및 중계기관은 종합 포털로부터 지원API 호출용 자격증명 및 지원API 제공용 자격증명을

금융보안원 www.fsec.or.kr - 23 -

# 금융분야 마이데이터 표준API 규격

# 발급받아, 기관 내 시스템 등에 반영

- (지원API 호출용 자격증명) 마이데이터사업자, 정보제공자 또는 중계기관이 지원API를 호출(종합포털이 API제공)하기 위한 접근토큰 발급 시 필요한 자격증명
- (지원API 제공용 자격증명) 종합포털이 지원API를 호출(마이데이터사업자, 정보제공자 또는 중계기관이 API제공)하기 위한 접근토큰 발급 시 필요한 자격증명

# 서비스정보 등록(마이데이터사업자)

1. 종합포털의 절차에 따라 서비스정보(서비스명, Callback URL 등) 등록
2. 종합포털로부터 서비스 자격증명을 발급받아, 기관 내 시스템 등에 반영
- (서비스 자격증명) 마이데이터사업자가 정보제공API를 호출(정보제공자가 API제공)하기 위한 접근토큰 발급 시 필요한 자격증명

# 종합포털과 접속 채널 등 설정(마이데이터사업자/정보제공자/중계기관/통합인증기관)

안전한 지원API 호출을 위해 마이데이터사업자, API를 직접 구축한 정보제공자, 중계기관 및 통합인증기관은 종합포털 IP 접근제어(방화벽 등록 등) 및 TLS 인증서 정보를 기관 내 시스템 등에 반영

# 기관정보/서비스정보 수신(마이데이터사업자/정보제공자/중계기관/통합인증기관)

1. 마이데이터사업자, API를 직접 구축한 정보제공자 및 중계기관, 통

금융보안원 www.fsec.or.kr - 24 -

# 금융분야 마이데이터 표준API 규격

# 1. 합인증기관

합인증기관은 “지원API 호출용 자격증명”을 이용하여 종합포털에 OAuth 2.0 인증(지원-001 API 호출) 후 종합포털이 발급한 접근토큰을 획득

|API ID|지원-001|
|---|---|
|API명(URI)|/mgmts/oauth/2.0/token|
|API 설명|지원API 호출을 위한 접근토큰 발급|
|API 제공자|종합포털|
|요청 정보|지원API 호출용 자격증명|
|응답 정보|접근토큰|

# 2. 마이데이터사업자

API를 직접 구축한 정보제공자 및 중계기관은 발급받은 접근토큰을 이용하여 종합포털로부터 기관정보(지원-002 API 호출) 및 서비스정보(지원-003 API 호출) 수신

|API ID|지원-002|
|---|---|
|API명(URI)|/mgmts/orgs|
|API 설명|기관 정보 조회|
|API 제공자|종합포털|
|요청 정보|접근토큰, 조회 타임스탬프|
|응답 정보|기관정보|

|API ID|지원-003|
|---|---|
|API명(URI)|/mgmts/services|
|API 설명|마이데이터사업자 서비스 정보 조회|
|API 제공자|종합포털|
|요청 정보|접근토큰, 조회 타임스탬프|
|응답 정보|서비스 정보|

- 기관 또는 서비스 신규‧변경‧삭제 정보 반영을 위해 필요 시 본 API를 비정기적으로 호출하여야 함

- 다만 마이데이터 산업 초기에 빈번한 API 호출을 방지하기 위해 참여자들이 기관‧서비스 정보를 사전에 등록하는 기간‧절차를 마련할 필요

# 3. 통합인증기관

통합인증기관은 발급받은 접근토큰을 이용하여 종합포털로부터 기관정보(지원-002 API 호출) 수신

금융보안원 www.fsec.or.kr - 25 -

# 금융분야 마이데이터 표준API 규격

|API ID|지원-002|
|---|---|
|API명(URI)|/mgmts/orgs|
|API 설명|기관 정보 조회|
|API 제공자|종합포털|
|요청 정보|접근토큰, 조회 타임스탬프|
|응답 정보|기관정보|

# 통합인증 이용 준비(마이데이터사업자/정보제공자/중계기관/통합인증기관)

※ 통합인증 이용 준비사항은 “[별첨1] 인증서 본인확인 기반 통합인증 절차 및 규격”, “[별첨2] 사설인증서 기반 통합인증 절차 및 규격”참조

# 기관 간 접속 채널 등 설정(마이데이터사업자/정보제공자/중계기관/통합인증 기관)

ㅇ 안전한 API 호출을 위하여 종합포털로부터 수신한 기관정보 및 서비스정보 등을 이용하여 IP 접근제어(방화벽 등록 등) 및 TLS 인증서 정보*를 기관 내 시스템 등에 반영

* 기관 간 전용선(또는 VPN)으로 연결한 경우 TLS 상호인증 관련 절차 생략 가능

|구 분|마이데이터 사업자|정보제공자 (API 직접 구축)|중계기관|통합인증기관|
|---|---|---|---|---|
|마이데이터 사업자|○|○| | |
|정보제공자 (API 직접 구축)|○| |○| |
|중계기관|○| |○| |
|통합인증 기관|○|○| | |

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

※ 통합인증기관과 정보제공자 및 중계기관 간 접속 채널은 “[별첨1] 인증서 본인확인 기반 통합인증 절차 및 규격”, “[별첨2] 사설인증서 기반 통합인증 절차 및 규격” 참조

금융보안원 www.fsec.or.kr - 27 -

# 금융분야 마이데이터 표준API 규격

# 3.2 개인신용정보 전송요구‧변경‧철회

개인신용정보 전송요구는 정보주체 인증방식인 개별인증 및 통합인증에 따라 절차가 상이하여 각각을 구분하여 설명함

# 3.2.1 개별인증 시

# 3.2.1.1 개인신용정보 전송요구 또는 전송요구 변경

정보주체가 개별인증 방식을 통하여 정보제공자로 하여금 본인의 개인 신용정보를 마이데이터사업자에게 전송해줄 것을 요구하기 위해서는 다음과 같은 절차를 거침

- 인가코드 발급
- 정보주체가 정보제공자에게 본인임을 개별인증을 통해 인증하고, 전송요구할 자산을 선택하여 전송요구권(신청 또는 변경)을 행사
- 정보제공자는 인증 및 전송요구가 성공적으로 완료되면 인가코드를 발급하여 마이데이터사업자에게 전달
- 접근토큰 발급
- 마이데이터사업자는 인가코드를 이용하여 정보제공자에게 접근토큰 발급을 요청
- 정보제공자는 정보주체의 전송요구 내역을 기반으로 권한(scope)을 설정하고 마이데이터사업자에게 접근토큰을 발급
- 개인신용정보 전송요구 내역 조회
- 마이데이터사업자는 발급된 접근토큰을 이용하여 전송요구 내역을 조회

금융보안원 www.fsec.or.kr - 28 -

# 금융분야 마이데이터 표준API 규격

# 인가코드 발급

사전준비 필요 사항

안드로이드 환경에서 일부 개별인증수단(금융인증서, 네이버인증서 등)을 웹뷰를 통해 구동하기 위해서는 DomStorage 관련 웹뷰 속성이 반드시 설정 필요 (미설정 시 에러 발생)

- (마이데이터사업자 사전준비 필요 사항) 안드로이드 OS용 마이데이터서비스앱의 개별인증을 위한 웹뷰 세팅 속성에 setDomStorageEnabled(true)를 반드시 설정 (iOS용은 불필요)

# 기타 정보

32411 4BIZ XB

14401 492 Tlzt ck 44 +3

[z] CIzt client_id, redirect URI statel Jtellzh) 5Q17135 432* (4383-001 API) redirect URI 5/ 33

Q13484 [a] 9334 943*4,Cl3i 3 33

4E4 Qz35 43

2CtoleE (Callback URL) state( JEHzh) 33

0l cHa+ 88

# 정보주체 마이데이터서비스

회원가입 후 해당 마이데이터사업자의 서비스앱에 로그인하여 전송요구 또는 전송요구 변경할 정보제 공자를 선택

금융보안원 www.fsec.or.kr - 29 -

# 금융분야 마이데이터 표준API 규격

# ② (서비스 서버) 인가코드 발급을 요청(개별인증-001 API 호출)

|API ID|개별인증-001|
|---|---|
|API명(URI)|/oauth/2.0/authorize|
|API 설명|인가코드 발급(정보제공API 호출에 필요한 접근토큰 발급 시 필요)|
|API 제공자|정보제공자|
|요청 정보|CI값, 클라이언트 ID, Callback URL, 앱스킴, 상태값 등|
|응답 정보|인가코드, 상태값 등|

# ③ (정보제공자) 마이데이터사업자가 전송한 개별인증-001 파라미터들

(x-user-ci, client_id, redirect_uri 등) 검증

- 마이데이터사업자가 전송한 redirect_uri가 해당 마이데이터사업자의 Callback URL 목록*에 속해있는지 여부 검증

* 마이데이터사업자가 종합포털에 등록한 Callback URL들(최대 4개까지 등록 가능)

# ④ (정보제공자) 개별인증을 위한 인증화면을 웹뷰 등으로 정보주체에 게 제공 (②에 대한 응답)

- 웹뷰 종료(닫기) 버튼은 정보제공자가 아닌 마이데이터사업자의 서비스 앱에서 제공

# (상세절차 예시)

정보제공자는 개별인증 접속 URL을 서비스 서버로 회신하고, 서비스 서버는 해당 URL을 서비스 앱으로 전달, 서비스 앱은 해당 URL에 접속하여 정보제공자로부터 개별인증을 처리할 수 있는 웹페이지 전문(HTML 등)을 회신받아 웹뷰에 출력

4HI %NBA 4BI 0-273043URL %? O) HTTP/1.1 302 Found

0-5 3 0-4 #olxi

금융보안원 www.fsec.or.kr - 30 -

# 금융분야 마이데이터 표준API 규격

# 4. 인증 절차

1. # 정보제공자

개별인증 접속 URL을 서비스 서버에 회신

(HTTP 상태코드 302 회신, location 헤더에 개별인증 접속 URL 설정)
2. # 서비스 서버

HTTP 응답의 location 헤더값에서 개별인증 접속 URL을 추출한 후 서비스 앱에 해당 URL을 전달

* HTTP 상태코드가 302이나, 서비스 서버에서 redirect를 수행하지 않고, location 헤더값 내 URL만 추출
3. # 서비스 앱

해당 URL(정보제공자 인증사이트의 개별인증 접속 URL)에 접속
4. # 정보제공자 인증사이트

인증화면 웹페이지 전문(HTML 등)을 서비스 앱에 회신
5. # 서비스 앱

인증화면을 웹뷰에 출력
6. # 정보주체

제공된 개별인증 화면을 통해 인증을 수행
7. # 서비스 앱

정보주체가 입력한 인증정보를 정보제공자(정보제공자 인증사이트)에게 전송
8. # 정보제공자

전달받은 인증정보 검증 등 개별인증 수행

- 정보제공자는 개별인증 완료 후 서비스 서버가 개별인증-001 API 호출 시(②) 전달한 CI값(x-user-ci)과 정보제공자가 보유하고 있는 정보주체(고객)의 CI값을 반드시 비교 및 검증

금융보안원 www.fsec.or.kr - 31 -

# 금융분야 마이데이터 표준API 규격

# ⑧ (정보제공자)

개인신용정보 전송요구 또는 전송요구 변경 화면을 웹 뷰 등으로 제공 (⑥에 대한 응답)

- 정보제공자는 신용정보법 제33조의2제5항에 의거하여 정보주체가 아래의 내용을 특정할 수 있도록 화면 구성 필요

|특정 사항|정보주체 선택(또는 고지) 항목|
|---|---|
|정기적 전송을 요구하는지 여부 및 요구 시 그 주기|• 정기적 전송 요구 여/부 선택 • 주기|
|전송요구의 종료시점|• 전송요구의 종료시점|
|전송을 요구하는 목적|• 전송을 요구하는 목적|
|전송을 요구하는 개인신용정보의 보유기간|• 마이데이터사업자가 수집한 정보를 보유할 수 있는 기간|
|전송을 요구하는 개인신용정보|• 업권별 상이 (상세내용은 2.2- 의 “전송요구 scope” 내용 참조)|

- 업권별 “전송을 요구하는 개인신용정보”제공 화면은 “2.2 인증규격 - 권한(scope)”의 전송요구 scope를 참고하여 구성 필요
- 전송요구 변경 시에는 기존 전송요구 시 정보주체가 선택했던 내역을 화면에 반영 필요

# ⑨ (정보주체)

제공된 개인신용정보 전송요구 또는 전송요구 변경 화면을 통해 전송요구 내역 선택

# ⑩ (서비스 앱)

정보주체가 특정한 전송요구 내역을 정보제공자에게 전송

# ⑪~⑫ (정보제공자)

전송요구 내역을 저장하고, 인가코드를 생성

# ⑬ (정보제공자)

생성한 인가코드 및 state(상태값)을 서비스 앱에 회신 (⑩에 대한 응답, ②에서 호출한 개별인증-001 API에 대한 회신)

금융보안원 www.fsec.or.kr - 32 -

# 금융분야 마이데이터 표준API 규격

# 회신 예시

HTTP/1.1 302 Found
Location: https://마이데이터사업자_Callback_URL?code=인가코드&state=상태값&api_tran_id=거래고유번호

⑭  (서비스     앱)  ⑬에서     전달받은      Callback  URL로   리다이렉트

⑮~⑯     (서비스     서버)   state(상태값)     검증    및   인가코드      저장    후   서비스
앱에게     ⑭에    대한    응답*

* 별도의 응답 상세 규격은 없으며, HTTP 응답코드 회신

금융보안원 www.fsec.or.kr

-  33 -

# 금융분야 마이데이터 표준API 규격

# 1. 나 : 앱방식 개별인증 시 인가코드 발급

# 사전준비 필요 사항

iOS 환경에서 앱인증 방식 개별인증 수행 시 마이데이터사업자앱→정보제공자앱으로 앱 간 전환을 하기 위해서는 마이데이터사업자앱의 info.plist에 정보제공자앱의 앱 URL스킴들이 사전에 등록되어 배포되어야 함.

- info.plist에 미등록 시 웹뷰에서 타 앱으로 전환 불가, 정보제공자 앱이 설치되어 있는지 여부 검증 불가 등 이슈 발생

정보제공자의 앱스킴이 추가/변경/삭제될 때마다 모든 마이데이터사업자가 info.plist를 갱신한 후 앱을 재배포하는 것은 매우 비효율적이기 때문에, 정보제공자들의 앱 URL스킴을 사전정의(60개, mydataProviderApp01~mydataProviderApp60)하고, 정보 제공자별로 할당하는 방식을 적용.

# 앱인증 방식으로 개별인증을 제공하는 정보제공자가 3개만 존재하는 경우 할당 예시

|정보제공자 앱URL스킴 (사전정의)|정보제공자 (할당)|
|---|---|
|mydataProviderApp01|A기관|
|mydataProviderApp02|B기관|
|mydataProviderApp03|C기관|
|mydataProviderApp04|(미할당)|
|mydataProviderApp05|(미할당)|
|mydataProviderApp06|(미할당)|
|mydataProviderApp07|(미할당)|
|mydataProviderApp08|(미할당)|
|mydataProviderApp09|(미할당)|
|mydataProviderApp10|(미할당)|
|mydataProviderApp11|(미할당)|
|mydataProviderApp12|(미할당)|
|mydataProviderApp13|(미할당)|
|mydataProviderApp14|(미할당)|
|mydataProviderApp15|(미할당)|
|mydataProviderApp16|(미할당)|
|mydataProviderApp17|(미할당)|
|mydataProviderApp18|(미할당)|
|mydataProviderApp19|(미할당)|
|mydataProviderApp20|(미할당)|
|mydataProviderApp21|(미할당)|
|mydataProviderApp22|(미할당)|
|mydataProviderApp23|(미할당)|
|mydataProviderApp24|(미할당)|
|mydataProviderApp25|(미할당)|
|mydataProviderApp26|(미할당)|
|mydataProviderApp27|(미할당)|
|mydataProviderApp28|(미할당)|
|mydataProviderApp29|(미할당)|
|mydataProviderApp30|(미할당)|
|mydataProviderApp31|(미할당)|
|mydataProviderApp32|(미할당)|
|mydataProviderApp33|(미할당)|
|mydataProviderApp34|(미할당)|
|mydataProviderApp35|(미할당)|
|mydataProviderApp36|(미할당)|
|mydataProviderApp37|(미할당)|
|mydataProviderApp38|(미할당)|
|mydataProviderApp39|(미할당)|
|mydataProviderApp40|(미할당)|
|mydataProviderApp41|(미할당)|
|mydataProviderApp42|(미할당)|
|mydataProviderApp43|(미할당)|
|mydataProviderApp44|(미할당)|
|mydataProviderApp45|(미할당)|
|mydataProviderApp46|(미할당)|
|mydataProviderApp47|(미할당)|
|mydataProviderApp48|(미할당)|
|mydataProviderApp49|(미할당)|
|mydataProviderApp50|(미할당)|
|mydataProviderApp51|(미할당)|
|mydataProviderApp52|(미할당)|
|mydataProviderApp53|(미할당)|
|mydataProviderApp54|(미할당)|
|mydataProviderApp55|(미할당)|
|mydataProviderApp56|(미할당)|
|mydataProviderApp57|(미할당)|
|mydataProviderApp58|(미할당)|
|mydataProviderApp59|(미할당)|
|mydataProviderApp60|(미할당)|

• (마이데이터사업자 사전준비 필요 사항) iOS용 마이데이터서비스앱에 60개의 앱 URL스킴(mydataProviderApp01~mydataProviderApp60)을 info.plist에 등록한 후 앱을 배포 (안드로이드용은 불필요, 자체 앱URL스킴 그대로 사용) ⇨ 이후 앱인증 방식으로 개별인증을 제공하는 정보제공자가 신규로 추가된다 하더라도 마이데이터서비스앱 수정이 불필요

• (앱인증 방식 개별인증을 제공하는 정보제공자 사전준비 필요 사항) 마이데이터 지원 기관을 통해 앱URL스킴을 할당받은 후 정보제공자의 앱에 해당 앱URL스킴을 적용

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

|client_id|redirect URI|app_scheme|state|
|---|---|---|---|
|iOS/Android|URL|Callback URL|state|

# 1. 정보주체

마이데이터서비스 회원가입 후 해당 마이데이터사업자의 서비스앱에 로그인하여 전송요구 또는 전송요구 변경할 정보제공자를 선택

# 2. 서비스 서버

인가코드 발급을 요청(개별인증-001 API 호출)

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|개별인증-001|/oauth/2.0/authorize|인가코드 발급(정보제공API 호출에 필요한 접근토큰 발급 시 필요)|정보제공자|CI값, 클라이언트 ID, Callback URL, 앱스킴, 상태값 등|인가코드, 상태값 등|

- 동일한 마이데이터서비스를 복수 개의 앱으로 제공하는 마이데이터 사업자의 경우, 반드시 현재 정보주체(고객)이 실행 중인 마이데이터 서비스 앱의 앱스킴을 전송

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# ③ (정보제공자 서버) 마이데이터사업자가 전송한 개별인증-001 파라미터들 검증

- 마이데이터사업자가 전송한 redirect_uri가 해당 마이데이터사업자의 Callback URL 목록*에 속해있는지 여부 검증
- * 마이데이터사업자가 종합포털에 등록한 Callback URL들(최대 4개까지 등록 가능)
- 마이데이터사업자가 전송한 app_scheme이 해당 마이데이터사업자의 앱스킴 목록*에 속해있는지 여부 검증
- * 마이데이터사업자가 종합포털에 등록한 앱스킴들 (일부 기관의 경우, 동일한 마이데이터서비스를 복수 개의 앱(인터넷뱅킹앱, 간편뱅킹앱 등)로 제공하기 때문에 복수 개 등록 가능)

# ④ (정보제공자 서버) 정보제공자의 앱으로 전환하기 위한 정보를 전송 (②에 대한 응답)

# ㅇ 응답메시지 명세

- HTTP 응답코드 : 200, Content-type : application/json; charset=UTF-8

|HTTP 항목명|항목설명|
|---|---|
|org_code|정보제공자 기관코드|
|app_scheme_iOS|iOS 앱 URL 스킴|
|install_url_iOS|iOS 앱설치 URL|
|app_scheme_android|안드로이드 앱 URL 스킴|
|install_url_android|안드로이드 앱설치 URL|

# 필수 타입(길이) 설명 (비고)

|항목|필수|타입(길이)|설명 (비고)|
|---|---|---|---|
|org_code|Y|aN (10)|정보제공자 기관코드|
|app_scheme_iOS|Y|aNS (500)|할당받은 iOS 앱URL 스킴|
|install_url_iOS|Y|aNS (100)|정보제공자 앱이 설치되지 않은 경우, 앱설치 URL(앱스토어)|
|app_scheme_android|Y|aNS (500)|안드로이드 앱URL 스킴 (할당받은 앱URL 스킴을 사용할 필요 없고, 자체 안드로이드앱의 앱URL 스킴 회신)|
|install_url_android|Y|aNS (100)|정보제공자 앱이 설치되지 않은 경우, 앱설치 URL(구글 플레이스토어)|

# 정보제공자 서버는 필요시 정보제공자 앱에 전달하기 위한 정보들 (예: 세션값, 개별인증-001 시 전달받은 app_scheme, 거래고유번호)

# 금융분야 마이데이터 표준API 규격

등)을 app_scheme_iOS, app_scheme_android 앱스킴 내 파라미터에 포함하여 전달 가능

- 마이데이터사업자는 ④에서 회신되는 정보*를 기반으로 웹방식 또는 앱방식 개별인증을 처리

* 웹방식은 HTTP 302 응답코드로 정보제공자 인증사이트 URL을 회신하며, 앱방식은 HTTP 200 응답코드로 앱URL스킴, 앱설치 URL 등을 JSON으로 회신

⑤ (서비스 앱) 마이데이터서비스 앱에서 정보제공자 앱으로 전환

- 마이데이터사업자는 ④에서 전달받은 정보를 이용하여 정보제공자 앱 설치 여부 확인, 미설치 시 앱설치 화면 이동 및 정보제공자 앱으로 전환을 수행

⑥ (정보주체) 실행된 정보제공자 앱을 통해 인증을 수행

⑦ (정보제공자 앱) 정보주체가 입력한 인증정보를 정보제공자 서버로 전송

⑧ (정보제공자) 전달받은 인증정보 검증 등 개별인증 수행

- 정보제공자는 개별인증 완료 후 서비스 서버가 개별인증-001 API 호출 시(②) 전달한 CI값(x-user-ci)과 정보제공자가 보유하고 있는 정보주체(고객)의 CI값을 반드시 비교

⑨ (정보제공자) 개인신용정보 전송요구 또는 전송요구 변경 화면을 정보제공자 앱에 제공 (⑦에 대한 응답)

- 정보제공자는 신용정보법 제33조의2제5항에 의거하여 정보주체가 아

금융보안원 www.fsec.or.kr - 37 -

# 금융분야 마이데이터 표준API 규격

# 래의 내용을 특정할 수 있도록 화면 구성 필요

|특정 사항|정보주체 선택(또는 고지) 항목|
|---|---|
|정기적 전송을 요구하는지 여부 및 요구 시 그 주기|• 정기적 전송 요구 여/부 선택 • 주기|
|전송요구의 종료시점|• 전송요구의 종료시점|
|전송을 요구하는 목적|• 전송을 요구하는 목적|
|전송을 요구하는 개인신용정보의 보유기간|• 마이데이터사업자가 수집한 정보를 보유할 수 있는 기간|
|전송을 요구하는 개인신용정보|• 업권별 상이 (상세내용은 2.2- 의 “전송요구 scope” 내용 참조)|

# 업권별 “전송을 요구하는 개인신용정보”제공 화면

업권별 “전송을 요구하는 개인신용정보”제공 화면은 “2.2 인증규격 - 권한(scope)”의 전송요구 scope를 참고하여 구성 필요

# 전송요구 변경 시

전송요구 변경 시에는 기존 전송요구 시 정보주체가 선택했던 내역을 화면에 반영 필요

# 전송요구 내역 선택

⑩ (정보주체) 제공된 개인신용정보 전송요구 또는 전송요구 변경 화면을 통해 전송요구 내역 선택

# 정보제공자 앱

⑪ (정보제공자 앱) 정보주체가 특정한 전송요구 내역을 정보제공자 서버로 전송

# 정보제공자 서버

⑫~⑬ (정보제공자 서버) 전송요구 내역을 저장하고, 인가코드를 생성

⑭ (정보제공자 서버) 생성한 인가코드 및 state(상태값)을 정보제공자 앱에 회신(⑪에 대한 응답, ②에서 호출한 개별인증-001 API에 대한 회신*)

* RFC 6749를 준용하여 인가코드 및 state(상태값) 등은 리다이렉트로 서비스 서버에 최종 전달(개별인증-001 API의 응답메시지)

금융보안원 www.fsec.or.kr - 38 -

# 금융분야 마이데이터 표준API 규격

# 회신 예시

HTTP/1.1 302 Found
Location: https://마이데이터사업자_Callback_URL?code=인가코드&state=상태값&api_tran_id=거래고유번호

- 정보제공자 서버가 정보제공자 앱으로 전송 시, 자체적 판단하에 추가적으로 전달이 필요한 정보들(예:전환할 마이데이터사업자 앱 스킴 정보 등)을 헤더값 등에 설정하여 자율적으로 전달 가능

# ⑮

(정보제공자 앱) ⑭에서 전달받은 Callback URL로 리다이렉트

# ⑯~⑰

(서비스 서버) state(상태값) 검증 및 인가코드 저장 후 정보제공자 앱에게 ⑮에 대한 응답*

* 별도의 응답 상세 규격은 없으며, HTTP 응답코드 회신

# ⑱

(정보제공자 앱) ②에서 전달받은 마이데이터서비스 앱 스킴 (app_scheme)을 이용하여 마이데이터서비스 앱으로 전환

※ 앱방식 개별인증 성공, 실패 또는 사용자 취소로 인해 마이데이터서비스 앱(개별인증-001 API 호출 시 요청메시지 내 app_scheme에 설정된 앱스킴에 해당하는 앱)으로 전환 시 정보제공자 앱은 rsp_code 파라미터를 추가하여 마이데이터 앱에 그 결과를 전달

# 앱 전환 시 앱스킴 호출 예시

개별인증-001 호출 시 요청메시지로 전달된 app_scheme이 “mydataApp://action”이라고 가정할 경우 아래와 같이 rsp_code를 추가하여 마이데이터서비스 앱으로 전환

mydataApp://action?rsp_code=결과코드

# 결과코드 코드값

|코드값|설명|
|---|---|
|01|성공|
|02|실패|
|03|사용자 취소|

금융보안원 www.fsec.or.kr - 39 -

# 금융분야 마이데이터 표준API 규격

# 1. 접근토큰 발급

|API ID|개별인증-002|
|---|---|
|API명(URI)|/oauth/2.0/token|
|API 설명|정보제공API 호출을 위한 접근토큰 발급|
|API 제공자|정보제공자|
|요청 정보|인가코드, 서비스 자격증명, redirect_URI 등|
|응답 정보|접근토큰, scope 등|

- 마이데이터사업자가 다음 단계인 “개인신용정보 전송요구 내역 조회”를 수행하기 위해서는 scope에 해당 업권의 “자산목록 scope”가 포함되어 있어야 함 (2.2- 참조)

- 접근토큰(정보제공 API 호출용)은 정보주체의 전송요구 당 1개가 발급되기 때문에 정보제공자는 마이데이터사업자 서비스 별 정보주체별로 접근토큰 1개를 발급 및 관리* 필요 (마이데이터서비스 별 정보주체 : 접근토큰 = 1 : 1)

• 따라서 정보주체가 전송요구 변경 시 기발급된 접근토큰 및 리프레시토큰은 폐기**되고 새로운 접근토큰 및 리프레시토큰이 발급

* 정보주체가 수행하는 인증방식(개별인증 또는 통합인증)과 무관 (예: 개별인증을 통해)

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

전송요구하여 접근토큰 및 리프레시토큰을 발급받은 후, 통합인증을 통해 전송요구 변경 시 기존 개별인증을 통해 발급받은 접근토큰 및 리프레시토큰은 폐기

이 경우 마이데이터사업자가 접근토큰 폐기 API(개별인증-004, 철회 시 사용)를 호출하여 폐기하는 것이 아닌, 정보제공자가 내부적으로 기존 접근토큰 및 리프레시토큰 폐기 후 새로운 접근토큰 및 리프레시토큰을 발급

업권이 복수 개인 기관(예:겸영여신업자 등)의 경우, 업권별로 접근토큰을 별도 발급 및 관리 필요

# 접근토큰 발급 예시

<가정> 1) 정보주체 A는 마이데이터사업자 M1의 서비스 S1, S2 및 마이데이터사업자 M2의 서비스 S3에 가입

2) A가 정보제공자 P에게 전송요구 또는 전송요구 변경

# 예시

- 예시1) S1 이용고객인 A가 전송요구 : P는 접근토큰/리프레시토큰(A-M1-S1-P) 발급
- 예시2) S2 이용고객인 A가 전송요구 : P는 접근토큰/리프레시토큰(A-M1-S2-P) 발급
- 예시3) S3 이용고객인 A가 전송요구 : P는 접근토큰/리프레시토큰(A-M2-S3-P) 발급
- 예시4) S2 이용고객인 A가 전송요구 변경 : P는 기존 접근토큰/리프레시토큰 (A-M1-S2-P)을 폐기하고 새로운 접근토큰/리프레시토큰(A-M1-S2-P‘)을 발급

# 개인신용정보 전송요구 내역 조회

개별인증의 특성상 마이데이터사업자는 전송요구 내역을 알 수 없어 정보제공자를 대상으로 전송요구 내역 조회 필요

개별인증의 경우 정보제공자가 제공하는 개인신용정보 전송요구 화면을 통해 정보주체가 직접 정보제공자에게 전송을 요구하기 때문에 현재 시점에서 마이데이터사업자는 전송요구 내역을 알 수 없음

금융보안원 www.fsec.or.kr - 41 -

# 금융분야 마이데이터 표준API 규격

# 특정 사항

|조회 API|정기적 전송을 요구하는지 여부 및 요구 시 그 주기|전송요구의 종료시점|전송을 요구하는 목적|전송을 요구하는 개인신용정보의 보유기간|
|---|---|---|---|---|
|• 업권 공통|• 정보제공-공통-002 API 호출|• 업권별 상이 (상세내용은 2.2- 의 “전송요구 scope” 내용 참조)| | |

# 1. 마이데이터사업자

마이데이터사업자는 “전송을 요구하는 개인신용정보”를 제외한 개인신용정보 전송내역 특정 사항들을 정보제공자로부터 회신(정보제공-공통-002 API 호출)

|API ID|정보제공-공통-002 API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|API ID|/consents|개인신용정보 전송요구 특정사항 회신|정보제공자|접근토큰|정기적 전송을 요구하는지 여부 및 요구 시 그 주기, 전송요구의 종료시점, 전송을 요구하는 목적, 전송을 요구하는 개인신용정보의 보유기간|

# 2. 마이데이터사업자

마이데이터사업자는 일부 “전송을 요구하는 개인신용정보”를 접근토큰 발급 시 회신받은 scope를 통해 확인 가능

|업권|전송 대상|scope|전송요구 내역 확인 가능 여부|비고|
|---|---|---|---|---|
|계좌 정보|bank.deposit| |불 가|scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가|
|투자상품|bank.invest| | | |
|대출상품|bank.loan| |불 가|scope만으로는 정보주체가 어떤 카드번호를 선택했는지 확인 불가|
|개인형IRP|bank.irp| | | |
|카드 정보|card.card| |불 가|scope만으로는 정보주체가 어떤 카드번호를 선택했는지 확인 불가|
|선불카드|card.prepaid| |불 가|scope만으로는 정보주체가 어떤 선불카드를 선택했는지 확인 불가|
|포인트 정보|card.point| |가 능|“card.point”가 scope에 포함된 경우 : 포인트 정보를 전송요구 했음을 의미|

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

|항목|API|가능/불가|설명|
|---|---|---|---|
|청구 및 결제|card.bill|가능|“card.bill”이 scope에 포함된 경우 : 청구 정보 및 결제 정보를 전송요구 했음을 의미|
|대출상품 정보|card.loan|가능|“card.loan”이 scope에 포함된 경우 : 대출상품 정보를 전송요구 했음을 의미|
|계좌 정보|invest.account|불가|scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가|
|개인형IRP 정보|invest.irp|불가|scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가|
|보험 정보|insu.insurance|불가|scope만으로는 정보주체가 어떤 증권번호를 선택했는지 확인 불가|
|대출상품 정보|insu.loan|불가|scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가|
|개인형IRP 정보|insu.irp|불가|scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가|
|선불전자지급 수단 정보|efin.prepaid|불가| |
|결제 정보|efin.paid|불가| |
|대출상품 정보|capital.loan|불가| |
|운용리스 정보|ginsu.insurance|불가| |
|통신 정보|telecom.mgmt|불가| |
|P2P 대출 정보|p2p.lending|불가| |
|인수채권/금전대부 정보|bond.bond|불가| |
|인수채권/금전대부 정보|usury.bond|불가| |

scope만으로는 정보주체가 어떤 선불전자 지급수단을 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 계정을 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 계좌번호를 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 증권번호를 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 통신 계약을 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 대출 계약을 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 인수채권/금전대부 자산을 선택했는지 확인 불가

scope만으로는 정보주체가 어떤 인수채권/금전대부 자산을 선택했는지 확인 불가

# ③ 마이데이터사업자는

scope로 전송요구 내역 확인이 불가한 “전송을 요구하는 개인신용정보”내역을 정보제공자로부터 회신 (자산별 전송요구 여부(is_consent) 값을 통해 전송요구 내역 확인 가능)

- (은행) 계좌 목록 조회 API 및 개인형IRP 계좌 목록 조회 API 호출

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# 1. 계좌 관련 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|은행-001|/accounts|정보주체가 보유한 계좌(수신계좌, 투자상품계좌, 대출상품계좌) 목록 조회|정보제공자|접근토큰|보유 계좌별 계좌번호, 전송요구 여부, 상품명, 계좌구분 등|
|IRP-001|/irps|정보주체가 보유한 개인형IRP 계좌 목록 조회|정보제공자|접근토큰|보유 개인형IRP 계좌별 계좌번호, 전송요구 여부, 상품명 등|

# 2. 카드 관련 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|카드-001|/cards|정보주체가 보유한 카드 목록 조회|정보제공자|접근토큰|보유 카드별 카드 식별자, 카드번호, 전송요구 여부, 상품명, 본인/가족구분 등|
|선불-001|/prepaid|정보주체가 가입한 선불카드 목록 조회|정보제공자|접근토큰|보유 선불카드별 식별자, 전송요구 여부, 상품명, 발급일자/기명일자 등|

# 3. 금융투자 관련 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|금투-001|/accounts|정보주체가 보유한 계좌 목록 조회|정보제공자|접근토큰|보유 계좌별 계좌번호, 전송요구 여부, 상품명, 계좌구분 등|

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# API 목록

# 1. 개인형 IRP 계좌 목록 조회 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|IRP-001|/irps|정보주체가 보유한 개인형IRP 계좌 목록 조회|정보제공자|접근토큰|보유 개인형IRP 계좌별 계좌번호, 전송요구 여부, 상품명 등|

# 2. 보험 목록 조회 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|보험-001|/insurances|정보주체가 보유한 보험상품 계약 목록 조회|정보제공자|접근토큰|보유 계약별 증권번호, 전송요구 여부, 상품명, 보험종류 구분 등|

# 3. 대출상품 목록 조회 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|보험-008|/loans|정보주체가 계약한 대출상품 목록 조회|정보제공자|접근토큰|보유 계약별 계좌번호, 전송요구 여부, 상품명 등|

# 4. 선불전자지급수단 목록 조회 API

|API ID|API명(URI)|API 설명|API 제공자|요청 정보|응답 정보|
|---|---|---|---|---|---|
|전금-001|/prepaid|정보주체가 가입한 선불전자지급수단 목록 조회|정보제공자|접근토큰|보유 선불전자지급수단별 식별값(권면ID), 전송요구 여부, 계정식별값 등|

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# 1. API 목록

# 1.1 전금-101

|API명(URI)|/paid|
|---|---|
|API 설명|정보주체가 가입한 계정 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|계정 식별값, 전송요구 여부, 결제수단 등록여부 등|

# 1.2 할부금융-001

|API명(URI)|/loans|
|---|---|
|API 설명|정보주체가 보유한 계좌 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|보유 계좌별 계좌번호, 전송요구 여부, 상품명, 계좌구분 등|

# 1.3 보증보험-001

|API명(URI)|/insurances|
|---|---|
|API 설명|정보주체가 보유한 보증보험 계약 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|보유 계약별 증권번호, 전송요구 여부, 상품명, 보험종류 구분 등|

# 1.4 통신-001

|API명(URI)|/telecoms|
|---|---|
|API 설명|정보주체가 가입한 통신 서비스 계약 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|보유 계약별 계약관리번호, 전송요구 여부, 가입번호, 통신구분 등|

# 1.5 P2P-001

|API명(URI)|/lendings|
|---|---|
|API 설명|정보주체가 가입한 P2P 대출 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|보유 계약별 대출계약번호, 전송요구 여부, 상품명, 상품유형 등|

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# 1. (인수채권/대부) 인수채권/금전대부 목록 조회 API 호출

|API ID|채권-001|
|---|---|
|API명(URI)|/bonds|
|API 설명|정보주체가 가입한 인수채권/금전대부 목록 조회|
|API 제공자|정보제공자|
|요청 정보|접근토큰|
|응답 정보|채권번호, 전송요구 여부, 기관구분, 채권인수일(최초대출일) 등|

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# 3.2.1.2 개인신용정보 전송요구 철회

ㅇ 정보주체가 마이데이터사업자의 서비스앱을 통해 전송요구를 철회하는 경우, 마이데이터사업자는 해당 정보제공자에게 접근토큰 및 리프레시토큰을 폐기할 것을 즉시 요청(개별인증-004 API 호출*)

* 정보주체가 직접 철회 요청 시에만 개별인증-004 API 호출 (전송요구 변경으로 새로운 접근토큰 및 리프레시토큰이 발급되는 경우, 기존 접근토큰 및 리프레시토큰 폐기를 위해 본 API를 호출하는 것은 아니며, 정보제공자가 내부적으로 기존 접근토큰 및 리프레시토큰을 자체 폐기 후 새로운 접근토큰을 발급)

|API ID|개별인증-004|
|---|---|
|API명(URI)|/oauth/2.0/revoke|
|API 설명|접근토큰 폐기|
|API 제공자|정보제공자|
|요청 정보|폐기하고자 하는 토큰, 서비스 자격증명 등|
|응답 정보|-|

금융보안원 www.fsec.or.kr - 48 -

# 금융분야 마이데이터 표준API 규격

# 3.2.2 통합인증 시

# 통합인증 방식 개요

- (통합인증방식) 정보주체가 마이데이터 서비스를 이용하기 위해, 통합인증 기관으로부터 발급받은 통합인증 수단을 이용하여 1회 인증으로 다수 정보제공자에게 개인신용정보 전송요구권 행사 및 인증을 동시에 수행하는 방식
- (통합인증기관) 다수 정보제공자가 정보주체를 공통적으로 식별 및 인증하는데 필요한 식별정보(정보통신망법 하위 고시의 ‘연계정보(CI)’)를 적법하게 제공 가능한 기관 중에 충분한 보안 수준 등을 갖춰 통합인증기관으로 참여한 기관
- 정보통신망법상 인증서 본인확인기관(예: 舊 공인인증기관 등)이 해당하며, 향후 적법하게 CI를 제공할 수 있는 전자서명법상 운영기준의 준수 사실을 인정받은 전자서명인증사업자 등으로 통합인증기관의 범위를 확대 예정
- (통합인증수단, 통합인증서) 통합인증기관으로 참여한 인증서 본인확인기관이 발급한 인증서 본인확인수단(공동인증서(범용, 은행, 증권) 등)

# 3.2.2.1 개인신용정보 전송요구 또는 전송요구 변경

※ 통합인증을 이용한 개인신용정보 전송요구 또는 전송요구 변경 절차는 “[별첨1] 인증서 본인확인 기반 통합인증 절차 및 규격”, “[별첨2] 사설인증서 기반 통합인증 절차 및 규격” 참조

# 3.2.2.2 개인신용정보 전송요구 철회

ㅇ “3.2.1.2. 개인신용정보 전송요구 철회”와 동일

- 통합인증을 통해 발급된 접근토큰을 갱신 또는 폐기하고자 하는 경우, 개별인증-003 또는 개별인증-004 API 호출

금융보안원 www.fsec.or.kr - 49 -

# 금융분야 마이데이터 표준API 규격

# 3.3 개인신용정보 전송

# 3.3.1 전송유형

□ 정보주체 개입 (비정기적 전송)

1. (정보주체) 마이데이터사업자의 서비스앱에 접속하여 자산정보, 거래 내역 등 조회
2. (마이데이터사업자) 정보제공자로부터 발급받은 접근토큰을 이용하여 업권별 정보제공 API를 호출
- 비정기적 전송 API를 구분하기 위한 헤더 값(“x-api-type”)을 반드시 설정
3. (정보제공자) 접근토큰 유효성과 요청 대상 정보의 정당성을 검증한 후 마이데이터사업자가 요청한 정보를 회신

□ 정보주체 미개입 (정기적 전송)

1. (마이데이터사업자) 정보제공자로부터 발급받은 접근토큰을 이용하여 업권별 정보제공 API를 호출
- 정기적 전송 여부를 구분하기 위한 헤더 값(“x-api-type: scheduled”)을 반드시 설정
- 정기적 전송주기는 송·수신되는 정보의 특성에 따라 기본정보 또는 추가정보로 분류(API별 정기적 전송주기 분류 기준은 제4장 참조)

금융보안원 www.fsec.or.kr - 50 -

# 금융분야 마이데이터 표준API 규격

|분 류|설 명|기본주기*|
|---|---|---|
|기본|생성/수정/삭제가 빈번하게 발생하지 않는 정보(예: 상품정보, 가입정보 등)를 송·수신하는 API|주 1회|
|추가|생성/수정/삭제가 빈번하게 발생하는 정보(예: 잔액, 거래내역 등)를 송·수신하는 API|주 1회**|

* 고객별/자산별 API 호출 전송주기를 의미(예: 정보주체가 A은행의 수신계좌 10개를 전송요구한 경우, 마이데이터사업자는 계좌별로 총 10번씩 수신계좌 기본정보/추가정보/거래내역 조회 API를 매 주 호출 가능). 또한, 페이지네이션으로 인해 API를 복수 번 호출 시에는 1회 전송으로 간주

** 시행 초기 전산 용량 등을 감안하여 초기에는 주 1회로 고정. 추후 논의 과정을 거쳐 고객의 선택권을 다양화할 예정

# ② (정보제공자) 접근토큰

유효성과 요청 대상 정보의 정당성을 검증한 후 마이데이터사업자가 요청한 정보를 회신

# 3.3.2 전송기준

# □ 정보주체 개입(비정기적 전송) 시 조회기준

|분 류|조회기준|
|---|---|
|① 전송요구 직후|전송요구 시점 기준 과거 최대 12개월까지|
|② 로그인 또는 새로고침 시|현재 시점 기준 이전 로그인 또는 새로고침시점까지 (최대 12개월)|
|③ 특정자산 거래내역 조회 시|과거 최대 5년까지|

# ① 전송요구 직후

고객이 전송요구 직후 통합자산조회 서비스를 제공받기 위해 마이 데이터사업자가 일정 데이터를 즉시 수집할 수 있는 조회기준

금융보안원 www.fsec.or.kr - 51 -

# 금융분야 마이데이터 표준API 규격

# 1. 전송요구한 자산들에 대한 기본정보/추가정보/거래내역 조회 API 호출 가능

* 거래내역 조회 API : 내역관련(거래내역, 청구정보, 카드승인내역, 결제내역 등) 조회가 가능한 API로써 요청메시지 내 조회기간(from_date/to_date 또는 from_month/to_month) 항목이 있는 API

# 2. 거래내역 조회 API

거래내역 조회 API는 일자(Date) 기준, 월(Month) 기준 API 구분 없이 전송요구 시점(현재 시점) 기준 최대 12개월 전까지의 정보를 조회·수집 가능하며, 1회 호출 시 설정 가능한 조회기간(From/To) 역시 최대 12개월 가능

* 예) 고객이 2021.12.1. 전송요구 시 : 2020.12.2. ~ 2021.12.1. 기간의 거래내역 조회 가능 (12개월치 정보를 API 1회 호출하여 조회)

* 단, 조회기간을 분리하여 동시에 여러 API를 호출함으로써 정보제공자에게 부하를 주는 행위 금지 (예: 12개월을 1개월 단위로 From/To를 나누어 동시에 12번의 API를 호출)

# 3. 정보제공자의 API 응답 오류

정보제공자의 API 응답 오류, 지연 등으로 수집이 실패한 경우, 마이데이터사업자는 최초 1회 조회·수집 성공 시까지 조회 API 재호출 가능

# 4. 마이데이터사업자의 헤더 값 설정

마이데이터사업자는 헤더 값(“x-api-type: user-consent”)을 반드시 설정

# 5. 로그인 또는 새로고침 시

고객이 로그인 시 또는 화면 새로고침 시 최신정보의 통합자산조회 서비스를 제공받기 위해 마이데이터사업자가 최신 데이터를 즉시 수집할 수 있는 조회기준

- 전송요구한 자산들에 대한 기본정보/추가정보/거래내역 조회 API 호출 가능

금융보안원 www.fsec.or.kr - 52 -

# 금융분야 마이데이터 표준API 규격

- 거래내역 조회 API는 일자(Date) 기준, 월(Month) 기준 API 구분 없이 현재 시점 기준 최대 12개월 전까지의 정보를 조회·수집 가능하며, 1회 호출 시 설정 가능한 조회기간(From/To) 역시 최대 12개월 가능

* 예) 고객이 2021.12.1. 최종 접속 후 2022.10.15. 로그인 시 : 2021.12.1. ~ 2022.10.15. 기간의 거래내역 조회 가능 (API 1회 호출하여 조회)

* 단, 조회기간을 분리하여 동시에 여러 API를 호출함으로써 정보제공자에게 부하를 주는 행위 금지 (예: 3개월을 1개월 단위로 From/To를 나누어 동시에 3번의 API를 호출)

- 정보제공자의 API 응답 오류, 지연 등으로 수집이 실패한 경우, 마이데이터사업자는 최초 1회 조회·수집 성공 시까지 조회 API 재호출 가능

- 마이데이터사업자는 헤더 값(“x-api-type: user-refresh”)을 반드시 설정

# ③ 특정자산 거래내역 조회 시

고객이 마이데이터서비스 앱에서 특정자산(계좌, 카드 등)에 대한 거래내역을 직접 조회*하는 경우에 대한 조회기준

* 기간을 설정하고 조회 버튼 클릭, 다음페이지(또는 더보기) 버튼 클릭 등 고객이 서비스 앱에서 직접 이벤트를 발생시키는 경우

- 고객이 마이데이터서비스앱에서 선택한 자산에 대한 기본정보/ 추가정보/ 거래내역 조회 API 호출 가능

- 거래내역 조회 API는 일자(Date) 기준, 월(Month) 기준 API 구분 없이 1회 호출 시 설정 가능한 조회기간(From/To)은 과거 최대 5년 전까지 가능

* 예) 고객이 2021.12.1. 접속하여 계좌번호 A의 거래내역 직접 조회 시 : 최대 2016.12.2. ~ 2021.12.1. 조회기간(From/To)을 설정하여 API 호출이 가능

금융보안원 www.fsec.or.kr - 53 -

# 금융분야 마이데이터 표준API 규격

- 다만, 고객이 조회기간(From/To)을 과거 5년 전까지 설정하여 조회 요청하더라도 마이데이터사업자가 한 번에 5년치 정보를 수집할 수 있는 것을 의미하는 것은 아니며, 고객이 명시적으로 조회를 요청한 내역들만 수집 가능

* 고객이 조회기간을 5년 설정했더라도 고객이 직접 다음페이지(또는 더보기) 버튼 클릭 시 페이지네이션에 의해 순차적으로 거래내역 API가 호출이 되며, 고객이 더 이상 요청하지 않는 경우 페이지네이션 중단

* 조회기간을 분리하여 동시에 여러 API를 호출함으로써 정보제공자에게 부하를 주는 행위 금지 (예: 12개월을 1개월 단위로 From/To를 나누어 동시에 12번의 API를 호출)

- 불필요한 API 호출을 방지하기 위해 마이데이터사업자는 「① 전송 요구 직후」, 「② 로그인 또는 새로고침 시」 및 「정기적 전송」을 통해 이미 수집한 거래내역은 조회기간에서 제외할 필요

* 예) 고객이 2021.12.1. 접속하여 계좌번호 A의 거래내역을 2016.12.2. ~ 2021.12.1. 기간 조회 시, 마이데이터사업자가 최근 12개월 거래내역을 이미 수집한 상태라면 조회기간 (From/To)을 2016.12.2. ~ 2020.12.1. 로 설정하여 API 호출

* 「③ 특정자산 거래내역 조회」를 통해 수집한 거래내역의 경우, 조회기간 관리가 힘들어 조회기간 제외대상에 미포함

- 마이데이터사업자는 헤더 값(“x-api-type: user-search”)을 반드시 설정

# 정보주체 미개입(정기적 전송) 시 조회기준

ㅇ 전송요구한 자산들에 대한 기본정보/추가정보/거래내역 조회 API 호출 가능

- 정보제공자의 API 응답 오류, 지연 등으로 수집이 실패한 경우, 마이데이터사업자는 최초 1회 조회·수집 성공 시까지 조회 API 재호출 가능

금융보안원 www.fsec.or.kr - 54 -

# 금융분야 마이데이터 표준API 규격

# 1. 거래내역 조회

API의 1회 호출 시 설정 가능한 조회기간(From/To)은 다음과 같음

- 월(Month) 기준 API 조회기간(from_month/to_month) : 최대 3개월
- 일자(Date) 기준 API 조회기간(from_date/to_date) : 최대 31일로 하되, 다음 API들은 예외적으로 최대 3개월 가능

|업권|API ID|API 명|URI|
|---|---|---|---|
|은행|은행-010|대출상품계좌 거래내역 조회|/accounts/loan/transactions|
|보험|보험-006|보험 거래내역 조회|/insurances/transactions|
|보험|보험-007|자동차보험 거래내역 조회|/insurances/car/transactions|
|보험|보험-011|대출상품 거래내역 조회|/loans/transactions|
|할부금융|할부금융-004|대출상품계좌 거래내역 조회|/loans/transactions|
|할부금융|할부금융-006|운용리스 거래내역 조회|/loans/oplease/transactions|
|보증보험|보증보험-003|보증보험 거래내역 조회|/insurances/transactions|
|P2P|P2P-004|P2P 대출 거래내역 조회|/lendings/transactions|
|인수채권/대부|채권-003|인수채권/금전대부 거래내역 조회|/bonds/transactions|

# 3.3.3 정기적 전송 가능 시간대

정기적 전송으로 인한 트래픽 과부하를 줄이기 위해 정보제공자별 정기적 전송 가능 시간대를 개별 적용(종합포털에 등록)하되, 최소 일 6시간 이상* 시간대 설정

* 정기적 전송 가능 시간대가 짧을수록 부하는 더 집중될 수 있으므로, 정보제공자는 이를 고려하여 시간대를 설정할 필요

마이데이터사업자는 종합포털로부터 정보제공자의 정기적 전송 가능 시간대 정보(지원-002 API 호출)를 수신한 후 해당 시간대를 준용하여 정기적 전송 요청

금융보안원 www.fsec.or.kr - 55 -

