===============
OpenAPI를 활용한 미세먼지 데이터 수집
===============

Aqicn Site를 이용한 날씨 수집
--------

- 미세먼지 데이터를 수집하기 위해 Aqicn의 OpenAPI를 이용해 수집을 진행

.. image:: docs/api/img/aqicn.png

- OpenAPI 활용 순서
1. OpenWeatherMap 가입
2. API Key 발급
3. API Key로 API 활용

- 가입
1. 가입은 OpenWeatherMap 홈페이지에서 진행 : `OpenWeatherMap 홈페이지 가입 <https://home.openweathermap.org/users/sign_up>`_.

- API Key 발급
1. 가입후 이메일 등록 절차가 완료 되면 Default API Key가 발급
2. 발급 정보는 `여기 <https://home.openweathermap.org/api_keys>`_ 에서 확인 가능
3. 기본 발급 토큰이외에 추가로 토큰을 생성 할 수 있음 

.. image:: docs/api/img/openweathermap_key.png

- API 활용
1. 가입후 이메일 등록 절차가 완료 되면 Default API Key가 발급
2. 활용할 API를 선정하고 API doc를 클릭하여 사용법을 확인

.. image:: docs/api/img/openweathermap_api.png

3. 현재 런던의 날씨 API를 받아 올 경우 : http://api.openweathermap.org/data/2.5/weather?q=London&appid=apiKey
4. 사용할 API 주소를 선택하고 검색 조건을 입력한 후 발급받은 API Key를 추가 후 http 전송하면 Json 형태의 결과 값을 얻음
5. 간단하게 웹 브라우저 (익스플로러, 크롬 등)에서 주소창에 입력후 결과 값 확인 가능 

.. image:: docs/api/img/openweathermap_result.png

- 발급 받은 API Key와 API 입력 값을 조합하여 웹 또는 앱에서 활용