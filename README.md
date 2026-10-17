[README (1).md](https://github.com/user-attachments/files/33589821/README.1.md)
# SariFresh# SariFresh: 24/7 Basic Needs Ordering App

CC106 – Application Development and Emerging Technologies | Team API Build
St. John Paul II College of Davao – College of Information and Communications Technology (CICT)

**Group:** GROUP 1  |  **Section:** 2555B  |  **Platform:** Android (Kotlin, Android Studio)  |  **minSdk:** 24 (Android 7.0)

---

## 1. App Description

SariFresh is a mobile app for a 24/7 local store. Customers view rice, eggs, and cooking oil with prices, choose quantities, book orders, and request delivery within the store's service area. They can also manage their account, saved addresses, and order history.

**Target users:** Households within the store's delivery area who need basic food products in the morning or evening, when they cannot visit the store in person.

### Features

- Register and log in
- View products and prices, choose quantities, and place orders
- Save and manage delivery addresses
- Check if an address is within the store's delivery area
- See a weather advisory when heavy rain may delay delivery
- View order history

### APIs Used

| # | API | Type | Feature in the App |
|---|-----|------|--------------------|
| 1 | OpenWeatherMap – Current Weather API | REST / Web | Delivery weather advisory |
| 2 | Firebase Authentication + Cloud Firestore | Cloud Service | Login, saved addresses, order history |
| 3 | Google Maps Geocoding API | REST / Web | Delivery area check |

Full details of each API (endpoints, fields used, classes, limitations) are in the **API Documentation Sheet (Annex B)**.

---

## 2. Setup Steps

### Requirements

- Android Studio (latest stable) with the Android SDK
- An Android Virtual Device (AVD) **or** a physical Android phone with USB debugging enabled
- Internet connection (all three APIs return live data)
- Accounts for Firebase, OpenWeatherMap, and Google Cloud

### Steps

1. Clone the repository:
   ```bash
   git clone <repository-link>
   ```
2. Open the project in Android Studio and wait for Gradle sync to finish.
3. Add your API keys to `local.properties` (see Section 3).
4. Add your `google-services.json` file to the `app/` folder (see Section 3).
5. Select an emulator or connected device running Android 7.0 (API 24) or higher.
6. Click **Run**.

To build the debug APK: **Build > Build Bundle(s) / APK(s) > Build APK(s)**.

---

## 3. Adding the API Keys

> **Never commit API keys to GitHub.** `local.properties` and `google-services.json` must stay out of version control (check your `.gitignore`).

### Step 1: Get the keys

| Service | How to get it |
|---------|---------------|
| **OpenWeatherMap** | Create a free account at [openweathermap.org](https://openweathermap.org/) and copy the key from the *API keys* tab. A new key may take a short while to activate. |
| **Google Geocoding** | In [Google Cloud Console](https://console.cloud.google.com/), create a project, enable the **Geocoding API**, create an API key, and restrict it to the Geocoding API only. Billing may need to be enabled. |
| **Firebase** | In the [Firebase Console](https://console.firebase.google.com/), create a project and add an Android app (use this project's package name). Enable **Email/Password** under Authentication and create a **Cloud Firestore** database. Download `google-services.json`. |

### Step 2: Add the keys to `local.properties`

Open `local.properties` in the project root (create it if it does not exist) and add:

```properties
OPENWEATHER_API_KEY=your_openweathermap_key_here
GEOCODING_API_KEY=your_google_geocoding_key_here
```

### Step 3: Expose the keys through BuildConfig

In `app/build.gradle.kts`:

```kotlin
import java.util.Properties

val localProps = Properties().apply {
    val file = rootProject.file("local.properties")
    if (file.exists()) load(file.inputStream())
}

android {
    buildFeatures { buildConfig = true }
    defaultConfig {
        buildConfigField("String", "OPENWEATHER_API_KEY",
            "\"${localProps.getProperty("OPENWEATHER_API_KEY", "")}\"")
        buildConfigField("String", "GEOCODING_API_KEY",
            "\"${localProps.getProperty("GEOCODING_API_KEY", "")}\"")
    }
}
```

In code, use `BuildConfig.OPENWEATHER_API_KEY` and `BuildConfig.GEOCODING_API_KEY`.

### Step 4: Add `google-services.json`

Place the file you downloaded from Firebase in the `app/` folder: `app/google-services.json`. Then sync Gradle.

### Firestore security rules

Set the rules so each signed-in user can only read and write their own addresses and orders.

---

## 4. Screenshots

At least two per API: a successful result and an error or empty state.

|              API            |                 Successful Result              |               Error / Empty State          |
|-----------------------------|------------------------------------------------|--------------------------------------------|
| **API 1: OpenWeatherMap**   | ![API 1 success](screenshots/api1_success.png) | ![API 1 error](screenshots/api1_error.png) |
| **API 2: Firebase**         | ![API 2 success](screenshots/api2_success.png) | ![API 2 error](screenshots/api2_error.png) |
| **API 3: Google Geocoding** | ![API 3 success](screenshots/api3_success.png) | ![API 3 error](screenshots/api3_error.png) |

---

## 5. Team Members

|       Member          |      Role       |                   API / Feature Handled                  |  
|-----------------------|-----------------|----------------------------------------------------------|
| Abogho, Cassandra     | API Developer   | API 3: Geocoding (delivery area check)                   |
| Dinampo, Jhon Dave    | Presentation    | API 1: OpenWeatherMap (weather advisory)                 |
| Labarento, John Henry | UI/UX Developer | Map and delivery-area screen (API 3)                     |
| Pabe, Erwina          | Documentation   | Annex A/B, README, API 1 testing and screenshots         |
| Sejalbo, Ralph Angelo | Presentation    | API 2: Firebase Authentication + Firestore               |
| Serdan, Alvin         | UI/UX Developer | Saved-address screens and weather banner (APIs 1 and 2)  |
| Telmoso, Charish Jane | Documentation   | API 2 and 3 testing, screenshots, APK build              |

---

## 6. Citations

Code adapted from tutorials, documentation, or AI tools is listed here. The member who used it can explain and modify it.

| Used In (file / class)      | Source (tutorial, documentation, or AI tool) | Member   |
|-----------------------------|----------------------------------------------|--------  |
| `<e.g., WeatherApi.kt>`     | `<e.g., Retrofit documentation, URL>`        | `<name>` |
| `<e.g., AuthRepository.kt>` | `<e.g., Firebase Authentication docs, URL>`  | `<name>` |

We certify that this project is the original work of our group and that all borrowed code and resources are cited above.
