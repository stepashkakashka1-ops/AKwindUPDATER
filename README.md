<p align="center">
  <alt="AKwind" width="120" />
</p>

<h1 align="center">AKwind Updater</h1>

<p align="center">
  <strong>Канал обновлений для Android-приложения AKwind</strong><br/>
  <em>Update channel for the AKwind wind tracker</em>
</p>

<p align="center">
  <a href="https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases/latest">
    <img src="https://img.shields.io/github/v/release/stepashkakashka1-ops/AKwindUPDATER?style=for-the-badge&color=38bdf8&label=latest" alt="Latest release" />
  </a>
  <img src="https://img.shields.io/badge/platform-Android%2012%2B-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android 12+" />
  <img src="https://img.shields.io/badge/status-beta-f59e0b?style=for-the-badge" alt="Beta" />
  <a href="https://github.com/stepashkakashka1-ops/AKwindUPDATER/blob/main/update.json">
    <img src="https://img.shields.io/badge/manifest-update.json-8b5cf6?style=for-the-badge" alt="update.json" />
  </a>
</p>

<p align="center">
  <a href="#-что-это">Что это</a> ·
  <a href="#-скачать">Скачать</a> ·
  <a href="#-как-работают-обновления">Как работают обновления</a> ·
  <a href="#-формат-updatejson">Формат update.json</a> ·
  <a href="#-как-выложить-новую-версию">Выложить версию</a>
</p>

---

## 🌊 Что это

