# ASKorsan
Android Studio Port Açıcı Paravan Uygulama Yapımı

Bu Uygulamanın kullanımından hiçbir şekilde sorumluluk kabul etmiyorum, herkes kendi oluşturduğu uygulamadan sorumludur benim amacım
bir uygulama oluşturmayı öğretmektir. Bu uygulama WebViewdir.

Adım 1: Proje Oluşturma
- Android Studio'yu açın ve "File" menüsünden "New" ve ardından "New Project" seçeneğini seçin.
- Projeye bir ad verin ve hedef SDK sürümünü seçin.
- "Empty Activity" şablonunu seçin ve projeyi oluşturun.

Adım 2: WebView Eklemek
- Proje yapısında, "app" klasöründe yer alan "res" klasörüne sağ tıklayın, "New" seçeneğini ve ardından "Directory" seçeneğini seçin. Klasör adını "raw" olarak belirtin.
- "raw" klasörüne sağ tıklayın, "New" seçeneğini ve ardından "Raw Resource File" seçeneğini seçin. Dosya adını "webview.html" olarak belirtin.
- "webview.html" dosyasını açın ve WebView içeriğini düzenleyin. Örneğin, aşağıdaki gibi bir içerik ekleyebilirsiniz:

```html
<!DOCTYPE html>
<html>
<head>
    <title>WebView Uygulaması</title>
</head>
<body>
    <h1>Merhaba Dünya!</h1>
</body>
</html>
```

- "activity_main.xml" dosyasını açın ve aşağıdaki şekilde WebView'i ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>
```

Adım 3: WebView Ayarları
- "MainActivity.java" dosyasını açın ve WebView için gerekli ayarları yapın:

```java
import android.os.Bundle;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        webView = findViewById(R.id.webview);
        webView.setWebViewClient(new WebViewClient());

        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);

        // webview.html dosyasının raw kaynağından yüklenmesi
        webView.loadUrl("file:///android_res/raw/webview.html");
    }
}
```

Adım 4: İzinleri Ekleme
- "AndroidManifest.xml" dosyasını açın ve aşağıdaki izni ekleyin:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Adım 5: FTP İşlemleri
- FTP işlemleri için FTP kütüphanesi kullanmanız gerekecektir. Örneğ

in, `commons-net` kütüphanesini kullanabilirsiniz. Bunun için `build.gradle` dosyanızda aşağıdaki bağımlılığı eklemeniz gerekecektir:

```groovy
implementation 'commons-net:commons-net:3.8.0'
```

- FTP sunucusuna bağlanmak ve sunucu bilgilerini göndermek için aşağıdaki örneği kullanabilirsiniz:

```java
import org.apache.commons.net.ftp.FTP;
import org.apache.commons.net.ftp.FTPClient;

import java.io.IOException;

public class FTPUtil {

    public static void uploadFile(String server, int port, String user, String password, String filePath) {
        FTPClient ftpClient = new FTPClient();
        try {
            ftpClient.connect(server, port);
            ftpClient.login(user, password);
            ftpClient.enterLocalPassiveMode();
            ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

            String remoteFilePath = "path/to/save/file.txt";
            ftpClient.storeFile(remoteFilePath, inputStream); // inputStream yerine göndermek istediğiniz dosya verisini kullanın

            ftpClient.logout();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (ftpClient.isConnected()) {
                try {
                    ftpClient.disconnect();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

- FTP işlemlerini gerçekleştirmek için, ilgili yere aşağıdaki kodu ekleyin:

```java
String server = "ftp.sunucu.com";
int port = 21;
String user = "ftp_kullanici_adi";
String password = "ftp_sifre";
String filePath = "path/to/local/file.txt";

FTPUtil.uploadFile(server, port, user, password, filePath);
```

Bu şekilde, Android Studio'da bir WebView uygulaması oluşturabilir ve FTP sunucusuna sunucu bilgilerini gönderebilirsiniz. Tabii ki, FTP sunucusu bağlantı ayrıntılarınızı (`server`, `port`, `user`, `password`) ve göndermek istediğiniz dosya ayrıntılarını (`filePath`) uygun şekilde güncellemeniz gerektiğini unutmayın.
