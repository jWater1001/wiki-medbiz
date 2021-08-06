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
20201112   안드로이드      SDK, 예제용 앱        `ANDROID_MOBILE_SDK_20201112 <static/sdk/ANDROID_MOBILE_SDK_20201112.zip>`_
---------  ------------  -------------------  ----------------------------------
20201113   안드로이드      SDK, 샘플용 앱        `MEDBIZ_Sample_App <static/sdk/Medbiz.zip>`_
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
    .. figure:: static/sdk/SDKSample.png

    ---
    :column: + p-1
    MEDBIZ Sample App 화면
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    .. figure:: static/sdk/MedbizSample.png

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

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    requestAccessToken()
    ^^^^^^^

    .. figure:: static/sdk/OAuth_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

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

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    requestAccessTokenWithRefreshToken()
    ^^^^^^^

    .. figure:: static/sdk/AccessTokenRefresh.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

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

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    userMe()
    ^^^^^^^

    .. figure:: static/sdk/GetUserInfomation.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

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

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    findMuidByDevSerial()
    ^^^^^^^

    .. figure:: static/sdk/Device_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

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

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    findMuidByDevSerial()
    ^^^^^^^

    .. figure:: static/sdk/Device_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            findMuidByDevSerial(accessToken, DeviceModelMuid, serialNum password)

        - Response

        D/DeviceManageActivity: setFindMuidByDevSerialCallback() Result : 200
        931c50f7f25b4754d2d84f1192738985

2. 사용자 계쩡에 장비 추가하기
===================================================

해당 기능은 Medbiz 플랫폼에 등록된 장비 MUID를 로그인 된(AccessToken) 계정으로 등록하는 API

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    addMyDevice()
    ^^^^^^^

    .. figure:: static/sdk/Device_2.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            addMyDevice(accessToken, 디바이스정보[장비 별명, 장비MUID])

        - Response

        D/DeviceManageActivity: setAddMyDeviceCallback() Result : 200
        Device {
            deviceMuid='931c50f7f25b4754d2d84f1192738985',
            deviceToken='e8b3dc7cf80a4ce9bd7c06b5a22127ea',
            developerUserMuid='9109204ebd381824578b652150256d6a',
            userRegistered=true,
            enabled=true,
            deviceModel= DeviceModel {
                modelMuid='930c1f66cc2746b49c9aff9e5f8da31f',
                modelSerialNumber='dfdsfsdf',
                developerUserMuid='9109204ebd381824578b652150256d6a',
                modelImageUri='null',
                modelDuplicationRegistration=false,
                modelName='sfsdfsdfs',
                modelDesc='sdfsdfasdfasfas',
                modelDeveloperName='sdfsdfsd',
                modelInfoImageUri='null',
                modelBuyLink='',
                modelSize='',
                modelWeight='',
                status='TEST',
                modelCreateDate=1603330342000,
                modelModifyDate=1603330342000
            },
            deviceSerialNumber='123456',
            deviceNickname='테스트장비',
            version=0,
            usersMuid=[9109204ebd381824578b652150256d6a],
            ownerUserMuid='9109204ebd381824578b652150256d6a',
            deviceCreateDate=1603341522000,
            deviceModifyDate=1603676195208,
            deviceMacAddress='null'
        }

3. 사용자 계정에 등록된 장비리스트 조회
===================================================

해당 기능은 사용자 계정에 등록된 장비 리스트를 조회하는 API

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    getMyDeviceList()
    ^^^^^^^

    .. figure:: static/sdk/Device_3.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            getMyDeviceList(accessToken, page, size)

        - Response

        D/DeviceManageActivity: getMyDeviceList() Result 200
        MyDevicesInfo {
            size=10,
            page=0,
            total=1,
            first=true,
            last=true,
            items=[
                Device {
                    deviceMuid='931c50f7f25b4754d2d84f1192738985',
                    deviceToken='e8b3dc7cf80a4ce9bd7c06b5a22127ea',
                    developerUserMuid='9109204ebd381824578b652150256d6a',
                    userRegistered=true,
                    enabled=true,
                    deviceModel=DeviceModel {
                        modelMuid='930c1f66cc2746b49c9aff9e5f8da31f',
                        modelSerialNumber='dfdsfsdf',
                        developerUserMuid='9109204ebd381824578b652150256d6a',
                        modelImageUri='null',
                        modelDuplicationRegistration=false,
                        modelName='sfsdfsdfs',
                        modelDesc='sdfsdfasdfasfas',
                        modelDeveloperName='sdfsdfsd',
                        modelInfoImageUri='null',
                        modelBuyLink='',
                        modelSize='',
                        modelWeight='',
                        status='TEST',
                        modelCreateDate=1603330342000,
                        modelModifyDate=1603330342000
                    },
                    deviceSerialNumber='123456',
                    deviceNickname='테스트장비',
                    version=0,
                    usersMuid=[9109204ebd381824578b652150256d6a],
                    ownerUserMuid='9109204ebd381824578b652150256d6a',
                    deviceCreateDate=1603341522000,
                    deviceModifyDate=1603676195000,
                    deviceMacAddress='null'
                }
            ]
        }

