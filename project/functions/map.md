## 🔍 Google Map

총 3가지 지도 API(Google, Kakao, Naver) 중 **Google Map**을 선택한 이유는 다음과 같습니다. <br>

- **다양한 API 및 개발 자료 제공**  
  - 카메라 이동 및 회전  
  - 마커 및 지도 위 오브젝트 표시  
  - TimeZone 등 다양한 기능 제공  
- **전 세계 데이터 커버리지**  
  - ARCore Geospatial API 기반 좌표 데이터를 활용해 여러 국가에서 활용 가능  
- **풍부한 지도 커스터마이징 기능**
  - 지도 스타일, 마커 디자인 등 다양한 시각적 요소를 자유롭게 설정 가능

---

## 📝 Feature Check List

- [x] 구글 Map API 연동  
- [x] 맵 배경 커스터마이징  
- [x] 마커 Bitmap 커스텀  
- [x] POI(위·경도) 기반 마커 활성화  
- [x] 내 위치 → 지하철 위치 거리 및 AR 마커 거리 측정

---

## 📷 Screenshot
<h1 align="center">
|   구글 맵1   |   구글 맵2   |  구글 맵3 |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/f2f1aea6-041c-4ba5-9d28-5428a8c295b0" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/f810c3d4-a82b-4ffb-bead-c388fdfcdf58" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/4ab83bdc-69be-474f-91c7-6b75ea455f38" width="200" height="450"/> |
</h1>

---

## 📮 관련 이슈

### 1️⃣ 구글 맵 스타일 커스텀

`GoogleMap.setMapStyle()`을 통해 스타일 커스텀을 적용했습니다. <br>  
[스타일 지정 마법사](https://mapstyle.withgoogle.com/)를 사용했고, 디자인은 [SnazzyMaps - Subtle Grayscale](https://snazzymaps.com/style/15/subtle-grayscale)을 참고했습니다.

```kotlin
mGoogleMap.setMapStyle(MapStyleOptions.loadRawResourceStyle(mContext, R.raw.style2_json))
```

📎 [Google Map API Docs](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView?authuser=0)

---

### 2️⃣ 마커 커스텀

커스텀 이미지와 타이틀을 포함한 마커를 생성합니다.

```kotlin
mGoogleMap.addMarker(
    MarkerOptions()
        .position(ttukseomStarbucks)
        .title("뚝섬 스타벅스")
        .icon(BitmapDescriptorFactory.fromBitmap(bitmapStarBucks!!))
)
```

```kotlin
bitmapStarBucks = createUserBitmap(
    BitmapFactory.decodeResource(
        mActivity.resources, R.drawable.ic_profile_ex2
    )
)
```

---

### 3️⃣ POI 위·경도 기반 카메라 이동 및 마킹

MapView `onCreate()` 이후 서버로부터 위경도 데이터를 수신하고 콜백을 활용해 `setLastLocation()`을 실행합니다.

```kotlin
override fun setLastLocation(location: Location) {
    val myLocation = LatLng(location.latitude, location.longitude)

    val marker = MarkerOptions()
        .position(myLocation)
        .title("i am here")

    val cameraOptions = CameraPosition.Builder()
        .target(myLocation)
        .zoom(18.0f)
        .tilt(45.0f)
        .bearing(90f)
        .build()

    val camera = CameraUpdateFactory.newCameraPosition(cameraOptions)

    MyLocation = myLocation
    MyCameraOptions = cameraOptions
    MyCamera = CameraUpdateFactory.newCameraPosition(MyCameraOptions)
}
```

📎 [Google Maps - Camera & View](https://developers.google.com/maps/documentation/android-sdk/views?hl=ko)

---

## 📚 Reference
- https://breadboy.tistory.com/282  
- https://developers.google.com/maps/documentation/get-map-id?hl=ko&authuser=0  
- https://stackoverflow.com/questions/39315219/custom-marker-with-user-image-inside-the-pin

