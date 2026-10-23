# 📁 File Manager (Android, Java)

Simple file manager: browse folders, open files, create folder, rename, copy, move, delete, details.
Pure Java, framework Activity, only one dependency (`androidx.core` for FileProvider).

## 1. Project structure & WHERE to add code

```
FileManager/
├── .github/workflows/build.yml      <- builds your APK online (no PC needed)
├── settings.gradle / build.gradle / gradle.properties
└── app/
    ├── build.gradle                 <- app id, version, add libraries here
    └── src/main/
        ├── AndroidManifest.xml      <- permissions, app name, new Activities
        ├── java/com/ebrat/filemanager/
        │   └── MainActivity.java    <- ALL app logic (main file)
        └── res/
            ├── layout/activity_main.xml   <- screen design (XML)
            └── xml/file_paths.xml         <- needed to open files
```

| I want to...                         | Edit this                                                      |
|--------------------------------------|----------------------------------------------------------------|
| Change how a file row looks          | `MainActivity.java` → `getView()` inside `adapter`             |
| Add a top menu item (e.g. Search)    | `onCreateOptionsMenu()` + `onOptionsItemSelected()`            |
| Add a long-press action (e.g. Share) | `showMenu()` → add to `opts` array + a new `case`              |
| Change screen layout / colors        | `res/layout/activity_main.xml`                                 |
| Change app name                      | `AndroidManifest.xml` → `android:label`                        |
| Change package name                  | `app/build.gradle` (`namespace`, `applicationId`) + the Java folder name + `package` line in Java |
| Add a new screen                     | New `XxxActivity.java` + register it in `AndroidManifest.xml`  |
| Add a library                        | `app/build.gradle` → `dependencies { implementation '...' }`   |
| Add app icon                         | Put `ic_launcher.png` in `res/mipmap-xxhdpi/` and add `android:icon="@mipmap/ic_launcher"` in the manifest `<application>` |

### Example: add a "Share" action
In `showMenu()` change the array to `{"Open","Rename","Copy","Move","Delete","Details","Share"}` and add:
```java
case 6:
    Intent s = new Intent(Intent.ACTION_SEND);
    s.setType("*/*");
    s.putExtra(Intent.EXTRA_STREAM, FileProvider.getUriForFile(this, getPackageName() + ".provider", f));
    s.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
    startActivity(Intent.createChooser(s, "Share"));
    break;
```

## 2. Build the APK from your phone (no PC)

### Option A: GitHub Actions (recommended)
1. Create a GitHub repo (e.g. `FileManager`).
2. Upload ALL files from this project (keep the folder structure; the `.github` folder must be included). Use GitHub app/website, or Termux:
   ```bash
   pkg install git
   git init && git add . && git commit -m "first"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/FileManager.git
   git push -u origin main
   ```
3. Open the repo → **Actions** tab → wait for "Build APK" to finish (3-6 min).
4. Open the finished run → **Artifacts** → download `FileManager-APK` → unzip → install `app-debug.apk`.
5. Every time you edit code and push, a new APK is built automatically.

### Option B: Edit with Quick Edit, build on GitHub
Edit `MainActivity.java` in Quick Edit → commit/push → download new APK from Actions.

### Option C: RDP (Windows)
Install Android Studio → Open this folder → Run ▶.

## 3. First run
On Android 11+ the app asks for **"All files access"**. Turn it ON, press back, and your files appear.
Tip: tap = open, long-press = menu, back button = go up, top-right menu = New folder / Paste.

## 4. Common errors
- **Build fails in Actions**: open the failed run, read the red error line (usually a typo in Java).
- **"Parse error" on install**: download the artifact fully and unzip it first.
- **Empty list**: "All files access" permission is not enabled.

## 5. Ideas to improve and sell/offer as a service
Search, sort options, grid view, ZIP/unzip, image thumbnails, dark theme, recycle bin, storage analyzer.
You can reuse this project as a base for freelance Android work (Fiverr/Upwork/Freelancer) or publish it on Google Play (build a signed release AAB first).