Этот репозиторий — **не исходники приложения**, а **официальный канал раздачи APK** для [AKwind](https://github.com/Yugugyiugiyguiygugyyg/AKwind).

| Файл / раздел | Назначение |
| --- | --- |
| [`update.json`](./update.json) | Манифест последней версии (его читает приложение) |
| [Releases](https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases) | Сборки APK с прямыми ссылками для скачивания |
| Этот README | Документация для пользователей и мейнтейнера |

AKwind — трекер ветра для кайтсёрфинга: живые данные, история, прогноз, AI-рекомендации, live-уведомления и виджет.

---

## ⬇️ Скачать

### Последняя версия

| | |
| --- | --- |
| **Версия** | `0.1.3 BETA` |
| **versionCode** | `7` |
| **APK** | [AKwind-0.1.3.apk](https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases/download/v0.1.3/AKwind-0.1.3.apk) |
| **Все релизы** | [Releases →](https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases) |

> **Android 12+ (API 31).** При установке APK вне Google Play система попросит разрешить установку из неизвестного источника — это нормально.

### Обновление из приложения

Если AKwind уже установлен:

1. Откройте **Настройки → О приложении**
2. Нажмите **Проверить обновления**
3. Дождитесь загрузки и подтвердите установку в системном установщике

---

## 🔄 Как работают обновления

```text
┌─────────────────┐     1. GET update.json      ┌──────────────────────────┐
│  AKwind (app)   │ ──────────────────────────► │  raw.githubusercontent   │
│                 │ ◄────────────────────────── │  .../main/update.json    │
└────────┬────────┘     2. versionCode / notes  └──────────────────────────┘
         │
         │  3. если versionCode больше локального
         ▼
┌─────────────────┐     4. скачать APK          ┌──────────────────────────┐
│  AppUpdateMgr   │ ──────────────────────────► │  GitHub Releases (apk)   │
│                 │     5. optional SHA-256     │                          │
└────────┬────────┘                             └──────────────────────────┘
         │
         ▼  6. системный установщик Android
```

1. Приложение запрашивает манифест по URL (вшит в сборку).
2. Сравнивает `versionCode` с установленной версией.
3. Если есть новее — показывает changelog (`notes`) и качает APK.
4. При указанном `sha256` проверяет целостность файла.
5. Открывает стандартный установщик Android (пользователь подтверждает установку).

**Манифест (raw):**

```text
https://raw.githubusercontent.com/stepashkakashka1-ops/AKwindUPDATER/main/update.json
```

---

## 📄 Формат `update.json`

```json
{
  "versionCode": 7,
  "versionName": "0.1.3 BETA",
  "apkUrl": "https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases/download/v0.1.3/AKwind-0.1.3.apk",
  "sha256": "",
  "mandatory": false,
  "notes": {
    "ru": "Исправления и улучшения.",
    "en": "Fixes and improvements.",
    "tr": "Düzeltmeler ve iyileştirmeler."
  }
}
```

| Поле | Тип | Описание |
| --- | --- | --- |
| `versionCode` | `int` | **Обязательно.** Целое; должно быть **строго больше**, чем у установленного APK |
| `versionName` | `string` | Человекочитаемая версия (`0.1.3 BETA`) |
| `apkUrl` | `string` | Прямая HTTPS-ссылка на `.apk` (обычно GitHub Release asset) |
| `sha256` | `string` | Опционально. SHA-256 хеш APK; пустая строка = без проверки |
| `mandatory` | `bool` | `true` — обновление помечено как обязательное |
| `notes` | `object` | Changelog по языкам: `ru`, `en`, `tr` (приложение берёт язык UI, иначе `en`) |

---

## 🚀 Как выложить новую версию

> Для мейнтейнера. Исходники приложения — в репозитории [AKwind](https://github.com/Yugugyiugiyguiygugyyg/AKwind).

### 1. Собрать APK

В репозитории приложения поднимите версии в `app/build.gradle.kts`:

```kotlin
versionCode = 8          // +1 к предыдущему
versionName = "0.1.4 BETA"
```

Сборка:

```powershell
java -jar gradle\wrapper\gradle-wrapper.jar :app:assembleRelease
```

APK обычно лежит в:

```text
app/build/outputs/apk/release/
```

### 2. (Рекомендуется) Посчитать SHA-256

```powershell
Get-FileHash -Algorithm SHA256 .\AKwind-0.1.4.apk
```

### 3. Создать GitHub Release

1. Откройте [Releases → New release](https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases/new)
2. Tag: `v0.1.4` (как в `versionName`)
3. Прикрепите APK, например `AKwind-0.1.4.apk`
4. Опубликуйте release
5. Скопируйте **прямую** ссылку на asset (кнопка Download у файла)

### 4. Обновить `update.json` в `main`

```json
{
  "versionCode": 8,
  "versionName": "0.1.4 BETA",
  "apkUrl": "https://github.com/stepashkakashka1-ops/AKwindUPDATER/releases/download/v0.1.4/AKwind-0.1.4.apk",
  "sha256": "вставьте_хеш_или_оставьте_пустым",
  "mandatory": false,
  "notes": {
    "ru": "Кратко, что нового",
    "en": "What's new",
    "tr": "Yenilikler"
  }
}
```

Закоммитьте и запушьте в ветку **`main`** — приложение читает именно raw-файл из `main`.

### 5. Проверить

1. Откройте raw-URL `update.json` в браузере — JSON должен быть валидным.
2. В AKwind: **Настройки → О приложении → Проверить обновления**.
3. Убедитесь, что `versionCode` в манифесте **больше**, чем у установленной сборки.

---

## 🔐 Безопасность и подпись

- Android **не ставит APK «тихо»**: приложение только скачивает файл и открывает системный установщик; установку подтверждает пользователь.
- Для обновления «поверх» предыдущей версии APK должен быть подписан **тем же ключом**, что и уже установленное приложение.
- Поле `sha256` защищает от повреждённой или подменённой загрузки (если хеш заполнен).
- URL манифеста зашит в `BuildConfig.UPDATE_MANIFEST_URL` сборки приложения.

---

## 🧩 Связанные репозитории

| Репозиторий | Роль |
| --- | --- |
| [Yugugyiugiyguiygugyyg/AKwind](https://github.com/Yugugyiugiyguiygugyyg/AKwind) | Исходный код приложения |
| **stepashkakashka1-ops/AKwindUPDATER** (этот) | Манифест + APK-релизы |

---

## 📱 AKwind в двух словах

- 💨 Текущий ветер, порывы, направление, температура  
- 📈 История и графики, статистика спота, wind rose  
- 🗓 Прогноз (Open-Meteo) и AI-прогноз (OpenRouter)  
- 🔔 Live-уведомления, когда условия подходят для катания  
- 🧩 Виджет на рабочий стол  
- 🌍 Языки: русский, English, Türkçe  

---

<p align="center">
  <sub>Сделано для ловли ветра · Catch the wind</sub><br/>
  <sub>© AKwind · Beta</sub>
</p>
