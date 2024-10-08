# 금융분야 마이데이터 표준API 규격

# 제1장. 개 요

# 1.1 목 적

□ 본 명세서는 정보주체가 개인신용정보 전송요구 시 마이데이터사업자에게 안전성과 신뢰성이 보장될 수 있는 방식으로 정보를 제공하기 위해 요구되는 표준 API 규격을 안내함을 목적으로 함

# 1.2 범 위

□ (적용대상) 본 표준 API 명세는 개인신용정보를 보유하고 있는 정보제공자 및 개인신용정보를 수신받는 마이데이터사업자를 대상으로 함

□ (적용범위) 정보제공자, 마이데이터사업자 및 통합인증기관 등은 정보주체가 마이데이터 서비스를 이용할 수 있도록 본 표준 API 명세의 인증 API, 업권별 정보제공 API 및 지원 API를 구현

※ API를 이용하지 않은 개인신용정보 전송방식과 절차, 규격 등은 본문서에서 다루지 않음

# 1.3 API 규격 유지관리

□ (규격의 개정) 본 규격은 신용정보법 등 관계 법령의 개정, 기술 발전, 서비스 요구사항 등 필요에 따라 개정함

□ (규격의 개정 방법) 규격의 개정(‘21.8월 이후)은 정보제공자, 마이데이터사업자 등 이해관계자로 구성된 ‘데이터 표준 API 규격 검토위원회(가칭)’에서 검토 후, 금융위원회의 승인으로 개정함

금융보안원 www.fsec.or.kr - 1 -

# 금융분야 마이데이터 표준API 규격

# 1.4 용어 정의

□ 본 규격서에서 사용하는 용어는 아래와 같으며 그 외 용어는 신용정보법 및 관련 법규의 용어를 준용함

- (개인신용정보) 금융거래 등 상거래에서 개인인 정보주체의 신용, 거래 내용, 거래능력 등을 판단할 수 있는 정보
- (고객, 정보주체) 처리된 개인신용정보로 알아볼 수 있는 정보주체로 개인신용정보 전송 요구권을 행사하는 자(신용정보법 상 개인인 신용정보주체)
- (정보제공자) 고객의 개인신용정보 전송요구에 따라 보유하고 있는 고객의 개인신용정보를 정보수신자에게 전송하는 자 (신용정보법 상 신용정보제공·이용자)
- (정보수신자) 고객의 개인신용정보 전송요구에 따라 정보제공자로부터 고객의 개인신용정보를 제공받는 자
- (마이데이터사업자) 금융위원회로부터 본인신용정보업 허가를 받아 고객에게 개인신용정보 통합조회서비스(이하 마이데이터서비스)를 제공하는 자
- (마이데이터서비스) 개인신용정보 통합조회서비스 등 마이데이터사업자가 고객에게 제공하는 서비스
- (본인인증, 인증) 고객이 정보제공자에게 개인신용정보 전송을 요구할 때, 고객이 해당 개인신용정보의 소유자임을 정보제공자가 확인하기 위한 방법(개별인증과 통합인증으로 구분)
- (통합인증) 고객이 통합인증기관이 발급한 인증수단을 이용하여 1회 인증만으로 다수의 정보제공자에 개인신용정보 전송요구 및 인증을 수행하는 방식

금융보안원 www.fsec.or.kr - 2 -

# 금융분야 마이데이터 표준API 규격

- (통합인증기관) 고객에게 통합인증수단을 발급하고 정보제공자의 요청에 따라 통합인증수단 검증을 통해 공통의 고객 식별정보를 적법하게 제공 가능하며, 통합인증에 요구되는 충분한 보안수준을 갖춘 기관 중 별도의 절차에 따라 통합인증기관으로 참여한 기관
- (API, Application Programming Interface) 마이데이터사업자와 정보제공자간 개인신용정보를 송수신하기 위한 미리 정의된 표준화된 전송규격 및 절차
- (인증 API) 고객이 개인신용정보 전송요구 및 본인인증을 수행하기 위해 필요한 API로, 개별인증 API와 통합인증 API로 구분
- (업권별 정보제공 API) 고객의 개인신용정보 전송요구에 의거, 정보 제공자가 마이데이터사업자에게 개인신용정보를 전송하기 위해 필요한 API
- (지원 API) 종합포털이 마이데이터 산업을 지원하기 위해 필요한 API로, 종합포털이 제공하는 API(마이데이터사업자 및 정보제공자가 호출)와 마이데이터사업자 및 정보제공자가 제공하는 API(종합포털이 호출)로 구분
- (마이데이터 종합포털) 정보제공자 및 마이데이터서비스의 등록 및 관리, 고객 개인신용정보 전송 요구 내역 일괄조회 등 마이데이터서비스 및 고객의 개인신용정보 전송요구를 지원하는 웹 기반 서비스(이하 종합포털)
- (중계기관) 마이데이터사업자의 API 요청에 대해 하나 이상의 정보제공자를 대신하여 고객의 개인신용정보를 중계하는 신용정보법 상 기관
- (TLS 인증서) 정보제공자와 마이데이터사업자 간 개인신용정보 전송

금융보안원 www.fsec.or.kr

# 금융분야 마이데이터 표준API 규격

# 시 상호인증 및 암호화 채널 형성을 위한 인증서

- (자격증명) API 요청 시 상호 간 자격을 인증하고 식별하기 위해 종합 포털로부터 발급받는 값
- (접근토큰) API를 이용하여 개인신용정보 전송을 요청한 마이데이터 사업자가 정보제공자가 보유하고 있는 해당 고객의 개인신용정보에 접근할 수 있는 자격이 있는지를 확인하기 위해 발급받는 정보

금융보안원 www.fsec.or.kr - 4 -