4. 사용자 계정에 등록된 장비리스트 삭제
===================================================

해당 기능은 사용자 계정에 등록된 장비 리스트를 삭제하는 API

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    deleteMyDevice()
    ^^^^^^^

    .. figure:: static/sdk/Device_4.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            deleteMyDevice(accessToken, deviceMuid)

        - Response

            D/DeviceManageActivity: deleteMyDevice() Result 200
            Device {
                deviceMuid='931c50f7f25b4754d2d84f1192738985',
                deviceToken='e8b3dc7cf80a4ce9bd7c06b5a22127ea',
                developerUserMuid='9109204ebd381824578b652150256d6a',
                userRegistered=false,
                enabled=true,
                deviceModel=DeviceModel {
                    modelMuid='930c1f66cc2746b49c9aff9e5f8da31f',
                    modelSerialNumber='dfdsfsdf',
                    developerUserMuid='9109204ebd381824578b652150256d6a',
                    modelImageUri='null',
                    modelDuplicationRegistration=false,
                    modelName='sfsdfsdfs',
                    modelDesc='sdfsdfasdfasfas',
                    modelDeveloperName='sdfsdfsd',
                    modelInfoImageUri='null',
                    modelBuyLink='',
                    modelSize='',
                    modelWeight='',
                    status='TEST',
                    modelCreateDate=1603330342000,
                    modelModifyDate=1603330342000
                },
                deviceSerialNumber='123456',
                deviceNickname='null',
                version=0,
                usersMuid=[],
                ownerUserMuid='null',
                deviceCreateDate=1603341522000,
                deviceModifyDate=1603676410946,
                deviceMacAddress='null'
            }

------------------
OneM2M
------------------

1. 디바이스에서 발생된 데이터를 플랫폼으로 전송
===================================================

해당 기능은 장비에서 발생된 데이터를 Medbiz 플랫폼으로 보내는 API
먼저 디바이스 정보 획득 후, 디바이스 정보와 데이터를 API를 사용해 전송

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    createCin()
    ^^^^^^^

    .. figure:: static/sdk/oneM2M_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            createCin(deviceMuid, deviceToken, field, Cin)

        - Response

            D/Onem2mActivity: createCin() Result : null / 201
            {
                "m2m:cin": {
                    "rn":"4-20201026014754529l0Q6",
                    "ty":4,
                    "pi":"eknnueBQG6xZ",
                    "ri":"c4KNT9SMeC",
                    "ct":"20201026T014754",
                    "et":"20231026T014754",
                    "lt":"20201026T014754",
                    "st":0,
                    "cs":112,
                    "cnf":"application/json",
                    "con": {
                        "systolic":120,
                        "diastolic":80,
                        "pulse":60,
                        "map":90,
                        "custom": {
                            "wave":[1,2,3,4,5],
                            "mode":"mode 1",
                            "user_index":0
                        }
                    },
                    "cr":"e8b3dc7cf80a4ce9bd7c06b5a22127ea"
                }
            }

    ----
    :column: col-lg-12 col-md-12 col-sm-12 p-1

    데이터 전송 후, 데이터 확인
    ^^^^^^^

    .. figure:: static/sdk/oneM2M_2.png


------------------
VFS
------------------

1. 유저 홈 경로 얻기
===================================================

해당 기능은 VFS 기능을 사용 전에 유저의 홈 경로(기준)를 알아내기 위해 사용
먼저 로그인 인증(31페이지)을 수행 후 AccessToken을 획득하고 있어야 함
userHomePath = "/home/" + userMe.getUserName()  > /home/{$유저ID}

getUserMeButton.setOnClickListener, Drive Class 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    userMe()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_1.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            userMe(accessToken)

        - Response

            D/VfsActivity: userMe() Result :200
            UserMe {
                userMuid='910f1b0829eae5444ba82238084c5a2d',
                userId='gemscrc',
                email='gemscrc@gwnu.ac.kr',
                createAt='2019-03-12T01:52:05.000+0000',
                userName='gemscrc',
                authorities=[ROLE_USER, ROLE_DEVELOPER, ROLE_VENDOR]
            }

2. 유저 홈 경로 이동
===================================================

유저의 홈 경로(기준)를 획득 후, 해당 홈 경로로 이동하는 기능
기존에 생성된 ★의 “/home/{유저 계정명}” 로 경로 이동

