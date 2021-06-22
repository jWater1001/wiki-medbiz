=============
디바이스 연동
=============

.. tip::

    MEDBIZ 플랫폼에 제품을 연동하려면 우선 개발자 등록을 필요로 합니다.
    `여기 <https://medbiz-user-guide.readthedocs.io/ko/latest/developer/guide.html>`_ 를 참조하여 개발자 등록을 진행할 수 있습니다.

디바이스를 플랫폼에 연동하는 절차는 다음과 같은 절차로 구성되어 있습니다.

* `제품 등록`_
* `제품 Manifest 등록`_
* `제품의 디바이스 생성`_

.. tip::
    MEDBIZ 플랫폼에 제품을 등록하기 위해 등록에 필요한 정보를 미리 준비해주세요.

---------
제품 등록
---------
    플랫폼에 제품을 등록하기 위한 절차를 설명합니다.

    |Enroll Device Model Image|

제품 이름
-----------------
    제품 이름은 상품을 대표하는 이름으로 예를들어 SAMSUNG의 스마트폰인 'Galaxy S'와 같은 이름을 가집니다.

중복 등록
-----------------
    제품이 여러 유저에게 등록되어 사용할 수 있도록 하는 경우 선택할 수 있습니다.

모델 코드
-----------------
     모델 코드는 제품의 브랜드명이아닌 귀사의 관리체계에 있는 코드명을 의미합니다. ex) SAMSUNG Galaxy S10 => SM-G975N

제조사
-----------------
    플랫폼에 등록하려는 제품의 제조사 정보를 기입할 수 있습니다.

설명
-----------------
    플랫폼에 등록하려는 제품의 전반적인 설명을 기입할 수 있습니다.

제품 이미지
-----------------
    플랫폼에 등록하려는 제품의 썸네일 이미지를 등록할 수 있습니다.

크기 및 무게
-----------------
    플랫폼에 등록하려는 제품의 실 크기와 무게 정보를 표현할 수 있습니다.

상세정보 이미지
-----------------
    플랫폼에 등록하려는 제품의 상세 이미지를 등록할 수 있습니다.

구매 링크
-----------------
    구매링크는 해당 제품을 구매할 수 있는 정보를 담고 있는 링크가 있는 경우 기입할 수 있습니다.

------------------
제품 Manifest 등록
------------------
    플랫폼에 제품의 Manifest 등록하기 위한 절차를 설명합니다

    제품의 Manifest는 예를 들어 온습도계와 같은 장비 모델을 등록했을 때, 온도, 습도에 대한 데이터에 대한 별도의 메타데이터 정보를 기입함으로
    써 데이터 조회 시 용이하게 사용할 수 있습니다.

    우선 등록한 제품의 제품 정보화면에서 아래의 그림과 같이 매니페스트 생성 버튼을 클릭합니다.

    |Create Manifest Image|

    Field Name은 수집하고자 하는 데이터의 이름을 설정하고, Description은 수집하고자 하는 데이터의 부가적인 설명을 기입합니다.

    |Input Manifest Image|

    데이터 별로 미리 설정된 파라미터를 선택하기 위해 아래 그림의 + 버튼을 클릭합니다.

    |Create Parameter Image|

    수집하고자 하는 데이터의 성격과 비슷한 파라미터를 고르거나 없는 경우는 별도의 신규 파라미터 신청 후, 파라미터를 선택합니다.

    |Select Parameter Image|

    추가할 데이터 필드나 파라미터가 없는 경우 Activate 버튼을 눌러 제품의 Manifest 정보 입력을 완료합니다.

    |Activate Parameter Image|

------------------
제품의 디바이스 생성
------------------
    플랫폼에 등록된 제품의 디바이스를 추가하기 위한 절차를 설명합니다


.. |Enroll Device Model Image| image:: /_static/enroll_model.png
    :scale: 100
.. |Create Manifest Image| image:: /_static/create_manifest.png
    :scale: 100
.. |Input Manifest Image| image:: /_static/input_manifest.png
    :scale: 100
.. |Create Parameter Image| image:: /_static/create_parameter.png
    :scale: 100
.. |Select Parameter Image| image:: /_static/select_parameter.png
    :scale: 100
.. |Activate Parameter Image| image:: /_static/activate_manifest.png
    :scale: 100