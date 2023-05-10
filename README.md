<h2>Flutter ile geliştirilen uygulamayı Play Store'da yayımlamak!</h2>

<a href="https://youtu.be/B7IMMJD8JFY">Videolu Anlatım (Tıkla)</a>

1- Uygulama oluşturalım:
`flutter create --org com.ideapark sayac`

2- Uygulama ikonu için kolayca tasarım oluşturamaya olanak sağlayan web sitesi: https://romannurik.github.io/AndroidAssetStudio/index.html İndirdiğiniz ikon klasörünü `android/app/src/main/res` klasörü ile değiştirin.

3- Uygulama imzalama: `keytool -genkey -v -keystore c:/key.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias key`
İki defa şifre soracak. İkisine de belirlediğiniz aynı şifreyi yazın ve bunu sakın unutmayın.

Bu aşamada `'keytool' is not recognized as an internal or external command, operable program or batch file.` hatası alırsanız, `C:\Program Files (x86)\Java\jre6\bin` konumunu sistem PATH'ine ekleyin. Sistem PATH'ine ulaşmak içinse; Bu Bilgisayar > Özellikler > Gelişmiş Sistem Ayarları > Ortam Değişkenleri > Path yolunu takip edebilirsiniz.

4- `android` klasöründe `key.properties` isimli bir dosya oluşturun ve içerisine şunları yazın;
```properties
storePassword=Uygulama imzalarken sorduğu 
keyPassword=Son sorduğu şifre, eğer direkt enterladıysanız üsttekinin aynısı
keyAlias=key
storeFile=jks dosyasının yolu
```
5- `ndroid/app/build.gradle` dosyasına şu kodları ekleyin;
```
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
```

- Sonrasında ise şu kodu bulun;
``` 
buildTypes {
	release {
       		signingConfig signingConfigs.debug
        }
}
```
- Şununla değiştirin;
```
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
```

Uygulamayı Play Store'da yayımlarken veya sonrasında güncelleme dağıtımı yaparken, `pubspec.yaml` dosyasındaki `version` değerine dikkat etmelisiniz. İlk defa yayımlayacaksanız `1.0.0` kullanabilirsiniz. `1.0` da olur.

Versiyonu da ayarladıktan sonra uygulamayı Play Store'da yüklemek için build edelim. Öncelikle daha önceki buildleri temizleyelim. Bunun için `flutter clean` komutunu kullanmalısınız. Son olaraksa Play Store'a yüklemek için kullanılan appbundle çıktısına ihtiyacım var. Bu, APK'nın farklı bir versiyonu. Bunun içinse  konsola`flutter build appbundle` yazmamız gerekiyor.

İşte bu kadar :)



	
