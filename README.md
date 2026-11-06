# AutoCaption

Free, offline automatic captions for videos (Android, Kotlin + Jetpack Compose).

Flow: pick video from gallery -> audio extracted (MediaCodec) -> Whisper (whisper.cpp, runs on the phone)
-> word-level timestamps -> live captions on the video + `.srt` / `.vtt` export.

* All 99 Whisper languages (Urdu, Hindi, English, Arabic, Punjabi ... or Auto-detect)
* Caption styles: font, size, color, bold, background box, outline, position, CapCut-style word highlight
* Edit caption text before exporting, share the file or save it in Downloads
* No API key, no server, no ads. Internet is needed only once to download the model (Base ~142 MB / Small ~466 MB)

## APK banane ka asaan tareeqa (Android Studio ke baghair)

1. github.com par naya (free) repository banao.
2. Is poore folder ki saari files repository mein upload karo (`.github` folder samet).
3. Repository mein **Actions** tab -> **Build APK** -> agar khud na chale to **Run workflow**.
4. 10-20 minute baad build khatam hone par run kholo, neeche **Artifacts** se `AutoCaption-debug-apk` download karo.
5. Zip kholo, `app-debug.apk` phone mein install karo (Unknown sources ki ijazat do).

## Android Studio se

1. Android Studio (Koala ya naya) mein folder open karo. Pehli baar SDK 34, NDK 26.1.10909125 aur CMake 3.22.1 install karne ko kahega.
2. whisper.cpp khud download hota hai (git + internet chahiye). Ya khud: `git clone --depth 1 --branch v1.8.7 https://github.com/ggml-org/whisper.cpp app/src/main/cpp/whisper.cpp`
3. Run karo (real phone par; emulator par bohat slow hai). Sirf `arm64-v8a` phones ke liye build hota hai.

## Tips

* Urdu / Hindi ke liye **Small** model chuno, aur language manually set karo (Auto-detect se behtar).
* Lambi video (30 min+) kam RAM wale phone par memory ke masle de sakti hai; chhoti clips / reels best hain.

## Project layout

```
app/src/main/cpp/            CMakeLists.txt + jni.c  (bridge to whisper.cpp v1.8.7)
app/src/main/java/com/whispercpp/whisper/WhisperLib.kt   native declarations
app/src/main/java/com/autocaption/app/
  audio/AudioExtractor.kt    video -> 16 kHz mono PCM
  model/ModelManager.kt      downloads ggml models (Hugging Face)
  whisper/WhisperEngine.kt   runs whisper on its own thread
  caption/                   words -> caption lines, styles, languages, SRT/VTT writer
  export/FileExporter.kt     save to Downloads / share
  ui/                        Compose screens + ViewModel
```