cdHomeButton.setOnClickListener, Drive Class 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    cdHome()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_2.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            cdHome(accessToken)

        - Response

            D/VfsActivity: cdHome() Result :200
            VfsCatalog{
                catalogCreateDatetime=1550637677419,
                catalogUpdateDatetime=1551848633983,
                catalogSiteLocked=false,
                id=109,
                pid=2,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='null',
                name='gemscrc',
                permission='740',
                dir='1',
                size=null,
                secret=false,
                ownerGroup='null',
                available=true
            }

3. ls 기능을 통한 현재 경로 파일 탐색
===================================================

★의 “Catalog-Id“ 의 경로, 즉 현재 경로에 존재하는파일 및 폴더를 검색

lsButton.setOnClickListener, Drive Class 참조


.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    ls()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_3.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            ls(accessToken)

        - Response

            D/VfsActivity: ====================== ls() Results start ======================
            D/VfsActivity: ls() Result :200
            VfsCatalog{
                catalogCreateDatetime=1587031642537,
                catalogUpdateDatetime=1587031642577,
                catalogSiteLocked=false,
                id=57491,
                pid=109,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='946563a55344234314802b265f6fae0d',
                name='wifi기업지원그림.png',
                permission='740',
                dir='0',
                size=268379,
                secret=false,
                ownerGroup='null',
                available=true
            }

            …. 조회 리스트 계속

            VfsCatalog{
                catalogCreateDatetime=1572944029532,
                catalogUpdateDatetime=1572944029579,
                catalogSiteLocked=false,
                id=35932,
                pid=109,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='949e02a3d4088546ddb88c19e9b1c03f',
                name='체온계_시리얼.xlsx',
                permission='740',
                dir='0',
                size=11260,
                secret=false,
                ownerGroup='null',
                available=true
            }
            D/VfsActivity: ====================== ls() Results  end  ======================

4. mkdir 기능을 통한 디렉토리 생성
===================================================

★의 “Catalog-Id“ 의 경로, 즉 홈 경로에 디렉토리를 생성한다.
생성할 디렉토리 이름을 기입 후, 디렉토리 생성 버튼 터치

mkdirButton.setOnClickListener, Drive Class 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    mkdir()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_4.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            mkdir(accessToken)

        - Response

            D/VfsActivity: mkdir() Result :200
            VfsCatalog{
                catalogCreateDatetime=1604922869540,
                catalogUpdateDatetime=1604922869540,
                catalogSiteLocked=false,
                id=79302,
                pid=109,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='null',
                name='sampleDir',
                permission='740',
                dir='1',
                size=0,
                secret=false,
                ownerGroup='null',
                available=true
            }

5. cd 기능을 통한 디렉토리 이동
===================================================

홈 경로(“/home/gemscrc”)에서 다른경로(“/home/gemscrc/sampleDir”)로의  이동을 위한 명령어다.
이동 디렉토리이름을 기입한 후, 이동 버튼 터치 > Catalog-Id 변경됨을 알 수 있음

cdButton.setOnClickListener, Drive Class 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    cd()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_5.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            cd(accessToken)

        - Response

            D/VfsActivity: cd() Result :200
            VfsCatalog {
                catalogCreateDatetime=1604922869540,
                catalogUpdateDatetime=1604922869540,
                catalogSiteLocked=false,
                id=79302,
                pid=109,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='null',
                name='sampleDir',
                permission='740',
                dir='1',
                size=0,
                secret=false,
                ownerGroup='null',
                available=true
            }

6. 파일(이미지) 선택 후, 파일 전송
===================================================

현 경로(“/home/gemscrc/sampleDir”, 5번에서 이동 된 Catalog-Id)로선택된 파일을 플랫폼 VFS로 전송한다.

putButton.setOnClickListener, Drive Class 참조

.. panels::
    :container:

    :column: col-lg-4 col-md-4 col-sm-4 p-1

    put()
    ^^^^^^^

    .. figure:: static/sdk/Vfs_6.png

    ---
    :column: col-lg-8 col-md-8 col-sm-8 p-1

    소스 코드 주석
    ^^^^^^^

    .. code::

        - Request

            put(accessToken, file)

        - Response

            D/VfsActivity: result /home/gemscrc/sampleDir/IMG_20201109_005822.jpg
            VfsCatalog{
                catalogCreateDatetime=1604924287688,
                catalogUpdateDatetime=1604924287688,
                catalogSiteLocked=false,
                id=79305,
                pid=79302,
                owner='gemscrc',
                group='gemscrc',
                catalogMuid='94b789d4993c2f4f2880428591bcc3dc',
                name='IMG_20201109_005822.jpg',
                permission='740',
                dir='0',
                size=200588,
                secret=false,
                ownerGroup='null',
                available=true
            }
