=========
개발 자료
=========

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

   .. figure:: static/MedbizSample.png
   .. figure:: static/SDKSample.png

1. OAuth 2.0
===============

해당 기능은 Android에서 Login API를 이용한 플랫폼 인증 기능을 수행한다.
AuthenticationActivity -> OAuthClient 객체 생성 코드 참조
해당 객체를 생성할 때, 개발자 사이트에서 만들었던 OAuth 클라이언트 정보를 객체에 생성자로 기입한다. ( 인증 관리 페이지 참고 )

.. panels::
    :column: col-lg-4 p-2

    .. figure:: static/MedbizSample.png

    ---
    :column: col-lg-8 p-2
    :body: text-left

    panel 2 footerfadsfasdfasdfasdfsadf
