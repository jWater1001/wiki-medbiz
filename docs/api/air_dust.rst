===============
OpenAPI를 활용한 미세먼지 데이터 수집
===============

Aqicn Site를 이용한 날씨 수집
--------

- 미세먼지 데이터를 수집하기 위해 Aqicn의 OpenAPI를 이용해 수집을 진행

.. image:: docs/api/img/aqicn.png

- OpenAPI 활용 순서
1. token 발급
2. token을 이용해 API 활용

- Token 발급
1. Token 발급은 Aqicn 홈페이지에서 진행 : `Aqicn 홈페이지 발급 <http://aqicn.org/data-platform/token/#/>`_.
2. 이메일과 이름을 입력하고 발급 신청하면 입력한 이메일 주소로 확인 메일 발송
3. 확인 메일 링크 클릭시 Token 확인 후 Token 정보를 따로 저장

- API 활용
1. 발급 받은 토큰 정보를 URI에 추가하여 사용
2. 활용할 API를 선정 : `API 종류 <http://aqicn.org/json-api/doc/#api-_>`_.

.. image:: docs/api/img/aqicn_api.png

3. 경도, 위도를 사용하여 미세먼지 데이터를 요청할 경우 : https://api.waqi.info/feed/geo:37.517530;126.719561/?token=개인토큰
4. 사용할 API 주소를 선택하고 검색 조건을 입력한 후 발급받은 Token를 추가 후 http 전송하면 Json 형태의 결과 값을 얻음
5. 간단하게 웹 브라우저 (익스플로러, 크롬 등)에서 주소창에 입력후 결과 값 확인 가능 

.. image:: docs/api/img/aqicn_exam.png


- 발급 받은 Token과 API 입력 값을 조합하여 웹 또는 앱에서 활용