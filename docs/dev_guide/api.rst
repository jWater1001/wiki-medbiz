=============
오픈 API 사용
=============

.. contents:: 목차

.. note::

    모든 medbiz 플랫폼의 API 호출은 access token을 필요로 합니다.
    `여기 <https://medbiz-user-guide.readthedocs.io/ko/latest/login.html>`_ 을 확인하여 token을 얻고 사용하는 방법을 익힐 수 있습니다.

---------
개요
---------
MEDBIZ 플랫폼에서 제공하는 오픈 API를 활용해 다양한 서비스를 개발할 수 있습니다.

--------------
서비스 개발 예
--------------
#. `다이콤 뷰어 서비스 <https://service.medbiz.or.kr/dicom/>`_ 메드비즈 플랫폼에 저장된 dcm 파일들을 웹에서 확인할 수 있습니다.
#. `웹 파일 서비스 <https://vfs.medbiz.or.kr/>`_ 메드비즈 플랫폼에 파일을 업로드하고 다운로드할 수 있습니다.
#. `당뇨 관리 서비스 <https://service.medbiz.or.kr/diabetes/>`_ 메드비즈 플랫폼을 통해 당뇨 관리 서비스를 제공할 수 있습니다.

---------------
사전 준비 사항
---------------
* MEDBIZ 플랫폼에서 제공되는 오픈 API들을 활용하기 위해서는 Oauth 2.0 기반 사용자 인증이 필요합니다. 

* Oauth 2.0 기반 인증을 진행하기 위해 개발자 페이지에서 OauthClient 등록하고 클라이언트 아이디와 클라이언트 시크릿을 발급받아야 합니다. 

* 클라이언트 아이디와 클라이언트 시크릿은 인증된 클라이언트(서비스)인지를 확인하는 수단이며, OauthClient가 등록되면 발급됩니다. 

* 등록된 클라이언트 아이디와 클라이언트 시크릿을 통해 MEDBIZ 로그인을 진행한 후 발급된 사용자의 AccessToken을 오픈API를 호출할 때 HTTP 헤더에 포함해서 전송해야 API를 호출할 수 있습니다.

.. warning::

    MEDBIZ 플랫폼에 로그인한 사용자 계정으로 OauthClient가 등록됩니다. OauthClient를 등록한 MEDBIZ 아이디는 '관리자' 권한을 가지게 되므로 해당 MEDBIZ 계정의 보안에 각별히 주의해야 합니다.

OauthClient 등록
==================

MEDBIZ OauthClient 등록하기 위한 기본 절차는 아래와 같습니다.

#. `MEDBIZ 개발자 사이트 <https://dev.medbiz.or.kr/>`_ 로 접속하여 OauthClient를 등록할 계정으로 로그인합니다.

#. 좌측에 '클라이언트 등록'을 선택하여 클라이언트 등록 페이지로 이동합니다.

  .. image:: static/api/oauth_client01.png

#. '클라이언트 등록' 페이지에서 필요한 정보들을 입력합니다. 각 속성의 의미는 아래와 같습니다.

.. image:: static/api/oauth_client02.png

OauthClient 등록 세부 정보
-----------------------------

**어플리케이션 이름**

등록할 OauthClient의 이름을 입력합니다. OauthClient 이름은 최대 64자까지 입력할 수 있습니다.
사용자가 해당 OauthClient를 통해 MEDBIZ 로그인을 진행할 때 다음 화면처럼 MEDBIZ 아이디로 로그인 화면에 OauthClient 이름이 표시되므로 적절한 이름을 사용하는 것을 권장합니다.

.. image:: static/api/oauth_client03.png

**API SCOPE**

개발하고자하는 서비스에서 접근하고자 하는 사용자의 데이터 범위(사용하고자하는 MEDBIZ 오픈API)를 선택하는 항목입니다. 사용자가 이용하려는
서비스에서 접근할 필요가 없음에도 너무 많은 API 범위를 선택하면 사용자가 서비스를 거부할 수 있습니다.

   ===============  ==================================================
   SCOPE            내용
   ===============  ==================================================
   DRIVE            Medbiz 드라이브에서 파일 읽기/쓰기
   DRIVE_READONLY   Medibz 드라이브 파일 읽기
   PROFILE          아이디 이메일주소 이름 성별 생일 보기
   DEVICE           사용중인 디바이스 정보 보기
   APPLICATION      사용중인 어플리케이션 보기
   GROUP            속해져있는 그룹 보기
   GROUP_PROFILE    사용자 기본 그룹 정보 보기
   HEALTH           개인 의료 데이터 읽기
   ===============  ==================================================

**AUTH METHODS**

인증에 사용할 OAUTH2 접근방식을 선택합니다. 해당 속성값은 3개 모두 선택하시면 됩니다.

**WEB SERVER REDIRECT URL**

사용자가 인증 종료 후 돌아갈 수 있도록 제공되어야 하는 콜백 URL입니다. 인증서버는 사용자가 인증을 정상적으로 모두 수행하게 되면 해당 URL로
리다이렉트 시키게 되어있습니다. 서비스에서는 해당 URL을 외부로 OPEN하여 콜백을 받을 수 있도록 설정하여야하며 해당 콜백 URL에서 서비스 자체
회원가입을 시키거나 access token을 확인하는 등으로 개발을 진행할 수 있습니다.

