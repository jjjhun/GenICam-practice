# GenICam Code Practice
GenICam 표준 관련 연습 코드 정리 repository

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
__[EMVA GenICam Introduction]__  
  https://www.emva.org/standards-technology/genicam/introduction-new/

## GenTL 프로토콜 연결 구현(C++)
- 이 항목에서는 GenICam GenTL Standard Version 1.6을 바탕으로 System → Interface → Device → Datastream 까지 Open하는 방법에 대해 예시 코드와 함께 설명하고자 한다.  
[필요 자료] https://www.emva.org/standards-technology/genicam/genicam-downloads/

### 1. GenTL Producer 동적 Loading

Window Ver.  
작성중

Linux Ver.  
작성중

### 2. GenTL 인터페이스 함수 선언 및 할당   
작성중

### 3. Transport Layer 연결   
작성중

### 전체 코드
```cpp
#include "GenTL.h"           // GenTL 헤더 파일(EMVA 제공)
#include <Windows.h>
#include <libloaderapi.h>    // GenTL Producer로 부터 GenTL 구현 모듈 동적 Loading를 위한 헤더(Window OS용, Linux의 경우 별도 라이브러리 사용 필요)
#include <iostream>

int main()
{
	std::string filename;
	filename = "D:/GenTLproducer/mvGenTLProducer.cti";   // 별도 다운받은 GenTL Producer 경로 지정
	uint32_t NumInterfaces;
	uint32_t NumDevices;
	uint32_t NumStreams;

	size_t BufferSize;

	HMODULE loadeddll = LoadLibraryA(filename.c_str());  // GenTL Producer 동적 Loading을 위한 호출

  // GenTL 헤더의 인터페이스 함수 선언

	GenTL::PGCInitLib GCInitLib;
	GenTL::PGCCloseLib GCCloseLib;
	GenTL::PTLOpen TLOpen;
	GenTL::PTLClose TLClose;
	GenTL::PTLUpdateInterfaceList TLUpdateInterfaceList;
	GenTL::PTLOpenInterface TLOpenInterface;
	GenTL::PTLGetNumInterfaces TLGetNumInterfaces;
	GenTL::PTLGetInterfaceID TLGetInterfaceID;
	GenTL::PIFUpdateDeviceList IFUpdateDeviceList;
	GenTL::PIFGetNumDevices IFGetNumDevices;
	GenTL::PIFGetDeviceID IFGetDeviceID;
	GenTL::PIFOpenDevice IFOpenDevice;
	GenTL::PDevGetNumDataStreams DevGetNumDataStreams;
	GenTL::PDevGetDataStreamID DevGetDataStreamID;
	GenTL::PDevOpenDataStream DevOpenDataStream;
	GenTL::PDSClose DSClose;
	GenTL::PDevClose DevClose;
	GenTL::PIFClose IFClose;
	GenTL::PTLGetInterfaceInfo TLGetInterfaceInfo;

  // Transport Layer 핸들 선언

	GenTL::TL_HANDLE hTL;
	GenTL::IF_HANDLE hIface;
	GenTL::DEV_HANDLE hDevice;
	GenTL::DS_HANDLE hStream;

  // GenTL Producer에서 인터페이스 함수 구현부 동적 Loading 후 할당

	GCInitLib = (GenTL::PGCInitLib)GetProcAddress(loadeddll, "GCInitLib");
	GCCloseLib = (GenTL::PGCCloseLib)GetProcAddress(loadeddll, "GCCloseLib");
	TLOpen = (GenTL::PTLOpen)GetProcAddress(loadeddll, "TLOpen");
	TLClose = (GenTL::PTLClose)GetProcAddress(loadeddll, "TLClose");
	TLUpdateInterfaceList = (GenTL::PTLUpdateInterfaceList)GetProcAddress(loadeddll, "TLUpdateInterfaceList");
	TLOpenInterface = (GenTL::PTLOpenInterface)GetProcAddress(loadeddll, "TLOpenInterface");
	TLGetNumInterfaces = (GenTL::PTLGetNumInterfaces)GetProcAddress(loadeddll, "TLGetNumInterfaces");
	TLGetInterfaceID = (GenTL::PTLGetInterfaceID)GetProcAddress(loadeddll, "TLGetInterfaceID");
	IFUpdateDeviceList = (GenTL::PIFUpdateDeviceList)GetProcAddress(loadeddll, "IFUpdateDeviceList");
	IFGetNumDevices = (GenTL::PIFGetNumDevices)GetProcAddress(loadeddll, "IFGetNumDevices");
	IFGetDeviceID = (GenTL::PIFGetDeviceID)GetProcAddress(loadeddll, "IFGetDeviceID");
	IFOpenDevice = (GenTL::PIFOpenDevice)GetProcAddress(loadeddll, "IFOpenDevice");
	DevGetNumDataStreams = (GenTL::PDevGetNumDataStreams)GetProcAddress(loadeddll, "DevGetNumDataStreams");
	DevGetDataStreamID = (GenTL::PDevGetDataStreamID)GetProcAddress(loadeddll, "DevGetDataStreamID");
	DevOpenDataStream = (GenTL::PDevOpenDataStream)GetProcAddress(loadeddll, "DevOpenDataStream");
	DSClose = (GenTL::PDSClose)GetProcAddress(loadeddll, "DSClose");
	DevClose = (GenTL::PDevClose)GetProcAddress(loadeddll, "DevClose");
	IFClose = (GenTL::PIFClose)GetProcAddress(loadeddll, "IFClose");

	/////////////Transport Layer 연결/////////////

   std::cout << "init : " << GCInitLib() << std::endl;
	std::cout << "Open TL: " << TLOpen(&hTL) << std::endl;
	std::cout << "Update Interface List : " << TLUpdateInterfaceList(hTL, NULL, 3) << std::endl;
	std::cout << "Get Num Interface : " << TLGetNumInterfaces(hTL, &NumInterfaces) << std::endl;

    if (NumInterfaces > 0)
    {
      std::cout << "Get Interface ID : " << TLGetInterfaceID(hTL, 1, NULL, &BufferSize) << std::endl;
      char IfaceID = (char)malloc(BufferSize);
      TLGetInterfaceID(hTL, 1, &IfaceID, &BufferSize);
  
      printf("%s\n", &IfaceID);
  
      std::cout << "Open Interface : " << TLOpenInterface(hTL, &IfaceID, &hIface) << std::endl;
      std::cout << "update device list :" << IFUpdateDeviceList(hIface, NULL, 1000) << std::endl;
      std::cout << "Get num devices : " << IFGetNumDevices(hIface, &NumDevices) << std::endl;
    
      if (NumDevices > 0)
      {
        std::cout << "Get Device ID : " << IFGetDeviceID(hIface, 0, NULL, &BufferSize) << std::endl;
        char DeviceID = (char)malloc(BufferSize);
        IFGetDeviceID(hIface, 0, &DeviceID, &BufferSize);
  
        printf("%s\n", &DeviceID);
  
        std::cout << "Open Device :" << IFOpenDevice(hIface, &DeviceID, 4, &hDevice) << std::endl;
        std::cout << "Get Num Stream" << DevGetNumDataStreams(hDevice, &NumStreams) << std::endl;
  
        if (NumStreams > 0)
        {
          std::cout << "Get DataStream ID : " << DevGetDataStreamID(hDevice, 0, NULL, &BufferSize) << std::endl;
          char StreamID = (char)malloc(BufferSize);
          DevGetDataStreamID(hDevice, 0, &StreamID, &BufferSize);
  
          printf("%s\n", &StreamID);
  
          std::cout << "Get Stream ID : " << DevOpenDataStream(hDevice, &StreamID, &hStream) << std::endl;
          std::cout << "DataStream closed : " << DSClose(hStream) << std::endl;
        }
  
        std::cout << "Device closed : " << DevClose(hDevice) << std::endl;
      }
      std::cout << "Interface closed : " << IFClose(hIface) << std::endl;
    }
	std::cout << "TL closed : " << TLClose(hTL) << std::endl;
	std::cout << "GC closed : " << GCCloseLib() << std::endl;
}

```
__참고 사이트__  
__[GenICam 디바이스 핸들링 파이썬 오픈소스 라이브러리(Harvester)]__  
https://github.com/genicam/harvesters  
(GenICam 기반 머신비전 디바이스 연결을 위한 디바이스 feature 매핑, 디바이스 통신 함수 제공)

__[GenTL Driver 다운로드(Harvester Documentation)]__  
https://harvesters.readthedocs.io/en/latest/INSTALL.html#installing-a-gentl-producer → Installing a GenTLProducer 항목 참고  
(Matrox Vision 社의 GenTL Producer 파일 제공)

