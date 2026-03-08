# mktool Android — Projet Gradle

## À propos
Application Android pour déballer et reconditionner les images de démarrage Android.
Portage du projet [mktool v5.4](https://github.com/GameTheory-/mktool) (outil Linux bureau → Android).

---

## ⚙️ Fonctionnalités
| Fonction | Description |
|---|---|
| **Unpack** | Décompresse une image boot/recovery |
| **Repack** | Reconditionne une image modifiée |
| **Image Info** | Affiche les paramètres de l'image |
| **Loki Patch** | Applique le patch loki sur une image |

---

## 🔧 IMPORTANT — Binaires natifs ARM requis

Vous devez placer les **binaires compilés pour ARM/ARM64** dans :

```
app/src/main/assets/tools/
├── mkbootimg          ← ARM64 binary
├── unpackbootimg      ← ARM64 binary
└── loki               ← ARM64 binary
```

### Comment obtenir les binaires ARM :
- **mkbootimg / unpackbootimg** : https://github.com/osm0sis/mkbootimg
  ```bash
  # Compiler pour ARM64 :
  git clone https://github.com/osm0sis/mkbootimg
  cd mkbootimg
  # Cross-compile pour aarch64
  aarch64-linux-gnu-gcc -o mkbootimg mkbootimg.c -static
  aarch64-linux-gnu-gcc -o unpackbootimg unpackbootimg.c -static
  ```
- **loki** : https://github.com/djrbliss/loki
  ```bash
  git clone https://github.com/djrbliss/loki
  cd loki
  aarch64-linux-gnu-gcc -o loki loki_patch.c loki_unlock.c -static
  ```

---

## 🚀 Compilation

### Prérequis
- Android Studio Hedgehog (2023.1.1) ou supérieur
- Android SDK 34
- JDK 17

### Étapes
```bash
# 1. Cloner / décompresser le projet
cd mktool-android

# 2. Placer les binaires ARM dans assets/tools/ (voir ci-dessus)

# 3. Compiler le debug APK
./gradlew assembleDebug

# 4. APK généré dans :
# app/build/outputs/apk/debug/app-debug.apk
```

---

## 📁 Structure du projet

```
mktool-android/
├── app/
│   ├── src/main/
│   │   ├── AndroidManifest.xml
│   │   ├── java/com/mktool/android/
│   │   │   ├── MainActivity.java          ← Écran principal
│   │   │   ├── UnpackActivity.java        ← Décompression
│   │   │   ├── RepackActivity.java        ← Reconditionnement
│   │   │   ├── ImageInfoActivity.java     ← Informations image
│   │   │   ├── LokiActivity.java          ← Patch loki
│   │   │   ├── UnpackRepackUtil.java      ← Logique principale
│   │   │   ├── ShellExecutor.java         ← Exécution shell
│   │   │   ├── NativeToolsManager.java   ← Gestion des binaires
│   │   │   └── FileUtils.java            ← Utilitaires fichiers
│   │   ├── assets/tools/                 ← Binaires ARM (à ajouter)
│   │   └── res/                          ← Ressources UI
│   └── build.gradle
├── build.gradle
├── settings.gradle
└── gradle/wrapper/
    └── gradle-wrapper.properties
```

---

## 📱 Sorties de l'application
Les fichiers extraits/créés sont sauvegardés dans :
```
/sdcard/mktool/
├── extracted/    ← Images décompressées
└── output/       ← Nouvelles images créées
```

---

## 🔒 Permissions requises
- `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE`
- `MANAGE_EXTERNAL_STORAGE` (Android 11+)

---

## Credits
- Basé sur [mktool by GameTheory-](https://github.com/GameTheory-/mktool)
- Outils natifs : [mkbootimg by osm0sis](https://github.com/osm0sis/mkbootimg), [loki by djrbliss](https://github.com/djrbliss/loki)
