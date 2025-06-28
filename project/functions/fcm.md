## 🔍 FCM (Firebase Cloud Messaging)

**FCM**은 Firebase Cloud Messaging의 약자로,  
무료로 안정적인 메시지 전송이 가능한 **크로스 플랫폼 메시징 솔루션**입니다. <br>

제가 FCM을 사용하는 목적은,  
회사 서버 측에서 앱 사용자에게 특정 상황에 대한 이벤트 메시지를 전달하기 위해서입니다. <br><br>

예를 들어, 사용자가 사진을 촬영하여 서버에 이미지, 사용자 ID, 위치 정보 등을 전송할 경우,  
이와 관련된 **포인트 지급**, **이벤트 안내** 등의 메시지를 앱으로 전달하는 데 활용됩니다. <br><br>

Firebase는 오픈된 레퍼런스가 많고, 설정 및 구현이 쉬워 빠르게 도입할 수 있었습니다. <br>

---

### 📦 FCM 서비스 클래스 예시

```kotlin
class MyFirebaseMessagingService : FirebaseMessagingService() {

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        sendNotification(remoteMessage)
    }

    /* 알림 생성 메서드 */
    private fun sendNotification(remoteMessage: RemoteMessage) {
        val uniId: Int = (System.currentTimeMillis() / 7).toInt()

        val intent = Intent(this, CoupangPartnerActivity::class.java).apply {
            flags = Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP
            // 데이터 payload 전달
            for (key in remoteMessage.data.keys) {
                putExtra(key, remoteMessage.data[key])
            }
        }

        val pendingIntent = PendingIntent.getActivity(
            this, uniId, intent, PendingIntent.FLAG_ONE_SHOT
        )

        val channelId = "my_channel"
        val soundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)

        val notificationBuilder = NotificationCompat.Builder(this, channelId)
            .setSmallIcon(R.drawable.winter1)
            .setContentTitle(remoteMessage.data["title"])
            .setContentText(remoteMessage.data["body"])
            .setAutoCancel(true)
            .setSound(soundUri)
            .setContentIntent(pendingIntent)

        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.notify(uniId, notificationBuilder.build())
    }
}
```

---

### ⚙️ AndroidManifest.xml 설정

```xml
<service
    android:name=".service.MyFirebaseMessagingService"
    android:enabled="true"
    android:exported="true">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT" />
    </intent-filter>
</service>
```

<br>
exported="true"로 설정해야 Firebase가 외부에서 com.google.firebase.MESSAGING_EVENT 액션을 통해 MyFirebaseMessagingService 클래스에 접근할 수 있습니다. <br>
해당 설정이 없으면 메시지 수신 콜백이 동작하지 않습니다. <br>

---

## 📝 Feature Check List
- [x] Firebase Cloud Messaging 연동
- [x] 푸시 알림 수신
- [x] 알림 클릭 시 화면 전환

---

## 📷 Screenshot

| 푸시 알람 | 푸시 알람 클릭 |
|:--------:|:--------------:|
| <img src="https://github.com/user-attachments/assets/50661241-3af5-4fee-838a-7d72bfc5c856" width="250" height="80"/> | <img src="https://github.com/user-attachments/assets/5a4d97a5-99f6-48d6-a983-7bf1e511e332" width="250" height="550"/> |

---

## 🔗 Reference

- [Firebase Cloud Messaging 기본 개념 및 구현](https://donghun.dev/Firebase-Cloud-Messaging)  
- [FCM 레퍼런스 예시](https://seopseop911.tistory.com/20)
