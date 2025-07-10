## 🔍 UAAL (Unity as a Library)

`UAAL`은 Android를 포함한 다양한 플랫폼에서 Unity 기반의 AR 기능을 통합할 수 있는 프레임워크입니다. <br>
ARAD 에서는 이 UAAL을 통해 **AR 이미지 생성 및 촬영 기능을 구현**하고, 해당 데이터를 서버에 업로드하는 기능을 개발하였습니다.
<br><br>

## 📝 Feature Check List
- [x] UAAL 설정 및 적용  
- [x] Android 앱(ARAD) ↔ Unity 앱(UAAL) 간 데이터 송수신  
- [x] 촬영된 이미지 데이터를 회사 서버에 업로드
<br><br>

## 📷 Screenshot

  
|   유니티 연동1  |   유니티 연동2   |
| :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/ea1ee3e1-5aab-4933-8fec-63f523134552" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/b0dbae5c-bf74-4764-9950-692fdb22282c" width="200" height="450"/> |

<br>

## 📮 관련 이슈

### Android ↔ Unity 간 데이터 송수신 방식
Unity와 Android는 각자 별도의 앱 프로세스로 동작하므로 `room` 또는 `SharedPreference` 공유가 어렵습니다.  
Unity와 Android 간 데이터 교환을 위해 제공되는 UnityPlayer.UnitySendMessage 등의 양방향 인터페이스를 활용하여 통신 구조를 구성하여,<br>
Android에서 Unity로 메시지를 보내거나, Unity에서 Android 메서드를 직접 호출하는 방식으로 구현하였습니다. <br><br><br>

### UAAL 통합으로 인한 앱 용량 이슈
Unity 모듈 통합으로 인해 Android 앱의 APK 용량 증가 문제가 발생하였습니다.  
ARAD에서는 Android 프로젝트 내에서 불필요한 Unity 리소스 제거, `split` 설정을 통한 ABI별 분리 빌드 적용, `proguard` 및 `shrinkResources` 설정을 통해 경량화를 수행하였습니다.