OauthClient 아이디와 클라이언트 시크릿 확인
=============================================

Client ID와 Secret은 서비스를 개발할 때 MEDBIZ 로그인을 사용하기 위해 사용됩니다. 

Client ID와 Secret은 다음 메뉴에서 확인할 수 있습니다.

1. "OAuth Clients > 내 클라이언트" 메뉴로 이동 후 생성한 클라이언트 클릭

2. 우측 상단에 'SHOW CLIENT ID & SECRET' 메뉴를 클릭하면, Client ID와 Secret을 확인할 수 있는 창이 표시됩니다.

      .. image:: static/api/oauth_client04.png

---------
오픈 API
---------
MEDBIZ 플랫폼에서 제공되는 오픈 API들을 활용하기 위해서는 Oauth 2.0 기반 사용자 인증이 필요합니다. Oauth 2.0 기반 인증을 진행하기 위해
먼저 개발자 페이지에서 OauthClient를 등록한 후 사용자가 로그인한 후 오픈 API에 사용자의 Oauth Access Token을 헤더에 포함하여 전송해야합니다.

디바이스 오픈 API
=================

디바이스 오픈API를 이용해서 MEDBIZ 플랫폼에 등록된 디바이스 정보에 접근할 수 있습니다. 디바이스 오픈 API를 활용하여 서비스 개발에 활용할 수 있습니다.

유저 Device 등록
------------------

* 사용자에게 디바이스를 등록하는 오픈API입니다. MEDBIZ 로그인으로 로그인한 사용자의 Oauth Token을 활용해 사용자에게 새로운 디바이스를 등록할 수 있습니다.

**요청 예**

    ======  ===========================================
    방식     POST
    url     :code:`https://openapi.medbiz.or.kr/v1/devices`
    header  Authorization : Bearer Token
    body    .. code:: json

              {
              "deviceNickName":"[디바이스 별명]"
              }
    ======  ===========================================

**응답 예**

  .. code-block:: json

    {
      "size": 10,
      "page": 0,
      "total": 1,
      "first": true,
      "last": true,
      "items": [
          {
              "deviceMuid": "[디바이스 MUID]",
              "deviceToken": "[디바이스 SECRET]",
              "userRegistered": "[디바이스 사용자 등록여부]",
              "enabled": "[디바이스 활성화 여부]",
              "deviceModel": {
                  "modelMuid": "[제품 MUID]",
                  "modelSerialNumber": "[제품 시리얼번호]",
                  "modelImageUri": "[제품 사진 URI]",
                  "modelDuplicationRegistration": "[제품 등록가능 여부]",
                  "modelName": "[제품 이름]",
                  "modelDesc": "[제품 설명]",
                  "modelDeveloperName": "[제품 생성자]",
                  "modelInfoImageUri": "[제품 정보 이미지]",
                  "modelBuyLink": "[제품 구매가능 링크]",
                  "modelSize": "[제품 크기]",
                  "modelWeight": "[제품 무게]",
                  "status": "[제품 상태]",
                  "modelCreateDate": "[제품 생성 시간]",
                  "modelModifyDate": "[제품 수정 시간"]
              },
              "deviceSerialNumber": "[디바이스 시리얼 번호]",
              "deviceNickname": "[디바이스 닉네임]",
              "version": "[디바이스 매니페스트 버전]",
              "deviceCreateDate": "[디바이스 생성 시간]",
              "deviceModifyDate": "[디바이스 수정 시간"]
          }
      ]
    }

유저 Device 전체 조회
----------------------

* 사용자에게 등록된 전체 디바이스 목록을 조회하는 오픈API입니다. MEDBIZ 로그인으로 로그인한 사용자의 Oauth Token을 활용해 사용자가 사용중인 디바이스 목록에 접근할 수 있습니다.

**요청 예**

     ======  ===========================================
     방식     GET
     url     :code:`https://openapi.medbiz.or.kr/v1/devices`
     header  Authorization : Bearer Token
     ======  ===========================================

**응답 예**

  .. code:: json

    {
        "size": 10,
        "page": 0,
        "total": 1,
        "first": true,
        "last": true,
        "items": [
            {
                "deviceMuid": "[디바이스 MUID]",
                "deviceToken": "[디바이스 SECRET]",
                "userRegistered": "[디바이스 사용자 등록여부]",
                "enabled": "[디바이스 활성화 여부]",
                "deviceModel": {
                    "modelMuid": "[제품 MUID]",
                    "modelSerialNumber": "[제품 시리얼번호]",
                    "modelImageUri": "[제품 사진 URI]",
                    "modelDuplicationRegistration": "[제품 등록가능 여부]",
                    "modelName": "[제품 이름]",
                    "modelDesc": "[제품 설명]",
                    "modelDeveloperName": "[제품 생성자]",
                    "modelInfoImageUri": "[제품 정보 이미지]",
                    "modelBuyLink": "[제품 구매가능 링크]",
                    "modelSize": "[제품 크기]",
                    "modelWeight": "[제품 무게]",
                    "status": "[제품 상태]",
                    "modelCreateDate": "[제품 생성 시간]",
                    "modelModifyDate": "[제품 수정 시간]"
                },
                "deviceSerialNumber": "[디바이스 시리얼 번호]",
                "deviceNickname": "[디바이스 닉네임]",
                "version": "[디바이스 매니페스트 버전]",
                "deviceCreateDate": "[디바이스 생성 시간]",
                "deviceModifyDate": "[디바이스 수정 시간]"
            }
        ]
    }

