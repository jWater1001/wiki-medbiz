
====================
OAuth Client
====================

.. contents:: 목차

--------------------
OAuth Client 등록
--------------------

Medbiz 플랫폼은 인증/권한 관리를 위해 OAuth2를 지원한다.

다음은 MedbizWatchAuth OAuth Client를 등록하는 과정이다.

1. Medbiz 개발자 사이트 홈페이지에 접속하여 로그인한다. 로그인 과정은 생략한다.

   * Medbiz 개발자 사이트 URL: https://dev.medbiz.or.kr

2. 상단에 'OAuth Clients' 메뉴를 클릭하고, 좌측에 '내 클라이언트' 메뉴를 클릭하여, 등록된 OAuth Client를 확인한다.

   .. figure:: oauth_client_files/new_oauth_03_check_my_client.png

3. 좌측에 '클라이언트 등록' 메뉴를 클릭하면, 클라이언트 등록 페이지가 나온다.

   .. figure:: oauth_client_files/new_oauth_05_client_reg_page.png

4. '클라이언트 등록' 페이지의 내용을 채운다.

   .. figure:: oauth_client_files/new_oauth_07_fill_client_reg.png

   API SCOPE에 대한 내용은 다음 표를 참고한다.

  .. table::
   :widths: auto

   ===============  ==================================================
   SCOPE            내용
   ===============  ==================================================
   DRIVE            Medbiz 드라이브에서 파일 보기 및 관리
   ---------------  --------------------------------------------------
   DRIVE_READONLY   Medibz 드라이브 파일 보기
   ---------------  --------------------------------------------------
   PROFILE          아이디 이메일주소 이름 성별 생일 보기
   ---------------  --------------------------------------------------
   DEVICE           사용중인 디바이스 정보 보기
   ---------------  --------------------------------------------------
   APPLICATION      사용중인 어플리케이션 보기
   ---------------  --------------------------------------------------
   GROUP            속해져있는 그룹 보기
   ---------------  --------------------------------------------------
   GROUP_PROFILE    사용자 기본 그룹 정보 보기
   ---------------  --------------------------------------------------
   HEALTH           개인 의료 데이터 읽기
   ===============  ==================================================

5. '등록' 버튼을 클릭하여, 성공하면, 등록된 MedbizWatchAuth OAuth Client가 화면에 보인다.

   .. figure:: oauth_client_files/new_oauth_09_client_reg_pending.png


--------------------
OAuth Client 활성화
--------------------

OAuth Client를 등록한 후에는 PENDING 상태이다. 운영자에게 연락하여 ACTIVE 상태로 변경해야 한다.

운영자 연락처는 `여기 <../contacts.html>`_ 를 참고한다.

OAuth Client를 활성화하는 과정은 다음과 같다.

1. "OAuth Clients > 내 클라이언트" 메뉴로 이동하면, 자신이 등록한 MedbizWatchAuth OAuth Client가 보인다.

   .. figure:: oauth_client_files/new_oauth_09_client_reg_pending.png

      등록한 OAuth Client 상태: PENDING

2. 운영자 연락처로 연락하여, 'ACTIVE' 상태로 변경 요청한다.
   요청할 때는 사용자 ID, OAuth Client 이름 등을 함께 알려준다.

3. 'ACTIVE' 상태로 변경되었다는 통지를 받으면, 다시, "OAuth Clients > 내 클라이언트" 메뉴로 이동하여, 'ACTIVE' 상태로 변경되었는지 확인한다.

   .. figure:: oauth_client_files/new_oauth_11_client_reg_active.png

      등록한 OAuth Client 상태: ACTIVE


------------------------------
Client ID & Secret 확인
------------------------------

Client ID와 Secret은 Application을 개발할 때, OAuth2 인증을 적용하기 위해 사용된다.

Client ID와 Secret을 활성화하는 과정은 다음과 같다.

1. "OAuth Clients > 내 클라이언트" 메뉴로 이동하면, 자신이 등록한 MedbizWatchAuth OAuth Client가 보인다.

   .. figure:: oauth_client_files/new_oauth_11_client_reg_active.png

      등록한 OAuth Client 정보

2. 오른쪽 상단에 'SHOW CLIENT ID & SECRET' 메뉴를 클릭하면, Client ID와 Secret을 확인할 수 있는 창이 뜬다.

   * MedbizWatchAuth의 Client ID & Secret
      - Client ID: 95f47de0cc94cf4290ba2526aaba50ac
      - Secret: 6a14457b35b6495eae12e2aeb723cbb3

   .. figure:: oauth_client_files/new_oauth_15_show_id_secret.png

      Client ID & Secret 확인창
