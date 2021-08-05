MEDBIZ Wiki Developer Information
=================================

위키의 작성을 위해서는 아래의 과정을 따라서 수행할 수 있습니다

작성된 위키는 커밋이 되면 자동적으로 반영이 되어 실제 문서에 업데이트됩니다

반영에는 다소 시간이 소요될 수 있습니다 (대부분 1분 이내)

`위키 사이트 <https://medbiz.readthedocs.io/en/latest/>`_

Purpose
-------

본 위키 작성을 통해 사용자들에게 플랫폼의 사용법을 알리는데 목적이 있습니다

feature
-------

- readthedocs를 이용한 위키 호스팅
- 사내 gitlab과의 연동을 통한 문서 관리
- 사내 gitlab과의 연동을 통한 버전 관리
- .rst 형식을 이용한 문서 작성

Requirements
------------
작성을 하고자 하시는 분들은 아래의 방법을 알고 계셔야 합니다

Mandatory

- reStructuredText 문법

Optinal

- readthedocs 의 기본 구조 이해 `참조 <https://docs.readthedocs.io/en/latest/>`_
- git 사용법 (약간의)
- Sphinx 사용법

Sphinx 빌드할 때  패키지 에러 발생시 플러그인을 설치해주세요.

- pip install -r requirements.txt

Structure
---------

1. .rst 형태의 파일로 문서를 작성 후 gitlab에 커밋을 하게 됩니다

2. 커밋이 되면 gitlab은 webhook으로 readthedocs에 커밋이 되었음을 알립니다.

3. readthedocs는 커밋 신호가 들어오면 해당 문서를 git에서 pull하여 html로 컴파일합니다

4. 해당 문서가 호스팅됩니다.

Building part of a book
-----------------------

기본적으로는 로컬에서 작성한 .rst 형태의 문서를 gitlab에 커밋하는 것으로 끝이 나게됩니다.

작성은 온라인 reStructuredText editor를 이용하여도 됩니다.

아래의 방법은 로컬에서 진행하는 방법입니다.

파이썬은 먼저 설치되어있어야 합니다

.. code-block:: console

   pip install sphinx
   git clone http://203.255.217.236:9009/medbiz/medbiz-user-guide.git
   make html

상기 과정을 수행하면 로컬에서 rst를 html로 컴파일 된 파일을 클릭하여 확인할 수 있습니다


Author
------

다음 메일로 연락주세요 : ktj1312@gwnu.ac.kr

License
-------

The project is licensed under the `medbizDevTeam <http://211.185.64.12:9003/>`_