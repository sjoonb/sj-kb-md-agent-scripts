# 첨부2 데이터 타입

|데이터 타입|설명|
|---|---|
|N|정수형 숫자 예) N(3) : 0, 5, -3, 10, -20, 999, -123 등 숫자 최대 3자리로 표현 가능 한 정수 (-999 ~ 999)|
|F|실수형 숫자로 길이에 precision(전체 자릿수)과 scale(소숫점 이하 자릿수) 표기 예) F(5, 2) : 1, -2, 1.2, -23.45, 456.78, 990.01, -99.1 등 최대 전체 5자리(정수부 3자리 + 소숫점 이하 2자리)로 표현 가능한 숫자 (-999.99 ~ 999.99)|
|AH|한글 (KSC-5601 코드 범위 내의 UTF-8 인코딩 문자열) AH의 길이는 Byte 길이를 의미하며, 따라서 3 Byte가 한글 1글자에 해당 (예: AH (30)은 한글 10글자를 의미)|
|A|알파벳 대문자|
|a|알파벳 소문자|
|AN|알파벳, 숫자 또는 그 조합 (단, 알파벳은 반드시 대문자로 설정)|
|aN|알파벳, 숫자 또는 그 조합|
|aNS|알파벳, 숫자 및 특수기호 조합|
|NS|숫자 및 특수기호 조합|
|Boolean|Boolean (true/false)|
|DATE|날짜 (YYYYMMDD)|
|DTIME|날짜 및 시분초 (YYYYMMDDhhmmss)|
|B64|Base64 인코딩 포맷|

* 본 규격의 요청메시지/응답메시지에서 JSON으로 전달되는 데이터들의 값(Value)은 데이터 타입에 상관없이 JSON String 타입으로 표현

# 예 시

- 정수형(N) JSON 전달 예시: “account_cnt": ”10“
- 실수형(F) JSON 전달 예시: “offered_rate”: “2.31”
- Boolean형 JSON 전달 예시: “is_concent”: “false”
- DTIME(날짜 및 시분초) JSON 전달 예시: “trans_dtime”: “20190821135030”

* 본 규격의 데이터 길이는 최대 길이를 의미

# 예 시

- trans_no (거래번호) : aN (64) 거래번호는 총 64자리가 아닌, 최대 64자리의 알파벳, 숫자 또는 그 조합을 의미
