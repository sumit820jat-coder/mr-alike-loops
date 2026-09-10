# Stream Loop (Personal Use App)

Ek Android app jisme 5 tabs hain — **Home, Stream, Live, Videos, Settings** — jaise dashboard-style streaming apps mein hota hai. Background mein FFmpeg se video ko infinite loop karke RTMP se push kiya jaata hai.

**Zaroori baat:** APK sirf `.zip` ko rename karke nahi banta — real build karna padta hai. Neeche 2 tareeke diye hain, dono free hain.

## Tareeka 1 — Bina Android Studio install kiye (GitHub Actions se free APK build)

1. https://github.com pe free account banao (agar nahi hai)
2. Naya **repository** banao (Public rakh sakte ho, koi dikkat nahi — ye sirf aapka personal build hai)
3. Is `StreamLoop` folder ka **pura content** us repo mein upload kar do:
   - GitHub website pe repo kholo → "Add file" → "Upload files" → is poore folder ke andar ki saari files aur sub-folders (`.github` folder bhi!) drag karke daal do → Commit
4. Upload hote hi **Actions** tab pe jao — "Build Debug APK" workflow apne aap chalna shuru ho jayega (2-4 min lagega)
5. Workflow complete hone ke baad usi run ke andar **Artifacts** section mein "StreamLoop-debug-apk" milega — download karo, andar `app-debug.apk` hoga
6. Ye APK phone mein bhej ke install kar lo (Unknown Sources allow karna padega)

Isme kuch install nahi karna — sab kuch GitHub ke servers pe hota hai, free of cost.

## Tareeka 2 — Android Studio se local build

1. **Android Studio** install karo: https://developer.android.com/studio
2. Ye poora `StreamLoop` folder Android Studio mein **Open** karo (File → Open)
3. Gradle sync hone do (internet chahiye — libraries download hongi)
4. **Build → Build Bundle(s) / APK(s) → Build APK(s)**
5. APK yahan milega: `app/build/outputs/apk/debug/app-debug.apk`

## Use kaise karein

1. **Videos** tab mein jaake apna video upload karo (app ki apni storage mein safe copy ho jaayega)
2. **Stream** tab mein platform "YouTube" select karo, apni Stream Key paste karo (YouTube Studio → Go Live → Stream key)
3. Uploaded video list se video choose karo
4. "Start Streaming" dabao — **Live** tab pe status dikhega, notification bhi aayega
5. Rokne ke liye "Stop" dabao (Stream ya Live tab se)

## Important Notes

- **Phone poori tarah OFF ho ya internet na ho, to stream kaam nahi karegi — ye kisi app se fix nahi ho sakta.** Phone ON rehna chahiye (screen off chalega) aur internet connection chahiye.
- App ab **wake lock** use karti hai — screen off hone par bhi background mein streaming chalti rahegi jab tak phone ON hai
- **Home tab** pe agar "Battery optimization ON hai" warning dikhe, to "Battery Optimization Band Karo" button dabao — warna Android kabhi kabhi app ko background mein khud hi rok deta hai
- Video ka format H.264 video + AAC audio (.mp4) hona chahiye — YouTube-compatible
- 24/7 chalane ke liye phone ko **charging pe** rakho aur **stable WiFi/data** connection zaroori hai
- Sirf "YouTube" aur "Custom RTMP" options functional hain
- Ye sirf **aapke apne channel** ke liye personal use hai — stream key kabhi share mat karo

## Project Structure

```
StreamLoop/
├── app/
│   ├── build.gradle
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/personal/streamloop/
│       │   ├── MainActivity.kt        # Bottom nav host, swaps fragments
│       │   ├── HomeFragment.kt        # Dashboard
│       │   ├── StreamFragment.kt      # Platform + key + video + start/stop
│       │   ├── LiveFragment.kt        # Current live status
│       │   ├── VideosFragment.kt      # Upload + library + storage bar
│       │   ├── SettingsFragment.kt    # Help/Privacy/Terms dialogs
│       │   ├── StreamRepository.kt    # Shared state (LiveData) across screens
│       │   ├── StreamService.kt       # Foreground service running ffmpeg loop
│       │   ├── VideoAdapter.kt        # RecyclerView adapter for library
│       │   └── VideoItem.kt
│       └── res/
│           ├── layout/  (one per fragment + item_video.xml)
│           ├── drawable/ (icons, badges, backgrounds)
│           ├── menu/bottom_nav_menu.xml
│           └── values/
├── build.gradle
└── settings.gradle
```

## Future improvements aap khud kar sakte ho

- Video ko custom thumbnail dikhana library list mein
- Stream key ko encrypted storage (EncryptedSharedPreferences) mein save karna taaki baar baar type na karna pade
- Multiple videos ki playlist/queue banake unko shuffle karna
- Bitrate/resolution control add karna

