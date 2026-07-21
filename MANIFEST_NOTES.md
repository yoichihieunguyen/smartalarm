# AndroidManifest.xml — phần cần khai báo cho các component mới

Folder này không chứa `AndroidManifest.xml`, nên khi copy code vào project
Android Studio, cần thêm các khai báo sau.

## Permissions (đặt trên thẻ `<application>`)

```xml
<uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
<uses-permission android:name="android.permission.USE_EXACT_ALARM" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<uses-permission android:name="android.permission.USE_FULL_SCREEN_INTENT" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
```

## Components (đặt trong thẻ `<application>`)

```xml
<activity
    android:name=".Activities.MainActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity
    android:name=".Activities.AlarmRingActivity"
    android:excludeFromRecents="true"
    android:exported="false"
    android:launchMode="singleTop"
    android:showWhenLocked="true"
    android:turnScreenOn="true" />

<service
    android:name=".Service.AlarmService"
    android:exported="false"
    android:foregroundServiceType="mediaPlayback" />

<receiver
    android:name=".Receiver.AlarmReceiver"
    android:exported="false" />

<receiver
    android:name=".Receiver.BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

## Resource cần có sẵn trong project

- `res/raw/alarm1.mp3` … `alarm5.mp3` — `MusicHelper` tham chiếu 5 file này.
- Các drawable đã được layout tham chiếu: `bg_card`, `bg_circle_button`,
  `bg_circle_accent`, `bg_circle_time`, `bg_picker_highlight`,
  `bg_button_primary`, `bg_button_secondary`.
- Dependency Material Components (cho `BottomNavigationView`):
  `implementation 'com.google.android.material:material:1.11.0'`

## Lưu ý runtime

- Android 12+ (API 31): nếu chưa được cấp `SCHEDULE_EXACT_ALARM`,
  `AlarmScheduler` tự fallback sang alarm không exact (có thể lệch vài phút).
  Muốn chính xác tuyệt đối thì mở Settings cấp quyền "Alarms & reminders".
- Android 13+ (API 33): `MainActivity` đã tự xin quyền `POST_NOTIFICATIONS`.
- Database đã nâng lên version 2 (đổi cột `repeat_days` sang TEXT) —
  cài đè app cũ sẽ bị xóa dữ liệu alarm (onUpgrade drop table).
