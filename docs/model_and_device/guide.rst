=============
디바이스 연동
=============

.. tip::

    MEDBIZ 플랫폼에 제품을 연동하려면 우선 개발자 등록을 필요로 합니다.
    `여기 <https://medbiz-user-guide.readthedocs.io/ko/latest/oauth2.html/>`_ 을 확인하여 token을 얻고 사용하는 방법을 익힐 수 있습니다.


절차
--------
    디바이스를 플랫폼에 연동하는 절차는 다음과 같은 절차로 구성되어 있습니다.
    1. 제품 등록
    2. 제품 속성(Manifest) 등록
    3. 제품의 디바이스 생성
    4. 플랫폼 연동

.. tip::

    모든 medbiz 플랫폼의 API 호출은 access token을 필요로 합니다.
    `여기 <https://medbiz-user-guide.readthedocs.io/ko/latest/oauth2.html/>`_ 을 확인하여 token을 얻고 사용하는 방법을 익힐 수 있습니다.

제품 등록
--------------------
    플랫폼에 제품을 등록하기 위한 절차를 설명합니다.

.. toctree::
    :maxdepth: 2

    api/fileAPI
    api/hl7
    api/oneM2M

API Rate Limits
---------------

    프로토콜과 API종류에 따라 상이합니다.
    자세한 사항은 이곳을 통해 확인할 수 있습니다.

API console
-----------

    지원하는 API들을 여기를 통해 테스트를 진행해 볼 수 있습니다.
    API들을 테스트하기 위해서는 플랫폼에 로그인을 하여야 합니다.
    만약 계정이 없다면 회원가입을 진행 후 사용하실 수 있습니다.

.. figure:: _static/CU.jpg