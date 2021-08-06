=============
개발 자료
=============

.. contents:: 목차

----
개요
----

MEDBIZ 플랫폼 연동을 위해 Android 기반 SDK를 활용하여 서비스를 개발할 수 있도록 자료를 제공합니다.

.. note::

    MEDBIZ 플랫폼에 Android SDK 활용을 위해서는 `디바이스 등록 <https://medbiz-user-guide.readthedocs.io/>`_
    과정과 `OAuth Client 등록 <https://medbiz-user-guide.readthedocs.io/>`_ 을 완료한 후 진행할 수 있습니다.


-------------
SDK 및 샘플앱
-------------

=========  ============  ===================  ==================================
날짜        플랫폼         종류                  다운로드
=========  ============  ===================  ==================================
20201112   안드로이드      SDK, 예제용 앱        `ANDROID_MOBILE_SDK_20201112 <static/ANDROID_MOBILE_SDK_20201112.zip>`_
---------  ------------  -------------------  ----------------------------------
20201113   안드로이드      SDK, 샘플용 앱        `MEDBIZ_Sample_App <static/Medbiz.zip>`_
=========  ============  ===================  ==================================


------------------
앱개발 환경 구성
------------------

본 플랫폼에 연동하기 위한 안드로이드 디바이스 개발 사양

==================  ============
구분                 내용
==================  ============
Gradle              4.0.1
------------------  ------------
Android API Level   23 - 30
------------------  ------------
지원 언어             Java
==================  ============

------------------
지원 기능 소개
------------------

본 플랫폼에 연동하기위해 제공되는 SDK 및 샘플은

1. 플랫폼 인증관리(OAuth 2.0) - Code or Password
2. 플랫폼 디바이스 관리 API - 등록, 조회 제거
3. VFS 연동 API - ls, cd, mkdir, put, get

의 기능을 제공하고 별도로 디바이스 연계기능이 포함된 샘플앱을 제공합니다.

.. panels::
    :body: text-center
    :header: text-center

    :column: + p-1
    SDK Sample App 화면
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    .. figure:: static/SDKSample.png

    ---
    :column: + p-1
    MEDBIZ Sample App 화면
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    .. figure:: static/MedbizSample.png

------------------
OAuth 2.0
------------------

1. Login
===============

해당 기능은 Android에서 Login API를 이용한 플랫폼 인증 기능을 수행한다.
AuthenticationActivity -> OAuthClient 객체 생성 코드 참조
해당 객체를 생성할 때, 개발자 사이트에서 만들었던 OAuth 클라이언트 정보를 객체에 생성자로 기입한다. ( 인증 관리 페이지 참고 )

AuthenticationActivity -> loginDialogButton.setOnClickListener 소스코드 참조
다이얼로그 이용 없이 ID, Password 입력 변수로 Password 클래스의 requestAccessToken() 메소드를 를 호출하여 인증 토큰(AccessToken,
RefreshToken)을 획득할 수 있다.

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-2

    requestAccessToken()
    ^^^^^^^

    .. figure:: static/OAuth_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-2

    소스 코드 주석
    ^^^^^^^

    .. code::

        private OAuthClient oAuthClientForPassword = new OAuthClient(
            new OAuthClientInformation(
                "발급 받은 OAuth Client ID",
                "발급 받은 Oauth Client Secret",
                "http://localhost/auth",
                "profile device",
                "token",
                "password"
            )
        );

        - Request

            requestAccessToken(userName, password)

        - Response

        D/AuthenticationActivity: 발급받은 OAuthToken Result : 200,
        OAuthToken {
            accessToken='0c5d0ada-4990-48b4-98f3-4f0067321eb1',
            tokenType='bearer',
            refreshToken='9e43275e-ad9a-42b1-92b2-392acc5b317a',
            expiresIn=3599,
            scope='device profile'
        }

2. OAuth Token Refresh
=========================

해당 기능은 OAuth AccessToken 만료 시에 RefreshToken을 통해 AccessToken을 재발급하는 기능을 구현한다.
기존 인증 후, 리프레시 토큰으로 AccessToken 재발급
MainActivity -> tokenRefreshButton.setOnClickListener -> Code -> requestAccessTokenWithRefreshToken 순서로 소스코드 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-2

    requestAccessTokenWithRefreshToken()
    ^^^^^^^

    .. figure:: static/AccessTokenRefresh.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-2

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            requestAccessTokenWithRefreshToken(토큰정보)

        - Response

        D/AuthenticationActivity: 발급받은 OAuthToken Result : 200,
        OAuthToken {
            accessToken='0c5d0ada-4990-48b4-98f3-4f0067321eb1',
            tokenType='bearer',
            refreshToken='9e43275e-ad9a-42b1-92b2-392acc5b317a',
            expiresIn=3599,
            scope='device profile'
        }

3. User Infomation
=========================

해당 기능은 로그인 된 유저의 정보를 얻어오는 기능을 수행
인증 완료 후 AccessToken 으로 요청

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-2

    userMe()
    ^^^^^^^

    .. figure:: static/GetUserInfomation.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-2

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            getUserMe(accessToken)

        - Response

        D/AuthenticationActivity: userMe() Result :200
        UserMe {
            userMuid='9109204ebd381824578b652150256d6a',
            userId='admin',
            email='admin@openlab.com',
            createAt='2020-10-06T06:08:24.000+00:00',
            userName='관리자',
            authorities=ROLE_ADMIN,ROLE_USER
        }

------------------
디바이스 관리
------------------

1. 장비 시리얼번호로 장비 MUID 찾기
===================================================

해당 기능은 Medbiz 플랫폼에 등록된 장비의 시리얼번호와 장비 모델 MUID를 통해 플랫폼에서 사용하는 장비 MUID를 조회하는 API

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-2

    findMuidByDevSerial()
    ^^^^^^^

    .. figure:: static/Device_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-2

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            findMuidByDevSerial(accessToken, DeviceModelMuid, serialNum password)

        - Response

        D/DeviceManageActivity: setFindMuidByDevSerialCallback() Result : 200
        931c50f7f25b4754d2d84f1192738985

1. 장비 시리얼번호로 장비 MUID 찾기
===================================================

해당 기능은 Medbiz 플랫폼에 등록된 장비의 시리얼번호와 장비 모델 MUID를 통해 플랫폼에서 사용하는 장비 MUID를 조회하는 API

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-2

    findMuidByDevSerial()
    ^^^^^^^

    .. figure:: static/Device_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-2

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            findMuidByDevSerial(accessToken, DeviceModelMuid, serialNum password)

        - Response

        D/DeviceManageActivity: setFindMuidByDevSerialCallback() Result : 200
        931c50f7f25b4754d2d84f1192738985