유저 Device 수정
------------------

* 사용자에게 디바이스를 수정하는 오픈API입니다. MEDBIZ 로그인으로 로그인한 사용자의 Oauth Token을 활용해 사용자에게 등록되어있던 디바이스의 정보를 수정할 수 있습니다.

**요청 예**

    ======  ===============================================================
    방식     PUT
    url     :code:`https://openapi.medbiz.or.kr/v1/devices/{DEVICE_MUID}`
    header  Authorization : Bearer Token
    body    .. code:: json

              {
                "deviceNickName":"[디바이스 별명]"
              }
    ======  ===============================================================

**응답 예**

  .. code:: json

    {
      "size": 10,
      "page": 0,
      "total": 1,
      "first": true,
      "last": true,
      "items": [
          {
              "deviceMuid": "[디바이스 MUID]",
              "deviceToken": "[디바이스 SECRET]",
              "userRegistered": "[디바이스 사용자 등록여부]",
              "enabled": "[디바이스 활성화 여부]",
              "deviceModel": {
                  "modelMuid": "[제품 MUID]",
                  "modelSerialNumber": "[제품 시리얼번호]",
                  "modelImageUri": "[제품 사진 URI]",
                  "modelDuplicationRegistration": "[제품 등록가능 여부]",
                  "modelName": "[제품 이름]",
                  "modelDesc": "[제품 설명]",
                  "modelDeveloperName": "[제품 생성자]",
                  "modelInfoImageUri": "[제품 정보 이미지]",
                  "modelBuyLink": "[제품 구매가능 링크]",
                  "modelSize": "[제품 크기]",
                  "modelWeight": "[제품 무게]",
                  "status": "[제품 상태]",
                  "modelCreateDate": "[제품 생성 시간]",
                  "modelModifyDate": "[제품 수정 시간]"
              },
              "deviceSerialNumber": "[디바이스 시리얼 번호]",
              "deviceNickname": "[디바이스 닉네임]",
              "version": "[디바이스 매니페스트 버전]",
              "deviceCreateDate": "[디바이스 생성 시간]",
              "deviceModifyDate": "[디바이스 수정 시간]"
          }
      ]
    }

유저 Device 등록 해제
-----------------------

* 사용자에게 디바이스를 등록해제(삭제)하는 오픈API입니다. MEDBIZ 로그인으로 로그인한 사용자의 Oauth Token을 활용해 사용자에게 등록되어있던 디바이스를 등록해제(삭제)할 수 있습니다.

**요청 예**

    ======  ===========================================
    방식     DELETE
    url     :code:`https://openapi.medbiz.or.kr/v1/devices/{DEVICE_MUID}`
    header  Authorization : Bearer Token
    ======  ===========================================

**응답 예**

  .. code:: json

    {
      "size": 10,
      "page": 0,
      "total": 1,
      "first": true,
      "last": true,
      "items": [
          {
              "deviceMuid": "[디바이스 MUID]",
              "deviceToken": "[디바이스 SECRET]",
              "userRegistered": "[디바이스 사용자 등록여부]",
              "enabled": "[디바이스 활성화 여부]",
              "deviceModel": {
                  "modelMuid": "[제품 MUID]",
                  "modelSerialNumber": "[제품 시리얼번호]",
                  "modelImageUri": "[제품 사진 URI]",
                  "modelDuplicationRegistration": "[제품 등록가능 여부]",
                  "modelName": "[제품 이름]",
                  "modelDesc": "[제품 설명]",
                  "modelDeveloperName": "[제품 생성자]",
                  "modelInfoImageUri": "[제품 정보 이미지]",
                  "modelBuyLink": "[제품 구매가능 링크]",
                  "modelSize": "[제품 크기]",
                  "modelWeight": "[제품 무게]",
                  "status": "[제품 상태]",
                  "modelCreateDate": "[제품 생성 시간]",
                  "modelModifyDate": "[제품 수정 시간]"
              },
              "deviceSerialNumber": "[디바이스 시리얼 번호]",
              "deviceNickname": "[디바이스 닉네임]",
              "version": "[디바이스 매니페스트 버전]",
              "deviceCreateDate": "[디바이스 생성 시간]",
              "deviceModifyDate": "[디바이스 수정 시간]"
          }
      ]
    }

파일 오픈 API
===============

Medbiz 플랫폼은 클라우드 저장소에 HTTP를 통해 파일을 업로드/다운로드할 수 있는 가상파일시스템(VFS; Virtual File System) API를 제공합니다.

아래 설명될 클라우드 저장소 API들은 MEDBIZ 로그인을 통해 사용자의 Access Token이 반드시 필요합니다.

