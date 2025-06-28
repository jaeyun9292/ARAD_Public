## 🔍 network

서버와의 연결을 위해 `HTTP` 기반의 통신 인터페이스를 바탕으로 네트워크 기능을 구현하였습니다. <br>
통신 방식은 `REST API` 기반이며, 데이터는 `JSON` 형식으로 송수신됩니다.    
해당 네트워크 로직 `Retrofit2` 라이브러를 사용하여 구현하였습니다. <br>

---

### 📡 API 목록

<img src="https://github.com/user-attachments/assets/ed0071e4-b097-444a-b4c0-5a128eb0da9b" width="50%" height="80%" />

---

## 📝 Feature Check List

- [x] API 가이드 기반 네트워크 연결  
- [x] Repository 패턴 적용하여 데이터 접근 로직을 추상화
- [x] 이미지 및 POI 데이터 서버 업로드 / 사용자 정보 수신  
- [x] 시나리오 안정성을 위한 Coroutine 및 Callback 병행 사용

---

## 📷 Screenshot

<h1 align="center">

|  AR 이미지 업로드   |  업로드 요청   |  이미지 승인 or 반려 |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/c8007af9-9fde-405e-b34f-0dcb2ee2c85c" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/b6182c6f-09e7-404d-b03f-5233b9a90eab" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/7d455f94-cc37-40eb-90f4-47631dd33dc6" width="250" height="450"/> | 
 

|  업로드 정보  |
| :-------------: |
<img src="https://github.com/user-attachments/assets/de9ef169-b9c0-4e7e-a305-94f108f5f7e9" width="600" height="450"/> | 

</h1>

---

## 📮 관련 이슈

### ✅ 네트워크 flow 설계 및 유지보수

- Retrofit을 기반으로 하는 네트워크 클래스를 **싱글톤**으로 구성하여,  
  앱 전역에서 단일 인터페이스로 통신이 가능하도록 설계했습니다.  
- 다양한 통신 시나리오에서 발생할 수 있는 오류를 방지하기 위해  
  **flow 제어 및 실패 처리 로직을 명확히 구성**했습니다.

---

### ✅ AR 이미지 데이터 업로드 흐름 개선

- **기존 문제**  
    - UAAL(Unity) 앱에서 촬영한 이미지가 Android 앱으로 전달된 후  
      Unity 앱이 종료된 상태에서 서버 업로드가 이루어져 **실패 발생**  

- **요구 사항**  
    - Unity 앱이 실행된 상태에서 서버 업로드가 이루어져야 함  

- **해결 방식**  
    - Unity 앱에서 Android 앱의 특정 클래스를 직접 호출해  
      업로드 로직을 실행하도록 구조 변경  
    - 결과적으로 Unity와 서버 업로드가 동시에 가능한 안정적인 흐름 구현

---

### ✅ 메인 홈 화면에서의 네트워크 흐름 오류

- **문제 상황**  
    - 앱 실행 시 소셜 로그인 정보(카카오/구글)와 POI 데이터를 서버에서 받아와야 함  
    - 하지만 Coroutine으로 병렬 요청 중  
      **데이터가 준비되기 전에 Google Map이 먼저 실행되며 앱이 종료되는 이슈 발생**

- **해결 방식**  
    - Coroutine 대신 Callback 기반으로 순차 흐름 구성  
        → 소셜 로그인/POI 수신 완료 후 Google Map 실행  
    - **네트워크 실패 대비 처리**도 추가  
        → 기본 이미지와 위치 정보를 우선 마커에 배치하여 지도가 비지 않도록 구성

---

### ✅ 구글 맵 Marker & POI 연동


<h1 align="center">

|  >Google Map + POI Marker 적용   |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/a8ebf5cd-e56a-4604-b47d-1b609faab578" width="200" height="450"/> 

</h1>
