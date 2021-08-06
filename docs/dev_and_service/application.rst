Application
====================

.. contents:: 목차

Application 등록
--------------------

개발자는 Application(이하 App 또는 앱)을 개발하여 Medbiz 플랫폼에 등록할 수 있다. 등록 신청한 Application은 내부 검토를 거치면서 등록이 거부될 수도 있다.

다음은 MedbizWatchPM25 앱을 등록하는 과정이다.

1. Medbiz 개발자 사이트 홈페이지에 접속하여 로그인한다. 로그인 과정은 생략한다.

   * Medbiz 개발자 사이트 URL: https://dev.medbiz.or.kr


2. 상단에 'Applications' 메뉴를 클릭하고, 좌측에 '내 애플리케이션' 메뉴를 클릭하여, 등록된 앱을 확인한다.

   .. figure:: static/new_app_03_check_my_app.png

      "Applications > 내 애플리케이션"


3. 좌측에 '애플리케이션 등록' 메뉴를 클릭하면, '애플리케이션 등록' 페이지가 나온다.

   .. figure:: static/new_app_05_app_reg_page.png

   '애플리케이션 등록' 페이지


4. '애플리케이션 등록' 페이지의 내용을 채운다. 

   앱은 **사용 범위** 설정 조건에 따라서 "`Medbiz 홈페이지 <https://medbiz.or.kr>`_ > 건강정보 > 헬스케어 서비스"에 보여질지 여부가 결정된다.

   * 사용 범위
      - VFS(클라우드저장소)
         + 앱이 Medbiz 클라우드 저장소를 사용하면 선택
         + 일반 사용자가 클라우드 저장소를 사용하면, 앱이 '헬스케어 서비스'에 보임
      - Device(사용가능 제품)
         + 앱이 특정 제품을 필요로 하면 선택
         + 사용자가 구매한 디바이스를 플랫폼에 등록하면, 해당 디바이스와 관련된 앱이 '헬스케어 서비스'에 보임

   * 애플리케이션 파일 또는 링크
      - 애플리케이션 파일
         + 등록할 앱이 실행 파일 형태일 경우, 해당 파일을 선택
      - 애플리케이션 링크
         + 등록할 앱이 웹서비스 형태일 경우, 해당 URL을 설정

   다음 그림은 디바이스 선택 화면이다. 여기서는 'MedbizWatch'를 선택한다.

   .. figure:: static/new_app_06_select_device.png

      "사용 범위 > Device" 라이도 버튼 클릭 화면: 앱이 필요로 하는 특정 디바이스를 선택

   다음 그림은 '애플리케이션 등록' 페이지에 MedbizWatchPM25 앱 등록을 위해 필요한 내용을 모두 기록한 화면이다.

   .. figure:: static/new_app_07_fill_app_reg.png

      '애플리케이션 등록' 내용을 채운 화면


5. 하단에 있는 '등록' 버튼을 클릭하여, 앱 등록이 성공하면, 등록된 앱이 화면에 보인다.

   .. figure:: static/new_app_09_app_reg_pending.png

      앱 등록 성공, PENDING 상태


Application 활성화
--------------------

Application(이하 App 또는 앱)을 등록한 후에는 PENDING 상태이다. 운영자에게 연락하여 ACTIVE 상태로 변경해야 한다.

운영자 연락처는 `여기 <../contacts.html>`_ 를 참고한다.

앱을 활성화하는 과정은 다음과 같다.

1. "Applications > 내 애플리케이션" 메뉴로 이동하면, 등록한 MedbizWatchPM25 앱이 보인다.

   .. figure:: static/new_app_09_app_reg_pending.png

      등록한 앱의 상태: PENDING


2. 운영자 연락처로 연락하여, 'ACTIVE' 상태로 변경 요청한다.
   요청할 때는 사용자 ID, 앱 이름 등을 함께 알려준다.


3. 'ACTIVE' 상태로 변경되었다는 통지를 받으면, 다시, "Applications > 내 애플리케이션" 메뉴로 이동하여, 'ACTIVE' 상태로 변경되었는지 확인한다.

   .. figure:: static/new_app_11_app_reg_active.png

      등록한 앱의 상태: ACTIVE


등록한 앱 확인(일반 사용자)
------------------------------

등록된 앱은 일반 사용자가 사용할 수 있다.

다음은 일반 사용자가 개발자가 등록한 앱을 사용하는 과정이다. 여기서는 MedbizWatchPM25 앱을 다운로드하는 과정을 보여준다.

1. Medbiz 플랫폼 홈페이지(https://medbiz.or.kr)에 접속하여 로그인하다. 로그인 과정은 생략한다.


2. "건강정보 > 헬스케어 서비스" 메뉴로 이동하면, 이전에 개발자가 등록한 MedbizWatchPM25 앱이 보인다.

   ※ MedbizWatchPM25 앱을 등록할 때, 

   .. figure:: static/new_app_13_check_user_service.png

      등록한 OAuth Client 정보


3. 해당 앱의 '자세히보기' 버튼을 클릭하면, '애플리케이션 파일' 또는 '애플리케이션 링크' 항목을 확인할 수 있다.
   
   - 애플리케이션 파일: 개발자가 실행 파일 형태의 앱을 등록한 경우
   - 애플리케이션 링크: 개발자가 웹서비스 형태의 앱을 등록한 경우

   .. figure:: static/new_app_15_show_user_service_details.png

      MedbizWatchPM25 앱의 '자세히보기' 화면


4. MedbizWatchPM25 앱은 실행 파일 형태의 앱이다. 그러므로, '애플리케이션 파일' 항목의 링크를 다운로드한다.
   다운로드 하는 방법은 '애플리케이션 파일' 항목에 명시된 URL 을 복사(Copy)하여, 웹브라우저에 붙여넣기(Paste)한 후, 키보드에서 엔터키를 누른다.

   .. figure:: static/new_app_17_download_app.png

      MedbizWatchPM25 앱을 다운로드한 화면
