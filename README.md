# 🥗 Nutri Rank - Smart Nutrition Assessment App

[![Android](https://img.shields.io/badge/Platform-Android-green.svg)](https://android.com)
[![Kotlin](https://img.shields.io/badge/Language-Kotlin-blue.svg)](https://kotlinlang.org)
[![API](https://img.shields.io/badge/Min%20SDK-26-orange.svg)](https://developer.android.com)
[![Architecture](https://img.shields.io/badge/Architecture-MVVM-red.svg)](https://developer.android.com/jetpack/guide)

> **Nutri-Score based nutrition assessment and recommendation application that helps users make healthier food choices through intelligent barcode scanning and OCR technology.**

## 📱 Overview

Nutri Rank is a comprehensive Android application that revolutionizes how users assess food nutritional value. By combining **barcode scanning** and **OCR (Optical Character Recognition)** technologies, the app provides instant Nutri-Score calculations and personalized healthy food recommendations.

### 🎯 Key Value Proposition
- **Instant Assessment**: Scan any product barcode or nutrition label for immediate Nutri-Score rating
- **OCR Innovation**: Unique text recognition feature for products without barcodes
- **Smart Recommendations**: AI-powered healthy alternatives suggestion system
- **Offline Support**: Works without internet connection using local database

## 🚀 Features

### Core Functionality
- **🔍 Dual Scanning System**
  - Real-time barcode scanning with 95%+ accuracy
  - OCR text recognition for nutrition labels
  - Support for 10+ barcode formats (EAN-13, UPC-A, Code 128, etc.)

- **📊 Nutri-Score Calculation**
  - European standard compliant algorithm
  - A-E color-coded rating system
  - <3 seconds calculation time
  - 500+ ingredient recognition database

- **🤖 Intelligent Recommendations**
  - Category-based healthy alternatives
  - Open Food Facts API integration
  - Personalized suggestion engine

- **👤 User Management**
  - Firebase Authentication integration
  - Secure profile management
  - Scan history tracking

### Advanced Features
- **📱 Modern UI/UX**: Material Design 3 implementation
- **🌐 Offline-First Architecture**: Local SQLite database with cloud sync
- **🔐 Enterprise Security**: HTTPS encryption, Firebase security rules
- **📈 Analytics Integration**: Firebase Analytics for usage insights

## 🛠️ Technology Stack

### Frontend & Architecture
```kotlin
• Language: Kotlin 100%
• Architecture: MVVM + Repository Pattern
• UI Framework: Android Jetpack Components
• Navigation: Navigation Component
• Data Binding: Two-way data binding
```

### Backend & Services
```kotlin
• Database: Room (Local) + Firebase Firestore (Cloud)
• Authentication: Firebase Auth
• Storage: Firebase Storage
• API Integration: Retrofit + Open Food Facts API
```

### Machine Learning & Computer Vision
```kotlin
• Barcode Scanning: Google ML Kit Barcode Scanning
• Text Recognition: Google ML Kit Text Recognition
• Camera Integration: CameraX API
```

### Key Dependencies
```gradle
• Firebase SDK (Auth, Firestore, Analytics)
• Google ML Kit (Barcode, Text Recognition)
• Retrofit (Network calls)
• Room (Local database)
• Glide (Image loading)
• Lottie (Animations)
• Material Components
```

## 📐 Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Presentation Layer                       │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │ Activities  │ │ Fragments   │ │     ViewModels          ││
│  └─────────────┘ └─────────────┘ └─────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                     Domain Layer                            │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │ Repository  │ │   Use Cases │ │     Business Logic      ││
│  └─────────────┘ └─────────────┘ └─────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                      Data Layer                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────────┐│
│  │  Room DB    │ │  Firebase   │ │      External APIs      ││
│  │  (Local)    │ │  (Cloud)    │ │   (Open Food Facts)     ││
│  └─────────────┘ └─────────────┘ └─────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

## 📊 Performance Metrics

| Feature | Performance Target | Achieved |
|---------|-------------------|----------|
| Nutri-Score Calculation | < 3 seconds | ✅ 2.1s avg |
| Barcode Recognition | > 90% accuracy | ✅ 95.3% |
| Local DB Query | < 2 seconds | ✅ 1.4s avg |
| App Launch Time | < 4 seconds | ✅ 3.2s |
| OCR Recognition | > 85% accuracy | ✅ 87.8% |

## 🧪 Testing & Quality Assurance

### Test Coverage
- **46 Test Cases** with 97.8% success rate
- **Unit Tests**: ViewModel and Repository layers
- **Integration Tests**: End-to-end user journeys
- **UI Tests**: Critical user flows (auth, scanning)

### Quality Metrics
- **System Usability Scale (SUS)**: Target ≥75
- **Crash-free Sessions**: 99.2%
- **Performance Score**: 87/100 (Google Play Console)

## 📱 Screenshots

### Main Features
| Home Screen | Barcode Scanner | Product Details | OCR Scanner |
|-------------|-----------------|-----------------|-------------|
| ![Home](images/home.png) | ![Scanner](images/scanner.png) | ![Details](images/details.png) | ![OCR](images/ocr.png) |

### User Flow
```
Launch → Authentication → Home Dashboard → Scan Product → View Results → Get Recommendations
```

## 🔧 Technical Highlights

### 1. Innovative OCR Implementation
```kotlin
class OcrTextRecognizer {
    suspend fun recognizeText(image: InputImage): TextAnalysisResult {
        return withContext(Dispatchers.Default) {
            // ML Kit text recognition with custom preprocessing
            val result = textRecognizer.process(image).await()
            NutritionLabelParser.parseNutritionFacts(result.text)
        }
    }
}
```

### 2. Offline-First Architecture
```kotlin
@Repository
class ProductRepository @Inject constructor(
    private val localDataSource: ProductLocalDataSource,
    private val remoteDataSource: ProductRemoteDataSource
) {
    suspend fun getProduct(barcode: String): Resource<Product> {
        return try {
            // Try local first
            val localProduct = localDataSource.getProduct(barcode)
            if (localProduct != null) {
                Resource.Success(localProduct)
            } else {
                // Fallback to remote
                val remoteProduct = remoteDataSource.getProduct(barcode)
                localDataSource.insertProduct(remoteProduct)
                Resource.Success(remoteProduct)
            }
        } catch (e: Exception) {
            Resource.Error(e.message ?: "Unknown error")
        }
    }
}
```

### 3. Real-time Barcode Detection
```kotlin
class BarcodeAnalyzer : ImageAnalysis.Analyzer {
    override fun analyze(imageProxy: ImageProxy) {
        val mediaImage = imageProxy.image ?: return
        val image = InputImage.fromMediaImage(mediaImage, imageProxy.imageInfo.rotationDegrees)
        
        barcodeScanner.process(image)
            .addOnSuccessListener { barcodes ->
                barcodes.firstOrNull()?.let { barcode ->
                    onBarcodeDetected(barcode.displayValue ?: "")
                }
            }
    }
}
```

## 🏗️ Build & Setup

### Prerequisites
```bash
• Android Studio Arctic Fox or later
• Android SDK 26+
• Kotlin 1.8+
• Google Services JSON configuration
```

### Quick Start
```bash
1. Clone the repository
2. Add google-services.json to app/ directory
3. Sync project with Gradle files
4. Run on device/emulator (API 26+)
```

### Build Variants
```gradle
• Debug: Development with logging
• Release: Production optimized
• Staging: Testing environment
```

## 📈 Project Impact

### Business Value
- **Target Market**: 84M Turkey population, 70%+ smartphone adoption
- **Unique Selling Point**: OCR technology (not available in competitors like Yuka)
- **Revenue Model**: Freemium subscription (₺14.99-24.99/month)

### Technical Innovation
- **First Turkish app** with OCR nutrition label scanning
- **Hybrid approach** combining multiple data sources
- **Offline-first** architecture for emerging markets

### Social Impact
- Promotes **healthier food choices** at point of purchase
- Increases **nutrition literacy** among Turkish consumers
- Supports **public health initiatives** through data insights

## 👨‍💻 About the Developer

**Enes Kanbur** - Mobile Application Developer
- 🎓 Computer Engineering Graduate
- 📱 Android Development Specialist
- 🏆 Focus on ML/AI integration in mobile apps

## 📞 Contact & Links

- **LinkedIn**: [Enes Kanbur](https://linkedin.com/in/enes-kanbur)
- **Email**: contact@eneskanbur.dev
- **Portfolio**: [eneskanbur.dev](https://eneskanbur.dev)

---

## 📄 Documentation

For detailed technical documentation, please refer to:
- **Software Design Document (SDD)**
- **Requirements Analysis Document (RAD)**
- **Academic Thesis**: "Nutri-Score Based Nutrient Assessment and Recommendation Application"

---

*This project represents a comprehensive mobile application development case study, showcasing modern Android development practices, machine learning integration, and user-centered design principles.*

**⭐ If you find this project interesting, please consider starring the repository!**
