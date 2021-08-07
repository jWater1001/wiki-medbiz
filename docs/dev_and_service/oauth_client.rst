OAuth Client
====================

.. contents:: 목차

OAuth Client 등록
--------------------

Medbiz 플랫폼은 인증을위해 OAuth2를 지원합니다.

다음은 OAuth Client를 등록하는 절차입니다.

#. `MEDBIZ 개발자 사이트 <https://dev.medbiz.or.kr/>`_ 에 접속하여 로그인합니다.

#. 상단에 'OAuth Clients' 메뉴를 클릭하고, 좌측에 '내 클라이언트' 메뉴를 클릭하여, 등록된 OAuth Client를 확인할 수 있습니다.

   .. figure:: static/oauth_client/new_oauth_03_check_my_client.png

#. 좌측에 '클라이언트 등록' 메뉴를 클릭하면, 신규 클라이언트 등록 페이지가 나옵니다.

   .. figure:: static/oauth_client/new_oauth_05_client_reg_page.png

#. '클라이언트 등록' 페이지의 내용을 작성합니다.

   .. figure:: static/oauth_client/new_oauth_07_fill_client_reg.png

   * WEB SERVER REDIRECT URL
      웹서버 리다이렉트 주소는 어플리케이션에서 MEDBIZ로그인을 요청한 후 사용자가 모든 로그인과정을 수행한 뒤 REDIRECT를 위해 돌아갈 주소입니다.
      서비스에서는 MEDBIZ에서 인증이 끝나는 경우 해당 URL을 바인딩하여 사용자정보를 로컬 DB에 입력하는등의 과정을 수행할 수 있습니다.

   * AUTH METHOD
      oAuth 2.0인증을 수행할 때 사용할 인증방식으로 보통의 경우 모두 체크를해도 상관없습니다.
      보다 상세한내용은 `oauth2.0 표준 안내 <https://oauth.net/>`_  문서를 확인하세요.
   
   * API SCOPE에 대한 내용은 다음 표를 참고합니다.

      .. note::
         기본적으로 MEDBIZ 로그인을 이용하기 위해서는 PROFILE을 선택해야합니다.

         해당 SCOPE는 클라이언트별로 적용되며 모든 SCOPE를 포함하여도 무방하지만 사용자가 로그인할 때 어플리케이션이 해당 권한에 접근함을 사용자에게 동의받습니다.
         
         따라서 사용하지 않는 SCOPE는 선택하지 않는것이 좋습니다.

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

#. '등록' 버튼을 클릭하여, 성공하면, 등록된 OAuth Client를 확인할 수 있습니다.

   .. figure:: static/oauth_client/new_oauth_09_client_reg_pending.png


Client ID & Secret 조회
------------------------------

Client ID와 Secret은 Application을 개발할 때, 어플리케이션에 OAuth2 인증을 적용하기 위해 사용됩니다.

Client ID와 Secret을 조회하는 과정은 아래와 같습니다.

#. "OAuth Clients > 내 클라이언트" 메뉴로 이동하여 OAuth Client 목록을 조회합니다.

   .. figure:: static/oauth_client/new_oauth_11_client_reg_active.png

      등록한 OAuth Client 정보

#. 조회하고자하는 oAuth Client를 선택합니다.

#. 오른쪽 상단에 'SHOW CLIENT ID & SECRET' 메뉴를 클릭하면, Client ID와 Secret을 확인할 수 있는 창이 표시됩니다.

   .. figure:: static/oauth_client/new_oauth_15_show_id_secret.png