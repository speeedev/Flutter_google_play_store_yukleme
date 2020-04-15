Flutter Uygulamasını Google Playde Yayınlamak

1- flutter create --org com.ideapark sayac

   // com.ideapark.sayac

2- launch icon android/app/src/main/res klasörü

   https://romannurik.github.io/AndroidAssetStudio/index.html

3- Uygulamamızı imzalayalım

   Key oluşturma
   
	keytool -genkey -v -keystore c:/key.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias key
	Bu şekilde bir terminal koduyla, kod içerisinde belirtilen yola keystore (.jks) dosyamız oluşturulacak.

4- Key dosyamızı oluşturduk ya sonra (key.properties)

   android/key.properties
	storePassword=İlk sorduğu şifre //123456A+
	keyPassword=Son sorduğu şifre, eğer direkt enterladıysanız üsttekinin aynısı
	//123456A+
	keyAlias=key
	storeFile=jks dosyamızın yolu, bende bu şekilde
	// storeFile=C:\\Users\\mksular\\Desktop/key.jks

5- Build.gradle düzenleme

   android > app > build.gradle
	def localProperties = new Properties()
	def localPropertiesFile = rootProject.file('local.properties')
	if (localPropertiesFile.exists()) {
    	localPropertiesFile.withReader('UTF-8') { reader ->
        localProperties.load(reader)
    	}
	}
	def keystoreProperties = new Properties()
	def keystorePropertiesFile = rootProject.file('key.properties')
	if (keystorePropertiesFile.exists()) {
    	keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
	}

    Sonra bu kodu bulup
	buildTypes {
       	release {
           // TODO: Add your own signing config for the release build.
           // Signing with the debug keys for now,
           // so `flutter run --release` works.
           signingConfig signingConfigs.debug
       	}
   	}

    Bu şekilde değiştiriyoruz
	signingConfigs {
       	release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       	}
   	}
   	buildTypes {
       	release {
           signingConfig signingConfigs.release
       	}
   	}


Uygulama versiyonunu kontrol etme

pubspec.yaml

version: 1.0.1

flutter clean,

flutter build apk veya flutter build appbundle




	
