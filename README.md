# GenICam-practice
GenICam 표준 정리 repository

## Repository 소개
GenICam 표준 한글 자료가 부족하여 공부 겸 정리하는 Repository

## GenICam 표준 개요
- __GenICam__(<ins>Gen</ins>eric <ins>I</ins>nterface for <ins>Cam</ins>eras)
  GenICam 표준은 모든 종류의 카메라 디바이스들에 대해 공통적인 개발 환경을 제공하고자 2003년 바르셀로나에서 발족한 유럽 머신비전 협회(EMVA, European Machine Vision Association)에서 작성한 표준으로 __디바이스와의 통신 프로토콜__ 표준부터 __디바이스 제어 프로토콜__ 표준, __명칭__ 표준 모두를 포함한다.

표준명 | 표준화 목적 | 설명 | 기능 | 예시
:---:|:---:|:---:|:---:|:---:
GenApi | 디바이스 구성 API 표준 | 머신비전 디바이스의 feature들을 구성하기 위한 XML 프로토콜 표준 | 디바이스 feature들에 대한 레지스터 매핑 | 디바이스 FPS, Exposure Rate 등의 feature들을 구성, 제어하기 위한 XML 통신 프로토콜 표준
GenTL | 디바이스 전송 계층 표준 | 머신비전 디바이스와의 데이터 통신을 위한 전송 계층 프로토콜 표준 | 디바이스/레지스터 연결, 데이터 송수신 | 시스템(0계층) - 인터페이스(1계층) - 디바이스(2계층) - 데이터스트림(3계층) - 버퍼(4계층) 구성
SFNC | 명칭(Naming Convnetion) 표준 | GenApi, GenTL 등 다른 표준에서 사용하는 명칭 규정 | - | -


GenTL 전송 계층 | 실체
:---:|:---:
System Module | 디바이스 드라이버
Interface Module | 프레임그래버/네트워크 카드
Device Module | 머신비전 디바이스(GigE, CoaXPress 등)
Stream Module | 데이터 스트림 
Buffer Module | 데이터 버퍼

표준 관련 상세 정보의 경우 EMVA 사이트 참고  
__[EMVA GenICam Introduction]__ https://www.emva.org/standards-technology/genicam/introduction-new/

## GenTL