.. warning::

    파일 오픈API를 사용하기 위해서는 MEDBIZ에서 클라우드 저장소 사용 동의가 필요합니다.

    **MEDBIZ 홈페이지를 통한 클라우드 저장소 사용 동의 절차**

    1. MEDBIZ VFS 서비스(https://vfs.medbiz.or.kr)에 로그인합니다.

    2. 사용자가 아직 파일시스템 사용동의를 하기 전이라면 저장소 사용 동의 화면이 표시됩니다.

    3. '동의하기' 버튼을 클릭하면 웹 기반 저장소 화면이 표시됩니다.

    저장소 사용 동의 화면은 한번만 동의하면, 이후에는 나오지 않는다.

VFS API 흐름
------------------------------

.. image:: static/api/api_vfs01.png


공통 사용 헤더 키
------------------------------

.. table:: MEDBIZ PLATFROM VFS OpenAPI HTTP Header Description
   :widths: auto

   =============================== ============== =========================================================================================================
   HTTP Header Key                 Value Type     Description
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Id               Long           가상파일 시스템에서 기준이 되는 ID, 현재 경로 정보
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Path             String         가상파일 시스템에서 상대 또는 절대경로를 지정한다. 상대경로로 사용할 경우 Medbiz-Catalog-Id를 기준으로 상대경로를 탐색한다.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Permission       String         가상파일 시스템에 대한 권한을 지정. 리눅스 파일 시스템의 permission을 모델링하여 적용하였으며 000 ~ 777 까지의 범위를 갖는다.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Extension        String         가상파일 시스템 확장자 정보를 요청하는 변수로, 검색시에 사용됨.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Src              String         가상파일 시스템의 특정 리소스를 지칭하며 변경, 복제 등에 대한 대상이되는 경로 정보.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Dst              String         가상파일 시스템의 변경, 복제에 대한 결과물의 대상이 되는 경로 정보.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Recursive        Boolean        가상파일 시스템의 하위 경로에 대한 동일 액션을 취할지에 대한 정보.
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Encryption       Boolean        가상파일 시스템에 저장하는 데이터에 대한 암호화 여부
   ------------------------------- -------------- ---------------------------------------------------------------------------------------------------------
   Medbiz-Catalog-Contents-Length  Long           업로드하고자하는 컨텐츠의 바이트 길이 정보
   =============================== ============== =========================================================================================================


사용자 HOME 경로 정보 확인 API
-------------------------------

* 로그인 절차를 통해 얻은 토큰을 Header에 추가하고 uri로 정보를 요청함

  .. table::
   :widths: auto

   ======  ===========================================================
   방식    GET
   ------  -----------------------------------------------------------
   uri     :code:`https://auth.medbiz.or.kr/user/me`
   ------  -----------------------------------------------------------
   header  Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe
   ------  -----------------------------------------------------------
   body    
   ======  ===========================================================

* HTTP형식으로 표현

  .. code:: http

    GET /auth/user/me HTTP/1.1
    Host: auth.medbiz.or.kr
    Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe

* JAVA, OKHttp lib로 표현

  .. code:: java

    OkHttpClient client = new OkHttpClient();
    Request request = new Request.Builder()
      .url("https://auth.medbiz.or.kr/user/me")
      .get()
      .addHeader("Authorization", "Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe")
      .build();
    Response response = client.newCall(request).execute();

* 사용자 HOME 경로 정보 확인 API에 대한 응답으로 vfsHomeDir 값이 해당 사용자의 홈 경로이다. vfsHomeDir값이 null일 경우 클라우드 저장소를 동의하지 않았다고볼 수 있음

  .. code:: json

    {
      "userMuid": "2c90eee6651291a601651363616b0003",
      "userid": "jongsu",
      "email": "jongsu@gwnu.ac.kr",
      "enabled": true,
      "createAt": 1533628605000,
      "updatedAt": 1533628605000,
      "vfsHomeDir": "/home/jongsu",
      "vfsStorageQuotaMb": 10240,
      "userName": "jongsu",
      "address": "원주",
      "phone": "010-0000-0000",
      "dKey": null
    }


경로변경 API
------------------------------

* 사용자 HOME 경로 정보 Medbiz-Catalog-Id 요청 API와 경로변경 API는 동일한 API이다.
* HTTP Header로 인증정보와, Medbiz-Catalog-Id, Medbiz-Catalog-Path 값을 필요로하며 경로에 따라 응답받은 Medbiz-Catalog-Id 값은 변경된다.
* HOME 경로를 조회할경우 Medbiz-Catalog-Id 초기값은 1로 설정하며 아래 요청을 통해 해당 경로에 대한 Medbiz-Catalog-Id 값을 받을수 있으며 이는 클라우드 저장소 상에서 사용되는 기준 경로 정보로 사용된다.

  .. table::
   :widths: auto

   =======  =============================================================
   방식       POST
   -------  -------------------------------------------------------------
   uri       :code:`{openapi domain}/v1/catalog/cd`
   -------  -------------------------------------------------------------
   header    .. code::

                Content-Type: application/json
                Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe
                Medbiz-Catalog-Id: 1
                Medbiz-Catalog-Path: /home/userhome
   =======  =============================================================

* HTTP형식으로 표현

  .. code:: http

	POST /v1/catalog/cd HTTP/1.1
	Host: openapi.medbiz.or.kr
	Medbiz-Catalog-Id: 1
	Medbiz-Catalog-Path: /home/jongsu
	Authorization: Bearer ecec47cb-8766-491e-afb2-7a2793f8f066

	
* JAVA, OKHttp lib로 표현

  .. code:: java

	OkHttpClient client = new OkHttpClient();

	Request request = new Request.Builder()
	  .url("https://openapi.medbiz.or.kr/v1/catalog/cd")
	  .post(null)
	  .addHeader("Medbiz-Catalog-Id", "1")
	  .addHeader("Medbiz-Catalog-Path", "/home/jongsu")
	  .addHeader("Authorization", "Bearer ecec47cb-8766-491e-afb2-7a2793f8f066")
	  .build();

	Response response = client.newCall(request).execute();


* 경로 변경 요청에 따른 응답

  .. code:: json

	{
		"changedCatalog": {
			"catalogCreateDatetime": 1547725912821,
			"catalogUpdateDatetime": 1547730705631,
			"id": 7,
			"pid": 2,
			"owner": "jongsu",
			"group": "jongsu",
			"catalogMuid": null,
			"name": "jongsu",
			"permission": "740",
			"dir": 1,
			"size": null,
			"secret": false,
			"ownerMuid:group": null,
			"available": true
		}
	}


카탈로그 기준 하위 경로 조회 API
----------------------------------

* 현재 위치하는 경로의 하위 디렉토리/파일 리스트를 조회하는 API로 현재 위치정보는 HTTP Header에 입력되는 Medbiz-Catalog-Id 값이 기준으로 사용된다.
* header 의 Medbiz-Catalog-Id 값을 변경하거나 Medbiz-Catalog-Path 값을 이용하여 기준경로를 변경할 수 있다.
* Medbiz-Catalog-Path 값은 필수요소가 아니다.

  .. table::
   :widths: auto

   ======  =============================================================
   방식    POST
   ------  -------------------------------------------------------------
   uri     :code:`{openapi domain}/v1/catalog/ls`
   ------  -------------------------------------------------------------
   header    .. code::

              Content-Type: application/json
              Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe
              Medbiz-Catalog-Id: 1
              Medbiz-Catalog-Path: /home/userhome
   ======  =============================================================

* HTTP형식으로 표현

  .. code:: http

	POST /v1/catalog/ls HTTP/1.1
	Host: openapi.medbiz.or.kr
	Medbiz-Catalog-Id: 1
	Medbiz-Catalog-Path: /home/jongsu
	Authorization: Bearer ecec47cb-8766-491e-afb2-7a2793f8f066


* JAVA, OKHttp lib로 표현

	OkHttpClient client = new OkHttpClient();

	Request request = new Request.Builder()
	  .url("https://openapi.medbiz.or.kr/v1/catalog/ls")
	  .post(null)
	  .addHeader("Medbiz-Catalog-Id", "1")
	  .addHeader("Medbiz-Catalog-Path", "/home/jongsu")
	  .addHeader("Authorization", "Bearer ecec47cb-8766-491e-afb2-7a2793f8f066")
	  .build();

	Response response = client.newCall(request).execute();

* 하위 경로 조회에 따른 응답

  .. code:: json

	{
		"result": [
			{
				"catalogCreateDatetime": 1547725912821,
				"catalogUpdateDatetime": 1547730705631,
				"id": 7,
				"pid": 2,
				"owner": "jongsu",
				"group": "jongsu",
				"catalogMuid": null,
				"name": "jongsu",
				"permission": "740",
				"dir": 1,
				"size": null,
				"secret": false,
				"ownerMuid:group": null,
				"available": true
			},
			{
				"catalogCreateDatetime": 1550637917093,
				"catalogUpdateDatetime": 1551070539625,
				"id": 149,
				"pid": 7,
				"owner": "jongsu",
				"group": "jongsu",
				"catalogMuid": "94b162fc9b8af44232926cda16c2fb94",
				"name": "1.csv",
				"permission": "740",
				"dir": 0,
				"size": 34,
				"secret": false,
				"ownerMuid:group": null,
				"available": true
			}
		]
	}


경로 생성 API
------------------------------

* HTTP Header로 인증정보와, Medbiz-Catalog-Id, Medbiz-Catalog-Path, Medbiz-Catalog-Permission 값을 필요로하며 경로에 따라 Medbiz-Catalog-Id 값은 변경된다.
* Medbiz-Catalog-Permission 값은 필수가 아니다.

  .. table::
   :widths: auto

   ======  =============================================================
   방식    POST
   ------  -------------------------------------------------------------
   uri     :code:`{openapi domain}/v1/catalog/mkdir`
   ------  -------------------------------------------------------------
   header    .. code:: 

              Content-Type: application/json
              Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe
              Medbiz-Catalog-Id: 1
              Medbiz-Catalog-Path: /home/jongsu/apittest
              Medbiz-Catalog-Permission: 740
   ======  =============================================================

* HTTP형식으로 표현

  .. code:: http

	POST /v1/catalog/mkdir HTTP/1.1
	Host: openapi.medbiz.or.kr
	Medbiz-Catalog-Id: 1
	Medbiz-Catalog-Path: /home/jongsu/apittest
	Authorization: Bearer ecec47cb-8766-491e-afb2-7a2793f8f066


* JAVA, OKHttp lib로 표현

  .. code:: java

	OkHttpClient client = new OkHttpClient();

	Request request = new Request.Builder()
	  .url("https://openapi.medbiz.or.kr/v1/catalog/mkdir")
	  .post(null)
	  .addHeader("Medbiz-Catalog-Id", "1")
	  .addHeader("Medbiz-Catalog-Path", "/home/jongsu/apittest")
	  .addHeader("Authorization", "Bearer ecec47cb-8766-491e-afb2-7a2793f8f066")
	  .build();

	Response response = client.newCall(request).execute();


파일 업로드 API
------------------------------

* HTTP Header로 인증정보와, Medbiz-Catalog-Id, Medbiz-Catalog-Contents-Length, Medbiz-Catalog-Encryption 값을 필요로하며 경로에 따라 Medbiz-Catalog-Id 값은 변경된다.
* Medbiz-Catalog-Encryption은 선택사항이며 Default false 이다. Medbiz-Catalog-Id 와 Medbiz-Catalog-Contents-Length는 필수 값이다.
* multipart/form-data 형식으로 데이터를 전송하며, form-data key 값은 multifile로 사용한다.

  .. table::
   :widths: auto

   ======  =====================================================================================================================================
   방식    POST
   ------  -------------------------------------------------------------------------------------------------------------------------------------
   uri     :code:`{openapi domain}/v1/catalog/put`
   ------  -------------------------------------------------------------------------------------------------------------------------------------
   header    .. code::

              Authorization: Bearer 126677bc-4da9-4cbb-9895-903fc4c20abe 
              Medbiz-Catalog-Id: 7
              Medbiz-Catalog-Contents-Length: 26528
              Medbiz-Catalog-Encryption: false
              Content-Type: multipart/form-data;
              Content-Disposition: Content-Disposition: form-data; name="multifiles"; filename="C:\Users\jongsu\Desktop\동그라미.png
   ======  =====================================================================================================================================

* HTTP형식으로 표현

  .. code:: http

	POST /v1/catalog/put HTTP/1.1
	Host: openapi.medbiz.or.kr
	Medbiz-Catalog-Id: 7
	Medbiz-Catalog-Encryption: false
	Medbiz-Catalog-Contents-Length: 26528
	Authorization: Bearer ecec47cb-8766-491e-afb2-7a2793f8f066
	Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

	Content-Disposition: form-data; name="multifile"; filename="C:\Users\jongsu\Desktop\동그라미.png


	------WebKitFormBoundary7MA4YWxkTrZu0gW--

* JAVA, OKHttp lib로 표현

  .. code:: java

    OkHttpClient client = new OkHttpClient();
    RequestBody requestBody = new MultipartBody.Builder()
        .setType(MultipartBody.FORM)
        .addFormDataPart("multifile", src.getName(), RequestBody.create(MultipartBody.FORM, src))
        .build();
    Request request = new Request.Builder()
      .url(domain+"/v1/catalog/put")
      .post(requestBody)
      .addHeader("content-type", "multipart/form-data;")openapi/
      .addHeader("Medbiz-Catalog-Id", Long.toString(Medbiz-Catalog-Id))
      .addHeader("Medbiz-Catalog-Contents-Length", Long.toString(src.length()))
      .addHeader("Medbiz-Catalog-Encryption", Boolean.toString(false))
      .addHeader("Authorization", authorization)
      .build();
    Response response = client.newCall(request).execute();


파일 다운로드 API
------------------------------

* HTTP Header로 인증정보와, Medbiz-Catalog-Id 값을 필요로하며 경로에 따라 Medbiz-Catalog-Id 값은 변경된다.
* Medbiz-Catalog-Id 값을 기준으로 fileName을 탐색한다.

  .. table::
   :widths: auto

   ======  =====================================================================================================================================
   방식    GET
   ------  -------------------------------------------------------------------------------------------------------------------------------------
   uri     :code:`{openapi domain}/v1/catalog/get/file/{fileName}`
   ------  -------------------------------------------------------------------------------------------------------------------------------------
   header    .. code:: 

              Medbiz-Catalog-Id: 16395
              Authorization: Bearer 6dd2e4b7-2758-4fe0-9e19-d9ccb6ad9758
   ======  =====================================================================================================================================

* HTTP형식으로 표현

  .. code:: http

	GET /v1/catalog/get/file/%EB%8F%99%EA%B7%B8%EB%9D%BC%EB%AF%B8.png HTTP/1.1
	Host: openapi.medbiz.or.kr
	Medbiz-Catalog-Id: 7
	Authorization: Bearer 32782624-84d1-4d89-ac14-9acf143b9051

* JAVA, OKHttp lib로 표현

  .. code:: java

	OkHttpClient client = new OkHttpClient();

	Request request = new Request.Builder()
	  .url("https://openapi.medbiz.or.kr/v1/catalog/get/file/%EB%8F%99%EA%B7%B8%EB%9D%BC%EB%AF%B8.png")
	  .get()
	  .addHeader("Medbiz-Catalog-Id", "7")
	  .addHeader("Authorization", "Bearer 32782624-84d1-4d89-ac14-9acf143b9051")
	  .build();

	Response response = client.newCall(request).execute();

    File temp = File.createTempFile(fileName, ".tmp"); //파일 받을 경로
    FileOutputStream fos = new FileOutputStream(temp);

    IOUtils.copy(response.body().byteStream(), fos);

    System.out.println("다운받은 파일 절대 경로 : "+temp.getAbsolutePath());


헬스데이터 오픈 API
====================

헬스데이터 API 흐름
------------------------------

.. warning::

  헬스 데이터 API의 사용을 위해서는 디바이스특성(manifest) 속성에서 기기에 해당하는 속성값을 입력한 경우에만 데이터의 변환이 이루어집니다.
  디바이스 속성 등록에서 아래 속성표에 해당하는 파라미터 이름과 파라미터 단위로 선택을 해야만 헬스데이터에서 조회가 가능합니다.

.. warning::
  헬스 데이터 API는 oAuth scope중 HEALTH를 포함한 클라이언트에서만 접근이 가능합니다.
  :doc:`oauth client 가이드 <../dev_and_service/oauth_client>` 를 참조하여 API를 호출하는 oAuth 클라이언트가 해당 SCOPE를 포함하는지 확인하세요.

.. figure:: static/api/health_data_flow.png

헬스데이터 속성 표
------------------------------

.. warning::

  헬스 데이터 API의 사용을 위해서는 제품 특성(manifest)등록에서 기기에 해당하는 속성값을 입력한 경우에만 데이터의 변환이 이루어집니다.
  제품 특성 등록과 관련된 내용은 제품(Product의 3절 Manifest등록에서 수행합니다)

+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 데이터종류 |  RootCode                              |  파라미터 이름      | 파라미터 단위              | 변환파라미터 코드                  | 변환단위 코드        | 비고 |
+============+========================================+=====================+============================+====================================+======================+======+
| 활동량     | CARDIO                                 | cadence             | MDC_DIM_RPM                | MDC_HF_CAD                         | MDC_HF_CAD           |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | distance            | MDC_DIM_M                  | MDC_HF_DISTANCE                    | MDC_DIM_M            |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | speed               | MDC_DIM_M_PER_SEC          | MDC_HF_SPEED                       | MDC_DIM_M_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | energy_expended     | MDC_DIM_CAL                | MDC_HF_ENERGY                      | MDC_DIM_CAL          |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 혈압       | BLOOD_PRESSURE                         | systolic            | MDC_DIM_MMHG               | MDC_PRESS_BLD_NONINV_SYS           | MDC_DIM_MMHG         |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | diastolic           | MDC_DIM_MMHG               | MDC_PRESS_BLD_NONINV_DIA           | MDC_DIM_MMHG         |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | map                 | MDC_DIM_MMHG               | MDC_PRESS_BLD_NONINV_MEAN          | MDC_DIM_MMHG         |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | pulse               | MDC_DIM_BEAT_PER_MIN       | MDC_PLUS_RATE_NON_INV              | MDC_DIM_BEAT_PER_MIN |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 체온       | TEMP                                   | temperature         | MDC_DIM_DEGC               | MDC_TEMP_ORAL                      | MDC_DIM_DEGC         |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 혈당       | GLUCOSE                                | glucose             | MILLI_G_PER_DL             | MDC_CONC_GLU_CAPILLARY_WHOLEBLOOD  | MILLI_G_PER_DL       |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | glucose_ctxt_meal   | ENUM                       | MDC_CTXT_GLU_MEAL                  | ENUM (비고 참조)     |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 산소포화도 | PULS_OXIM                              | pulse_oxime_sat_o2  | MDC_DIM_PERCENT            | MDC_PULS_OXIM_SAT_O2               | MDC_DIM_PERCENT      |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | pulse_oxime_pleth   | MDC_DIM_DIMLESS            | MDC_PULS_OXIM_PLETH                | MDC_DIM_DIMLESS      |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 체중       | SCALE                                  | body_weight         | MDC_DIM_KILO_G             | MDC_MASS_BODY_ACTUAL               | MDC_DIM_KILO_G       |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | body_height         | MDC_DIM_CENTI_M            | MDC_LEN_BODY_ACTUAL                | MDC_DIM_CENTI_M      |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | body_mass_index     | MDC_RATIO_MASS_BODY_LEN_SQ | MDC_RATIO_MASS_BODY_LEN_SQ         | MDC_DIM_KG_PER_M_SQ  |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 신체계측   | SCALE_MEASUREMENT                      | waist_circumference | MDC_DIM_CENTI_M            | Circumf by WHI                     | MDC_DIM_CENTI_M      |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
| 폐활량     | SPIROMETER                             | fvc                 | MDC_DIM_X_L                | MDC_VOL_AWAY_EXP_FORCED_CAPACITY   | MDC_DIM_X_L          |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fev1                | MDC_DIM_X_L                | MDC_VOL_AWAY_EXP_FORCED_1S         | MDC_DIM_X_L          |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | pef                 | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_EXP_FORCED_PEAK      | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fef25               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_EXP_FORCED_25_FVC    | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fef50               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_EXP_FORCED_50_FVC    | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fef75               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_EXP_FORCED_75_FVC    | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fef2575             | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_EXP_FORCED_25_75_FVC | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fif25               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_INSP_FORCED_25       | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fif50               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_INSP_FORCED_50       | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+
|            |                                        | fif75               | MDC_DIM_L_PER_SEC          | MDC_FLOW_AWAY_INSP_FORCED_75       | MDC_DIM_L_PER_SEC    |      |
+------------+----------------------------------------+---------------------+----------------------------+------------------------------------+----------------------+------+

헬스데이터 전송
------------------------------

헬스데이터는 기기 속성에서 파라미터 이름과 파라미터 단위를 우선적으로 선택하여 제품을 등록한 후 디바이스가 데이터를 아래의 형태에 맞게 전송해야합니다.

아래의 예를 참조하여 기기가 혈압계인 경우 전송하는 방법을 안내합니다.

:doc:`제품 등록 및 Manifest 등록 <../dev_and_service/product_device>` 에서 제품 특성 선택시 데이터 속성표에 맞는 적절한 파라미터를 선택하세요

데이터 생성
````````````````````````````````

아래의 샘플 oneM2M API를 통해 혈압데이터인 경우 데이터를 생성하는 예를 볼 수 있습니다.

기본적으로는 oneM2M의 content instance 생성 과정과 동일하지만 m2m:cin.con key에 입력되는 value로 json 형태로 입력되어야 하며 해당 데이터들의 key값은 상기 속성표의 데이터 이름입니다.

따라서 혈압 데이터는 아래와 같은 형태로 데이터가 전송되어야 합니다.

**혈압 요청 예**

    ======  ===========================================
    방식     POST
    url     :code:`https://onem2m.medbiz.or.kr/Mobius/{deviceMUID}/fields/{fieldName}`
    header  X-M2M-RI : Bearer Token
            X-M2M-Origin : {requestID}
            Content-Type : application/json;ty=4
    body    .. code:: json

              {
                "m2m:cin": {
                  "con": {
                    "systolic": 123,
                    "diastolic" : 80,
                    "map" : 85,
                    "pulse" : 67
                  }
                }
              }
    ======  ===========================================

  
추가적으로 체온 데이터는 아래와 같은 형태로 데이터가 전송되어야 합니다.

**체온 요청 예**

======  ===========================================
방식     POST
url     :code:`https://onem2m.medbiz.or.kr/Mobius/{deviceMUID}/fields/{fieldName}`
header  X-M2M-RI : Bearer Token
        X-M2M-Origin : {requestID}
        Content-Type : application/json;ty=4
body    .. code:: json

          {
            "m2m:cin": {
              "con": {
                "temperature": 37.2
              }
            }
          }
======  ===========================================

헬스데이터 API
------------------------------

헬스데이터 API를 통해 개인의 헬스데이터를 조회할 수 있습니다.

조회는 헬스 데이터 속성 표에 따른 RootCode를 기반으로 조회할 수 있습니다.


헬스데이터 조회 API
````````````````````````````````

* 사용자가 자신의 헬스데이터를 조회할 수 있습니다

.. figure:: static/api/health_api_01.png


**요청 형식**

  ======  ===========================================
  방식     GET
  url     :code:`https://openapi.medbiz.or.kr/v1/health`
  header  Authorization : Bearer Token
  param   
          +--------------+----------+------------+-----------------------+
          | Param        | Optional | Default    | Value                 |
          +==============+==========+============+=======================+
          | obxRootRefId | X        | X          | 헬스데이터 RootCode   |
          +--------------+----------+------------+-----------------------+
          | dateStart    | O        | StartOfDay | yyyy-MM-dd'T'HH:mm:ss |
          +--------------+----------+------------+-----------------------+
          | dateEnd      | O        | EndOfDay   | yyyy-MM-dd'T'HH:mm:ss |
          +--------------+----------+------------+-----------------------+
          | offset       | O        | 0          | Integer               |
          +--------------+----------+------------+-----------------------+
          | limit        | O        | 10         | Integer               |
          +--------------+----------+------------+-----------------------+
  body    
  ======  ===========================================

헬스데이터 공유 안내
````````````````````````````````

* 다른 사용자가 사용자의 헬스데이터에 접근할 수 있도록 공유할 수 있습니다
* 공유단위는 RootCode를 최소단위로 다른사용자에게 접근을 허용할 수 있습니다
* 공유시에는 만료일을 지정하여 자동으로 해당일이 되면 접근이 불가하도록 설정할 수 있습니다

.. note:: text
  현재 헬스 데이터 공유 권한 부여는 플랫폼 정책상 openAPI로 제공되지 않습니다


헬스데이터 공유 조회 API
````````````````````````````````

* 다른 사용자의 헬스데이터를 조회할 수 있습니다
* 공유를 받은 유저의 데이터에만 접근할 수 있습니다

.. figure:: static/api/health_api_02.png


**요청 형식**

  ======  ===========================================
  방식     GET
  url     :code:`https://openapi.medbiz.or.kr/v1/shared-health`
  header  Authorization : Bearer Token
  param   
          +--------------+----------+------------+-------------------------+
          | Param        | Optional | Default    | Value                   |
          +==============+==========+============+=========================+
          | obxRootRefId | X        | X          | 헬스데이터 RootCode     |
          +--------------+----------+------------+-------------------------+
          | userid       | X        | X          | 조회하고자하는 사용자ID |
          +--------------+----------+------------+-------------------------+
          | dateStart    | O        | StartOfDay | yyyy-MM-dd'T'HH:mm:ss   |
          +--------------+----------+------------+-------------------------+
          | dateEnd      | O        | EndOfDay   | yyyy-MM-dd'T'HH:mm:ss   |
          +--------------+----------+------------+-------------------------+
          | offset       | O        | 0          | Integer                 |
          +--------------+----------+------------+-------------------------+
          | limit        | O        | 10         | Integer                 |
          +--------------+----------+------------+-------------------------+
  body    
  ======  ===========================================