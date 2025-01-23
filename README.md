# 1 -

### Örnek Adı:  
**LED Yakıp Söndürme (Buton ile - Pull-Up)**

---

### Örnek Açıklama:  
Bu projede, bir LED'i bir buton yardımıyla yakıp söndüreceğiz. Bu versiyonda **Pull-Up Direnç** kullanacağız. Arduino'nun dahili pull-up dirençlerini aktif hale getirerek, butonun lojik durumunu stabilize edeceğiz. **Buton basılmadığında HIGH, basıldığında LOW** sinyal alınır.

---

### Örnek Bağlantılar:  
1. **LED Anotu (Uzun bacak)** → Arduino **D9**  
2. **LED Katotu (Kısa bacak)** → Arduino **GND**  
3. **Butonun bir bacağı** → Arduino **D7**  
4. **Butonun diğer bacağı** → Arduino **GND**  

---

### Örnek Kodu:  
```cpp
// Pin Tanımlamaları
const int ledPin = 9;  // LED bağlı olduğu pin
const int buttonPin = 7;  // Buton bağlı olduğu pin
int buttonState = 0;  // Buton durumu

void setup() {
  pinMode(ledPin, OUTPUT);        // LED çıkış olarak ayarlanıyor
  pinMode(buttonPin, INPUT_PULLUP); // Buton için dahili pull-up aktif
}

void loop() {
  buttonState = digitalRead(buttonPin); // Buton durumu okunuyor

  if (buttonState == LOW) { // Buton basılıysa (LOW)
    digitalWrite(ledPin, HIGH); // LED yanar
  } else {
    digitalWrite(ledPin, LOW); // LED söner
  }
}
```


## 2 -


### Örnek Adı:  
**LED Yakıp Söndürme (Buton ile - Input)**

---

### Örnek Açıklama:  
Bu projede yine bir LED'i bir buton yardımıyla yakıp söndüreceğiz. Ancak bu defa buton, **input** olarak doğrudan kullanılacak. Harici bir **pull-down direnç** kullanılarak buton lojik durumunun kararlı hale gelmesi sağlanacak. **Buton basılmadığında LOW, basıldığında HIGH** sinyal alınır.

---

### Örnek Bağlantılar:  
1. **LED Anotu (Uzun bacak)** → Arduino **D9**  
2. **LED Katotu (Kısa bacak)** → Arduino **GND**  
3. **Butonun bir bacağı** → Arduino **D7**  
4. **Butonun diğer bacağı** → Arduino **5V**  
5. **10kΩ Pull-Down Direnç:**  
   - Bir ucu **butonun D7 ile bağlandığı bacak**  
   - Diğer ucu **GND**

---

### Örnek Kodu:  
```cpp
// Pin Tanımlamaları
const int ledPin = 9;  // LED bağlı olduğu pin
const int buttonPin = 7;  // Buton bağlı olduğu pin
int buttonState = 0;  // Buton durumu

void setup() {
  pinMode(ledPin, OUTPUT);   // LED çıkış olarak ayarlanıyor
  pinMode(buttonPin, INPUT); // Buton giriş olarak ayarlanıyor
}

void loop() {
  buttonState = digitalRead(buttonPin); // Buton durumu okunuyor

  if (buttonState == HIGH) { // Buton basılıysa (HIGH)
    digitalWrite(ledPin, HIGH); // LED yanar
  } else {
    digitalWrite(ledPin, LOW); // LED söner
  }
}
```

## 3 -

### Örnek Adı:  
**RGB LED ile Renk Değiştirme (Potansiyometre ile)**

---

### Örnek Açıklama:  
Bu projede bir RGB LED'in renklerini bir potansiyometre yardımıyla değiştireceğiz. Potansiyometrenin analog çıkışını okuyarak RGB LED'in kırmızı, yeşil ve mavi renk bileşenlerini kontrol edeceğiz. **Potansiyometre 0-1023 arasında değer üretir** ve bu değer `map()` fonksiyonu ile 0-255 arasında LED parlaklık değerine dönüştürülür.

---

### Örnek Bağlantılar:  
1. **RGB LED Bağlantıları:**  
   - **Kırmızı bacak (R)** → Arduino **D9**  
   - **Yeşil bacak (G)** → Arduino **D10**  
   - **Mavi bacak (B)** → Arduino **D11**  
   - **Ortak Katot (GND)** → Arduino **GND**  
2. **Potansiyometre Bağlantıları:**  
   - Orta pin (çıkış) → Arduino **A0**  
   - Sağ pin → Arduino **5V**  
   - Sol pin → Arduino **GND**

---

### Örnek Kodu:  
```cpp
// Pin Tanımlamaları
const int redPin = 9;    // Kırmızı LED pin
const int greenPin = 11; // Yeşil LED pin
const int bluePin = 10;  // Mavi LED pin
const int potPin = A0;   // Potansiyometre bağlı olduğu pin

void setup() {
  pinMode(redPin, OUTPUT);   // Kırmızı LED çıkış
  pinMode(greenPin, OUTPUT); // Yeşil LED çıkış
  pinMode(bluePin, OUTPUT);  // Mavi LED çıkış
}

void loop() {
  int potValue = analogRead(potPin); // Potansiyometreden okunan değer (0-1023)

  // Potansiyometre değerini LED renklerine bölüştürme
  int redValue;
  int greenValue;
  int blueValue;

  if (potValue <= 409) { // 0-409 aralığı (%40)
    redValue = map(potValue, 0, 409, 0, 255); // Kırmızı parlaklık
    greenValue = 0;
    blueValue = 0;
  } else if (potValue <= 716) { // 409-716 aralığı (%40-%70)
    redValue = 0;
    greenValue = map(potValue, 409, 716, 0, 255); // Yeşil parlaklık
    blueValue = 0;
  } else { // 716-1023 aralığı (%70-%100)
    redValue = 0;
    greenValue = 0;
    blueValue = map(potValue, 716, 1023, 0, 255); // Mavi parlaklık
  }

  // RGB LED renk ayarları
  analogWrite(redPin, redValue);
  analogWrite(greenPin, greenValue);
  analogWrite(bluePin, blueValue);

  delay(10); // Stabilite için kısa gecikme
}

```

## 4 -

### Örnek Adı:  
**RGB LED ile Renk Değiştirme (3 Potansiyometre ile)**

---

### Örnek Açıklama:  
Bu projede bir RGB LED'in renklerini **3 potansiyometre** yardımıyla değiştireceğiz. Her bir potansiyometre, RGB LED'in bir rengini kontrol edecektir. Potansiyometrelerin analog çıkışları, RGB LED'in kırmızı, yeşil ve mavi renk bileşenlerine karşılık gelen PWM sinyallerini oluşturacaktır.

**Potansiyometre Kullanım Mantığı:**  
- Her potansiyometre **0-1023** arasında değer üretir. Bu değerler, `map()` fonksiyonu ile **0-255** arasına dönüştürülerek RGB LED'e uygulanır.

---

### Örnek Bağlantılar:  
1. **RGB LED Bağlantıları:**  
   - **Kırmızı bacak (R)** → Arduino **D9**  
   - **Yeşil bacak (G)** → Arduino **D10**  
   - **Mavi bacak (B)** → Arduino **D11**  
   - **Ortak Katot (GND)** → Arduino **GND**  
2. **Potansiyometre Bağlantıları:**  
   - **Potansiyometre 1 (Kırmızı)**:  
     - Orta pin → Arduino **A0**  
     - Sağ pin → Arduino **5V**  
     - Sol pin → Arduino **GND**  
   - **Potansiyometre 2 (Yeşil):**  
     - Orta pin → Arduino **A1**  
     - Sağ pin → Arduino **5V**  
     - Sol pin → Arduino **GND**  
   - **Potansiyometre 3 (Mavi):**  
     - Orta pin → Arduino **A2**  
     - Sağ pin → Arduino **5V**  
     - Sol pin → Arduino **GND**

---

### Örnek Kodu:  
```cpp
// Pin Tanımlamaları
const int redPin = 9;    // Kırmızı LED pin
const int greenPin = 10; // Yeşil LED pin
const int bluePin = 11;  // Mavi LED pin
const int potRedPin = A0;    // Kırmızı için potansiyometre
const int potGreenPin = A1;  // Yeşil için potansiyometre
const int potBluePin = A2;   // Mavi için potansiyometre

void setup() {
  pinMode(redPin, OUTPUT);   // Kırmızı LED çıkış
  pinMode(greenPin, OUTPUT); // Yeşil LED çıkış
  pinMode(bluePin, OUTPUT);  // Mavi LED çıkış
}

void loop() {
  int redValue = analogRead(potRedPin);    // Kırmızı potansiyometresinden okunan değer
  int greenValue = analogRead(potGreenPin); // Yeşil potansiyometresinden okunan değer
  int blueValue = analogRead(potBluePin);   // Mavi potansiyometresinden okunan değer

  // Potansiyometre değerlerini 0-255 aralığına dönüştürme
  redValue = map(redValue, 0, 1023, 0, 255);  
  greenValue = map(greenValue, 0, 1023, 0, 255);  
  blueValue = map(blueValue, 0, 1023, 0, 255);  

  // RGB LED renk ayarları
  analogWrite(redPin, redValue);
  analogWrite(greenPin, greenValue);
  analogWrite(bluePin, blueValue);

  delay(10); // Stabilite için kısa gecikme
}
```


## 5 -

### Örnek Adı:  
**Buzzer ile Ses Çıkışı (Buton Kontrolü)**

---

### Örnek Açıklama:  
Bu projede bir buton kullanarak **buzzer** (zırıl ses çıkaran eleman) ile ses çıkışı elde edeceğiz. Butona her basıldığında buzzer farklı frekansta ses çıkaracak. Buton basılı değilken ses çıkmayacak. Bu örnek, butonun **input** olarak kullanılmasıyla ses çıkışı sağlayacak.

---

### Örnek Bağlantılar:  
1. **Buzzer Bağlantıları:**  
   - **Buzzerın + bacağı** → Arduino **D8**  
   - **Buzzerın - bacağı** → Arduino **GND**  
2. **Buton Bağlantıları:**  
   - Butonun bir bacağı → Arduino **D7**  
   - Butonun diğer bacağı → Arduino **5V**  
3. **10kΩ Pull-Down Direnç:**  
   - Bir ucu **butonun D7 ile bağlandığı bacak**  
   - Diğer ucu **GND**

---

### Örnek Kodu:  
```cpp
// Pin Tanımlamaları
const int buzzerPin = 8;    // Buzzer bağlı olduğu pin
const int buttonPin = 7;    // Buton bağlı olduğu pin
int buttonState = 0;        // Buton durumu

void setup() {
  pinMode(buzzerPin, OUTPUT);  // Buzzer çıkış olarak ayarlanıyor
  pinMode(buttonPin, INPUT);   // Buton giriş olarak ayarlanıyor
}

void loop() {
  buttonState = digitalRead(buttonPin); // Buton durumu okunuyor

  if (buttonState == HIGH) { // Buton basılıysa (HIGH)
    tone(buzzerPin, 1000);  // Buzzer ile 1000 Hz frekansında ses çıkart
  } else {
    noTone(buzzerPin);      // Buton serbestse ses çıkmaz
  }
}
```

## 6 -

### Örnek Adı:  
**LCD Ekran Kullanımı (16x2 - 2 Kablolu Bağlantı)**

---

### Örnek Açıklama:  
Bu projede, **16x2 LCD ekranı** doğrudan bağlantı yaparak kullanacağız. LCD ekran üzerinde basit bir metin göstereceğiz. Bu ekran, genellikle **6 pin** kullanır ve **daha fazla bağlantı** gerektirir. LCD'yi doğru bağlamak için **güç, veri ve kontrol hatlarını** kullanacağız.

---

### Örnek Bağlantılar:  
1. **16x2 LCD Bağlantıları:**  
   - **VSS (GND)** → Arduino **GND**  
   - **VCC (5V)** → Arduino **5V**  
   - **V0 (Kontrast Ayarı)** → Potansiyometre ile GND  
   - **RS** → Arduino **D12**  
   - **E** → Arduino **D11**  
   - **D4** → Arduino **D5**  
   - **D5** → Arduino **D4**  
   - **D6** → Arduino **D3**  
   - **D7** → Arduino **D2**  
   - **RW** → **GND** (Bu bağlantı genellikle okuma işlevi için kullanılır, biz yazma yapacağımız için bu bacak doğrudan GND'ye bağlanır)

---

### Örnek Kodu:  
```cpp
#include <LiquidCrystal.h>  // LCD kütüphanesini dahil et

// LCD pin tanımlamaları (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  lcd.begin(16, 2);  // LCD'yi başlat (16 sütun, 2 satır)
  lcd.print("Hello, World!");  // LCD ekranda mesaj yazdır
}

void loop() {
  // Sürekli olarak yapılacak bir işlem olmadığı için burası boş
}
```

---

### Açıklama:
- **`LiquidCrystal` kütüphanesi**, LCD ekranla iletişim kurmamızı sağlar.
- **Pin Tanımlamaları:** LCD'nin RS, E, D4, D5, D6, D7 pinleri, Arduino'nun dijital pinlerine bağlanır. Bu şekilde, LCD'yi kontrol edebiliriz.
- **Kontrast Ayarı:** LCD ekranın kontrastını ayarlamak için **potansiyometre** kullanabilirsiniz.

Bu örnekte **"Hello, World!"** mesajı LCD ekranında görünecektir. Bağlantı doğru yapıldığında ekrandaki metni görebilirsiniz.



## 7 -



### Örnek Adı:  
**LCD Ekranda Animasyonlar Yapma**

---

### Örnek Açıklama:  
Bu projede, 16x2 LCD ekranda basit bir animasyon yapacağız. Ekranda bir metnin sola kaymasını sağlayacağız. Bu işlem için **`lcd.scrollDisplayLeft()`** fonksiyonunu kullanacağız, böylece yazı ekranın sağ tarafından başlayarak sola doğru kayacaktır. LCD ekran üzerinde hareketli bir animasyon oluşturacağız.

---

### Örnek Bağlantılar:  
1. **16x2 LCD Bağlantıları:**  
   - **VSS (GND)** → Arduino **GND**  
   - **VCC (5V)** → Arduino **5V**  
   - **V0 (Kontrast Ayarı)** → Potansiyometre ile GND  
   - **RS** → Arduino **D12**  
   - **E** → Arduino **D11**  
   - **D4** → Arduino **D5**  
   - **D5** → Arduino **D4**  
   - **D6** → Arduino **D3**  
   - **D7** → Arduino **D2**  
   - **RW** → **GND** (Bu bağlantı genellikle okuma işlevi için kullanılır, biz yazma yapacağımız için bu bacak doğrudan GND'ye bağlanır)

---

### Örnek Kodu:  
```cpp
#include <LiquidCrystal.h>  // LCD kütüphanesini dahil et

// LCD pin tanımlamaları (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  lcd.begin(16, 2);  // LCD'yi başlat (16 sütun, 2 satır)
  lcd.print("Arduino Animation");    // LCD'ye yazıyı yazdır

}

void loop() {

  delay(1000); // Bir saniye bekle

  // Ekranda yazıyı sola kaydır
  for (int i = 0; i < 16; i++) {

    lcd.scrollDisplayLeft(); // Yazıyı sola kaydır
    delay(200);  // Her kaydırma arasına 200 ms gecikme ekle
  }

   for (int i = 16; i > 0; i--) {

    lcd.scrollDisplayRight(); // Yazıyı sağa kaydır
    delay(200);  // Her kaydırma arasına 200 ms gecikme ekle
  }
}

```

---

### Açıklama:  
1. **Ekrana Yazı Yazma:**  
   İlk olarak ekrana **"Arduino Animation"** yazısını yazıyoruz. Bu yazıyı biraz beklettikten sonra kaydırmaya başlıyoruz.

2. **`scrollDisplayLeft()` Fonksiyonu:**  
   Bu fonksiyon, ekranda yazılı olan tüm içeriği sola kaydırarak animasyon efekti yaratır.

3. **`scrollDisplayRight()` Fonksiyonu:**  
      Bu fonksiyon, ekranda yazılı olan tüm içeriği sağa kaydırarak animasyon efekti yaratır.   

3. **Delay:**  
   Kaydırma işlemi arasında kısa bir gecikme ekliyoruz ki animasyon net bir şekilde görünsün.

---

Bu örnekte LCD ekran üzerinde **"Arduino Animation"** metni sağa ve sola doğru kayacak. Bu basit animasyonu daha karmaşık hale getirerek başka metinler veya efektler de ekleyebilirsiniz.



## 8 -

### Örnek Adı:  
**LCD Ekranda Özel Karakterler Çizme (Kalp, Yıldız, vb.)**

---

### Örnek Açıklama:  
Bu projede, 16x2 LCD ekran üzerinde **özel karakterler** çizeceğiz. LCD ekranlarda **8x5 boyutlarında özel karakterler** tasarlayabiliriz. Bu özel karakterleri belleğe yükleyip, ekran üzerinde farklı semboller (kalp, yıldız gibi) göstereceğiz.

**Özel Karakterler:**  
LCD ekranlarda özel karakterler, 8x5 piksel grid üzerinde şekiller oluşturmak için **`createChar()`** fonksiyonu kullanılarak tanımlanır. Bu fonksiyon sayesinde LCD'de genellikle kullanılmayan karakterlerin yerine istediğimiz semboller kullanılabilir.

---

### Örnek Bağlantılar:  
1. **16x2 LCD Bağlantıları:**  
   - **VSS (GND)** → Arduino **GND**  
   - **VCC (5V)** → Arduino **5V**  
   - **V0 (Kontrast Ayarı)** → Potansiyometre ile GND  
   - **RS** → Arduino **D12**  
   - **E** → Arduino **D11**  
   - **D4** → Arduino **D5**  
   - **D5** → Arduino **D4**  
   - **D6** → Arduino **D3**  
   - **D7** → Arduino **D2**  
   - **RW** → **GND** (Bu bağlantı genellikle okuma işlevi için kullanılır, biz yazma yapacağımız için bu bacak doğrudan GND'ye bağlanır)

---

### Örnek Kodu:  
```cpp
#include <LiquidCrystal.h>  // LCD kütüphanesini dahil et

// LCD pin tanımlamaları (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Özel karakterler için diziler (8x5 boyutunda)
byte heart[8] = {
  B00000,
  B01010,
  B11111,
  B11111,
  B11111,
  B01110,
  B00100,
  B00000
};

byte star[8] = {
  B00100,
  B01110,
  B11111,
  B01110,
  B00100,
  B00000,
  B00000,
  B00000
};

void setup() {
  lcd.begin(16, 2);  // LCD'yi başlat (16 sütun, 2 satır)

  // Özel karakterleri oluştur
  lcd.createChar(0, heart);  // Kalp karakterini 0 numaralı char olarak oluştur
  lcd.createChar(1, star);   // Yıldız karakterini 1 numaralı char olarak oluştur

  lcd.clear();  // Ekranı temizle
  lcd.setCursor(0, 0); // İlk satıra yerleştir
  lcd.print("Kalp ve Yildiz");  // Ekranda yazı göster

  // LCD ekrana özel karakterler yazdır
  lcd.setCursor(0, 1);  // İkinci satıra geç
  lcd.write(byte(0));   // Kalp sembolünü göster
  lcd.write(byte(1));   // Yıldız sembolünü göster
}

void loop() {
  // Sürekli yapılacak bir işlem olmadığı için boş bırakılıyor
}
```

---

### Açıklama:
1. **`createChar()` Fonksiyonu:**  
   LCD'de özel karakterler oluşturmak için **`createChar()`** fonksiyonu kullanılır. Bu fonksiyon, 8x5'lik bir piksel matrisini alır ve o matrisi karakter olarak ekler.

2. **Özel Karakterler Tanımlaması:**  
   - **Kalp (Heart):** 8 satırdan oluşan bir matris ile kalp şekli oluşturulmuş.  
   - **Yıldız (Star):** 8 satırdan oluşan bir matris ile yıldız şekli oluşturulmuş.

3. **`lcd.write(byte(x))` Fonksiyonu:**  
   Özel karakterleri ekranda göstermek için **`write()`** fonksiyonu kullanılır. Burada, özel karakterleri **0 ve 1** numaralı karakterler olarak ekrana yazdırıyoruz.

---

Bu örnekte, ekranda **kalp ve yıldız** sembollerini görmek mümkün olacak. Diğer özel karakterleri de benzer şekilde tanımlayabilir ve LCD ekranınızda farklı semboller oluşturabilirsiniz.


## 9 -


### Örnek Adı:  
**LCD Ekranda Animasyonlu Özel Karakterler (Sağa ve Sola Kaydırma)**

---

### Örnek Açıklama:  
Bu projede, daha önce oluşturduğumuz **özel karakterleri** kullanarak LCD ekranında animasyonlu bir kaydırma işlemi yapacağız. LCD ekran üzerinde, **kalp** ve **yıldız** sembollerini sağa ve sola kaydıracağız. Her iki özel karakteri bir arada kaydırarak, animasyon oluşturacağız.

---

### Örnek Bağlantılar:  
1. **16x2 LCD Bağlantıları:**  
   - **VSS (GND)** → Arduino **GND**  
   - **VCC (5V)** → Arduino **5V**  
   - **V0 (Kontrast Ayarı)** → Potansiyometre ile GND  
   - **RS** → Arduino **D12**  
   - **E** → Arduino **D11**  
   - **D4** → Arduino **D5**  
   - **D5** → Arduino **D4**  
   - **D6** → Arduino **D3**  
   - **D7** → Arduino **D2**  
   - **RW** → **GND** (Bu bağlantı genellikle okuma işlevi için kullanılır, biz yazma yapacağımız için bu bacak doğrudan GND'ye bağlanır)

---

### Örnek Kodu:  
```cpp
#include <LiquidCrystal.h>  // LCD kütüphanesini dahil et

// LCD pin tanımlamaları (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Özel karakterler için diziler (8x5 boyutunda)
byte heart[8] = {
  B00000,
  B01010,
  B11111,
  B11111,
  B11111,
  B01110,
  B00100,
  B00000
};

byte star[8] = {
  B00100,
  B01110,
  B11111,
  B01110,
  B00100,
  B00000,
  B00000,
  B00000
};

void setup() {
  lcd.begin(16, 2);  // LCD'yi başlat (16 sütun, 2 satır)

  // Özel karakterleri oluştur
  lcd.createChar(0, heart);  // Kalp karakterini 0 numaralı char olarak oluştur
  lcd.createChar(1, star);   // Yıldız karakterini 1 numaralı char olarak oluştur

  lcd.clear();  // Ekranı temizle
  lcd.setCursor(0, 0); // İlk satıra yerleştir
  lcd.print("Animasyonlu Karakterler");  // Ekranda yazıyı göster
}

void loop() {
  // Ekranda sağa kaydırma
  for (int i = 0; i < 16; i++) {
    lcd.clear();  // Ekranı temizle
    lcd.setCursor(i, 1);  // Yıldız ve kalp sembollerini sağa kaydırarak yerleştir
    lcd.write(byte(0));   // Kalp sembolünü göster
    lcd.setCursor(i + 1, 1);
    lcd.write(byte(1));   // Yıldız sembolünü göster
    delay(300);  // Kaydırma arasına 300 ms gecikme ekle
  }

  // Ekranda sola kaydırma
  for (int i = 15; i >= 0; i--) {
    lcd.clear();  // Ekranı temizle
    lcd.setCursor(i, 1);  // Yıldız ve kalp sembollerini sola kaydırarak yerleştir
    lcd.write(byte(0));   // Kalp sembolünü göster
    lcd.setCursor(i + 1, 1);
    lcd.write(byte(1));   // Yıldız sembolünü göster
    delay(300);  // Kaydırma arasına 300 ms gecikme ekle
  }
}
```

---

### Açıklama:  
1. **Ekranda Sağ ve Sola Kaydırma:**  
   - **Sağa Kaydırma:** İlk for döngüsü, kalp ve yıldız sembollerini LCD ekranının sağa kaydırarak ekrana yerleştirir.
   - **Sola Kaydırma:** İkinci for döngüsü ise aynı sembollerle LCD ekranının sol tarafına doğru kaydırma işlemi yapar.

2. **`lcd.clear()` Fonksiyonu:**  
   LCD ekranı temizlemek için her adımda **`lcd.clear()`** fonksiyonu kullanılır, böylece her kaydırma işlemi arasında önceki sembol temizlenir.

3. **`lcd.setCursor(x, y)` ve `lcd.write(byte(x))` Fonksiyonları:**  
   - **`lcd.setCursor(x, y)`** komutu, karakterin yerleştirileceği koordinatı belirler.
   - **`lcd.write(byte(x))`** komutu ise belirtilen özel karakteri ekrana yazdırır.

---

Bu projede, LCD ekran üzerinde **kalp** ve **yıldız** sembollerinin sağa ve sola kaymasını sağlayacağız. Bu animasyonu daha da özelleştirerek farklı şekiller ve efektler ekleyebilirsiniz.


## 10 -

### Örnek Adı:  
**7 Segment Ekran Kullanımı (Bağlantılar ve Temel Kodlar)**

---

### Örnek Açıklama:  
Bu projede, bir **7 segment ekran** kullanarak sayıları görüntüleyeceğiz. 7 segment ekranlar, genellikle **0-9** arasındaki rakamları göstermek için kullanılır. Bu ekran, 7 LED'den oluşur ve her bir LED, bir sayı gösterecek şekilde kombinasyonlarla yanar.

Öncelikle, her bir segmentin hangi pinlere bağlanacağını ve sayıların nasıl göründüğünü göstereceğiz.

---

### Örnek Bağlantılar:  
1. **7 Segment Ekran Bağlantıları (Anodik tip):**  
   - **Pin 1 (A Segmenti)** → Arduino **D2**  
   - **Pin 2 (B Segmenti)** → Arduino **D3**  
   - **Pin 3 (C Segmenti)** → Arduino **D4**  
   - **Pin 4 (D Segmenti)** → Arduino **D5**  
   - **Pin 5 (E Segmenti)** → Arduino **D6**  
   - **Pin 6 (F Segmenti)** → Arduino **D7**  
   - **Pin 7 (G Segmenti)** → Arduino **D8**  
   - **Pin 8 (Ortak Katot)** → Arduino **GND**

Not: Bu örnek, anodik tip 7 segment ekranlar için geçerlidir. Eğer katotlu bir ekran kullanıyorsanız, pin bağlantılarını ters yapmanız gerekebilir.

---

### Örnek Kodu:  
```cpp
int segmentPins[] = {2, 3, 4, 5, 6, 7, 8}; // 7 segment pinleri

byte numbers[10] = {
  B00111111,  // 0
  B00000110,  // 1
  B01011011,  // 2
  B01001111,  // 3
  B01100110,  // 4
  B01101101,  // 5
  B01111101,  // 6
  B00000111,  // 7
  B01111111,  // 8
  B01101111   // 9
};

void setup() {
  for (int i = 0; i < 7; i++) pinMode(segmentPins[i], OUTPUT);
}

void loop() {
  for (int num = 0; num < 10; num++) { // 0'dan 9'a kadar sayıları sırayla göster
    for (int i = 0; i < 7; i++) {
      int state = bitRead(numbers[num], i); // İlgili segmentin durumunu al
      digitalWrite(segmentPins[i], state); // Segmenti yak veya söndür
    }
    delay(1000); // 1 saniye bekle
  }
}



```

---

### Açıklama:  
1. **Pin Tanımlamaları:**  
   7 segment ekranın her segmenti, Arduino'nun dijital pinlerine bağlanır. **A** segmenti **D2**'ye, **B** segmenti **D3**'e ve devamında diğer segmentler sırasıyla bağlanır.

2. **`numbers` Dizisi:**  
   Bu dizi, 0'dan 9'a kadar olan sayıların 7 segment ekran üzerindeki gösterimini **binary** formatında tutar. Her bir byte, ekranın her bir segmentinin durumunu belirtir (1 = yanar, 0 = sönük).

3. **`displayNumber()` Fonksiyonu:**  
   Verilen sayıyı göstermek için **`displayNumber()`** fonksiyonu, her bir segmentin durumunu ayarlamak için kullanılır. **`bitRead()`** fonksiyonu ile, her bir sayı için hangi segmentlerin aktif olduğunu kontrol ederiz.

4. **`loop()` Fonksiyonu:**  
   Sayıları **0'dan 9'a kadar** sırayla ekranda göstermek için bir döngü oluşturulmuştur. Her sayıyı 1 saniye boyunca gösterdikten sonra bir sonraki sayıya geçilir.

---

Bu örnekte **7 segment ekran** ile temel sayı göstermek ve segmentlerin nasıl çalıştığını öğrenmek mümkün. Bu mantıkla daha büyük sayılar veya diğer gösterimler de yapılabilir.



## 11 -

### Örnek: Ultrasonik Sensör ile Mesafe Ölçümü (3 Pinli)

#### Örnek Açıklama:
Bu örnekte, **28015 Ultrasonik Mesafe Sensörü** kullanılarak bir mesafe ölçümü yapılır. Sensörün **Signal** pini üzerinden hem ses dalgası gönderilir hem de yankının dönüş süresi ölçülür. Ölçülen mesafe cm cinsinden hesaplanır ve seri monitöre yazdırılır.

#### Örnek Bağlantılar:
1. **VCC** → Arduino **5V**
2. **GND** → Arduino **GND**
3. **Signal** → Arduino **D7**

#### Örnek Kodu:
```cpp
// Ultrasonik sensör pin tanımları
const int signalPin = 7;  // Ultrasonik sensörün trig/echo pinine bağlanır

void setup() {
  Serial.begin(9600); // Seri haberleşmeyi başlat
}

void loop() {
  long duration, distance; // Ses dalgası süresi ve mesafe değişkenleri

  // Ultrasonik sensöre ses dalgası gönder
  pinMode(signalPin, OUTPUT); // Signal pinini çıkış yap
  digitalWrite(signalPin, LOW);
  delayMicroseconds(2);       // Kısa bir süre bekle
  digitalWrite(signalPin, HIGH);
  delayMicroseconds(5);       // 5 mikro saniyelik tetikleme sinyali
  digitalWrite(signalPin, LOW);

  // Ses dalgasının geri dönüş süresini ölç
  pinMode(signalPin, INPUT);  // Signal pinini giriş yap
  duration = pulseIn(signalPin, HIGH); // Yankının dönüş süresini ölç

  // Mesafeyi hesapla (cm cinsinden)
  distance = duration / 29 / 2;

  // Seri monitöre mesafeyi yazdır
  Serial.print("Mesafe: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(100); // 100 ms bekle
}
```

#### Açıklamalar:
1. **Signal Pin:** Sensörün tek pini üzerinden hem veri gönderimi hem de alımı gerçekleştirilir.
2. **Ses Dalga Süresi:** **`pulseIn()`** fonksiyonu ile ses dalgasının gidip gelme süresi ölçülür.
3. **Mesafe Hesabı:** **`duration / 29 / 2`** formülü kullanılarak süre mesafeye dönüştürülür.
4. **Seri Monitör:** Mesafe değeri **Serial Monitor** üzerinden okunabilir.


## 12 -

### Örnek Adı: Ultrasonik Mesafe Sensörü ile Mesafe Ölçümü (LED ve LCD Ekrana Yazdırma - Adafruit LCD)

#### Örnek Açıklama:
Bu örnekte **28015 Ultrasonik Mesafe Sensörü** kullanılarak mesafe ölçümü yapılır. Ölçülen mesafe, bir LED ile görsel olarak belirtilir ve aynı zamanda bir **Adafruit LiquidCrystal** ekranına yazdırılır. LCD ekran, **Adafruit_LiquidCrystal** kütüphanesi kullanılarak kontrol edilir.

#### Örnek Bağlantılar:
1. **Ultrasonik Sensör:**
   - **VCC** → Arduino **5V**
   - **GND** → Arduino **GND**
   - **Signal** → Arduino **D7**

2. **LED:**
   - LED Anotu → Arduino **D2**
   - LED Katotu → 220 ohm direnç → **GND**

3. **LCD (Adafruit):**
   - **VCC** → Arduino **5V**
   - **GND** → Arduino **GND**
   - **SCL** → Arduino **A5**
   - **SDA** → Arduino **A4**

#### Örnek Kodu:
```cpp
#include <Adafruit_LiquidCrystal.h>

// LCD nesnesi oluşturuluyor
Adafruit_LiquidCrystal lcd_1(0);

// Ultrasonik sensör ve LED pin tanımları
const int signalPin = 7;  // Ultrasonik sensörün trig/echo pinine bağlanır
const int ledPin = 2;     // LED bağlanan pin

void setup() {
  lcd_1.begin(16, 2);      // LCD başlatılır (16 sütun, 2 satır)
  lcd_1.print("Mesafe Olcumu"); // Başlangıç mesajı
  delay(2000);             // 2 saniye bekle
  lcd_1.clear();           // Ekranı temizle

  Serial.begin(9600);      // Seri haberleşmeyi başlat
  pinMode(ledPin, OUTPUT); // LED pinini çıkış olarak ayarla
}

void loop() {
  long duration, distance;

  // Ultrasonik sensöre ses dalgası gönder
  pinMode(signalPin, OUTPUT); // Signal pinini çıkış yap
  digitalWrite(signalPin, LOW);
  delayMicroseconds(2);       // Kısa bir süre bekle
  digitalWrite(signalPin, HIGH);
  delayMicroseconds(5);       // 5 mikro saniyelik tetikleme sinyali
  digitalWrite(signalPin, LOW);

  // Ses dalgasının geri dönüş süresini ölç
  pinMode(signalPin, INPUT);  // Signal pinini giriş yap
  duration = pulseIn(signalPin, HIGH); // Yankının dönüş süresini ölç

  // Mesafeyi hesapla (cm cinsinden)
  distance = duration / 29 / 2;

  // Mesafeyi seri monitöre ve LCD'ye yazdır
  Serial.print("Mesafe: ");
  Serial.print(distance);
  Serial.println(" cm");

  lcd_1.setCursor(0, 0); // LCD'nin ilk satırına git
  lcd_1.print("Mesafe: ");
  lcd_1.print(distance);
  lcd_1.print(" cm    "); // Gereksiz karakterleri silmek için boşluk ekle

  // Mesafe 50 cm'den küçükse LED'i yak
  if (distance < 50) {
    digitalWrite(ledPin, HIGH); // LED açık
    lcd_1.setCursor(0, 1);      // LCD'nin ikinci satırına git
    lcd_1.print("Cok Yakin!   ");
  } else {
    digitalWrite(ledPin, LOW);  // LED kapalı
    lcd_1.setCursor(0, 1);      // LCD'nin ikinci satırına git
    lcd_1.print("Guvenli      ");
  }

  delay(500); // 500 ms bekle
}
```

#### Açıklamalar:
1. **LCD Kontrolü:**
   - **Adafruit_LiquidCrystal** kütüphanesi, LCD ekranı kolay bir şekilde kontrol etmek için kullanılır.
   - LCD'ye "Mesafe" ve "Uyarı" bilgileri yazdırılır.
2. **LED Kontrolü:**
   - Mesafe 50 cm'den küçükse LED yanar ve LCD'de "Çok Yakın!" mesajı görüntülenir.
   - Mesafe 50 cm'den büyükse LED söner ve LCD'de "Güvenli" mesajı gösterilir.
3. **Mesafe Hesabı:**
   - Ses dalgasının gidip gelme süresine göre mesafe hesaplanır ve cm cinsinden sonuç alınır.



## 13 -


### Örnek Adı: Ultrasonik Mesafe Sensörü ile Mesafe Ölçümü (RGB LED ile Uyarı)

#### Örnek Açıklama:
Bu projede **28015 Ultrasonik Mesafe Sensörü** kullanılarak mesafe ölçümü yapılır ve mesafe durumuna göre bir **RGB LED** farklı renklerle yanar:
- **Kırmızı**: Mesafe 20 cm'den küçükse.
- **Sarı**: Mesafe 20-50 cm arasındaysa.
- **Yeşil**: Mesafe 50 cm'den büyükse.

#### Örnek Bağlantılar:
1. **Ultrasonik Sensör:**
   - **VCC** → Arduino **5V**
   - **GND** → Arduino **GND**
   - **Signal** → Arduino **D7**

2. **RGB LED:**
   - **Kırmızı Pin** → Arduino **D9**
   - **Yeşil Pin** → Arduino **D10**
   - **Mavi Pin** → Arduino **D11**
   - Ortak katot (veya anot) → **GND** (veya **5V**) (Ortak bağlantı türüne dikkat edin!)

3. **LCD (Adafruit):**
   - **VCC** → Arduino **5V**
   - **GND** → Arduino **GND**
   - **SCL** → Arduino **A5**
   - **SDA** → Arduino **A4**

#### Örnek Kodu:
```cpp
#include <Adafruit_LiquidCrystal.h>

// LCD nesnesi oluşturuluyor
Adafruit_LiquidCrystal lcd_1(0);

// Ultrasonik sensör ve RGB LED pin tanımları
const int signalPin = 7;   // Ultrasonik sensörün trig/echo pinine bağlanır
const int redPin = 9;      // RGB LED'in kırmızı bacağı
const int greenPin = 10;   // RGB LED'in yeşil bacağı
const int bluePin = 11;    // RGB LED'in mavi bacağı

void setup() {
  lcd_1.begin(16, 2);      // LCD başlatılır (16 sütun, 2 satır)
  lcd_1.print("Mesafe Olcumu"); // Başlangıç mesajı
  delay(2000);             // 2 saniye bekle
  lcd_1.clear();           // Ekranı temizle

  Serial.begin(9600);      // Seri haberleşmeyi başlat
  pinMode(redPin, OUTPUT); // Kırmızı LED pini çıkış
  pinMode(greenPin, OUTPUT); // Yeşil LED pini çıkış
  pinMode(bluePin, OUTPUT); // Mavi LED pini çıkış
}

void loop() {
  long duration, distance;

  // Ultrasonik sensöre ses dalgası gönder
  pinMode(signalPin, OUTPUT); // Signal pinini çıkış yap
  digitalWrite(signalPin, LOW);
  delayMicroseconds(2);       // Kısa bir süre bekle
  digitalWrite(signalPin, HIGH);
  delayMicroseconds(5);       // 5 mikro saniyelik tetikleme sinyali
  digitalWrite(signalPin, LOW);

  // Ses dalgasının geri dönüş süresini ölç
  pinMode(signalPin, INPUT);  // Signal pinini giriş yap
  duration = pulseIn(signalPin, HIGH); // Yankının dönüş süresini ölç

  // Mesafeyi hesapla (cm cinsinden)
  distance = duration / 29 / 2;

  // Mesafeyi seri monitöre ve LCD'ye yazdır
  Serial.print("Mesafe: ");
  Serial.print(distance);
  Serial.println(" cm");

  lcd_1.setCursor(0, 0); // LCD'nin ilk satırına git
  lcd_1.print("Mesafe: ");
  lcd_1.print(distance);
  lcd_1.print(" cm    "); // Gereksiz karakterleri silmek için boşluk ekle

  // RGB LED kontrolü
  if (distance < 20) {
    // Kırmızı LED yanar
    digitalWrite(redPin, HIGH);
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin, LOW);
    lcd_1.setCursor(0, 1);      // LCD'nin ikinci satırına git
    lcd_1.print("Cok Yakin!   ");
  } else if (distance >= 20 && distance <= 50) {
    // Sarı renk için kırmızı ve yeşil LED'ler birlikte yanar
    digitalWrite(redPin, HIGH);
    digitalWrite(greenPin, HIGH);
    digitalWrite(bluePin, LOW);
    lcd_1.setCursor(0, 1);      // LCD'nin ikinci satırına git
    lcd_1.print("Orta Mesafe  ");
  } else {
    // Yeşil LED yanar
    digitalWrite(redPin, LOW);
    digitalWrite(greenPin, HIGH);
    digitalWrite(bluePin, LOW);
    lcd_1.setCursor(0, 1);      // LCD'nin ikinci satırına git
    lcd_1.print("Guvenli      ");
  }

  delay(500); // 500 ms bekle
}
```

#### Açıklamalar:
1. **RGB LED Kullanımı:**
   - RGB LED, her bir bacağı farklı renkler için kontrol edilir.
   - Kırmızı, sarı (kırmızı+yeşil), ve yeşil renkler farklı mesafe aralıkları için yanar.
2. **LCD Kullanımı:**
   - Mesafe ve uyarı mesajları LCD ekranına yazdırılır.
3. **Mesafe Hesabı:**
   - Ultrasonik sensörden gelen yankı süresi kullanılarak mesafe hesaplanır.




## 14 -

###  Gaz Sensörü Kullanımı

**Örnek Açıklama:**
Gaz sensörü, ortamda bulunan gazların seviyesini algılayan bir sensördür. Bu örnekte, bir gaz sensörü kullanarak, belirli bir gaz seviyesi tespit edildiğinde bir işlem yapılması sağlanabilir. Bağlantıları doğru şekilde yaparak, sensörün dijital değerlerini okuma işlemi gerçekleştirilir.

**Örnek Bağlantıları:**
- Gaz sensörünün A1 pinine 1k direnç ve GND bağlantısı yapılır.
- Gaz sensörünün H1 pini GND'ye bağlanır.
- Gaz sensörünün A2 pini A0 pinine bağlanır.
- Gaz sensörünün B1, B2 ve H2 pinleri 5V'a bağlanır.

**Bağlantı Şeması:**
1. Gaz sensörünün A1 pinini 1k direnç ile GND'ye bağla.
2. Gaz sensörünün H1 pinini GND'ye bağla.
3. Gaz sensörünün A2 pinini A0 pinine bağla.
4. Gaz sensörünün B1, B2 ve H2 pinlerini 5V'a bağla.

**Örnek Kodu:**

```cpp
int gasSensorPin = A0;  // Gaz sensörünün bağlı olduğu analog pin
int gasValue = 0;  // Gaz sensöründen alınan analog değer

void setup() {
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  gasValue = analogRead(gasSensorPin);  // Gaz sensöründen veri oku
  Serial.println(gasValue);  // Okunan değeri seri port üzerinden yazdır
  delay(1000);  // 1 saniye bekle
}
```

Bu örnekte, gaz sensöründen alınan analog değerler seri port üzerinden görüntülenir. Eğer sensördeki gaz seviyesi belirli bir eşiği aşarsa, daha ileri işlemler ekleyebilirsin.


## 15 -

### 15- Gaz Sensörü ile Ortamdaki Gaz Seviyesine Göre RGB LED Yakmak

**Örnek Açıklama:**
Bu örnekte, gaz sensörü kullanarak ortamda gaz seviyesi tespit edilecek ve bu seviyeye göre RGB LED'in rengini değiştireceğiz. Eğer gaz seviyesi belirli bir eşik değerin üzerine çıkarsa, LED kırmızıya dönecek, düşük seviyelerde ise yeşil veya mavi renkte yanacak.

**Örnek Bağlantıları:**
- Gaz sensörünün A1 pinine 1k direnç ve GND bağlantısı yapılır.
- Gaz sensörünün H1 pini GND'ye bağlanır.
- Gaz sensörünün A2 pini A0 pinine bağlanır.
- Gaz sensörünün B1, B2 ve H2 pinleri 5V'a bağlanır.
- RGB LED'in kırmızı, yeşil ve mavi pinleri sırasıyla 9, 10 ve 11 numaralı pinlere bağlanır.
- RGB LED'in ortak katodu (ya da anodu) GND'ye bağlanır (LED'in tipine göre değişebilir).

**Bağlantı Şeması:**
1. Gaz sensörünün A1 pinini 1k direnç ile GND'ye bağla.
2. Gaz sensörünün H1 pinini GND'ye bağla.
3. Gaz sensörünün A2 pinini A0 pinine bağla.
4. Gaz sensörünün B1, B2 ve H2 pinlerini 5V'a bağla.
5. RGB LED'in kırmızı pinini 9 numaralı dijital pine, yeşil pinini 10 numaralı dijital pine ve mavi pinini 11 numaralı dijital pine bağla.
6. RGB LED'in ortak katodu (ya da anodu) GND'ye bağla.

**Örnek Kodu:**

```cpp
int gasSensorPin = A0;  // Gaz sensörünün bağlı olduğu analog pin
int redPin = 9;         // RGB LED'in kırmızı pini
int greenPin = 10;      // RGB LED'in yeşil pini
int bluePin = 11;       // RGB LED'in mavi pini
int gasValue = 0;       // Gaz sensöründen alınan analog değer
int threshold = 500;    // Gaz seviyesinin eşik değeri

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  gasValue = analogRead(gasSensorPin);  // Gaz sensöründen veri oku
  Serial.println(gasValue);  // Okunan değeri seri port üzerinden yazdır

  if (gasValue > threshold) {  // Eğer gaz seviyesi yüksekse
    // Kırmızı ışık yak
    analogWrite(redPin, 255);
    analogWrite(greenPin, 0);
    analogWrite(bluePin, 0);
  } else {  // Gaz seviyesi düşükse
    // Yeşil ışık yak
    analogWrite(redPin, 0);
    analogWrite(greenPin, 255);
    analogWrite(bluePin, 0);
  }
  delay(1000);  // 1 saniye bekle
}
```

Bu kodda, gaz sensöründen alınan analog değer belirli bir eşik değerin üzerine çıkarsa RGB LED kırmızıya döner, aksi takdirde yeşil renkte yanar. Gaz seviyesi değiştikçe renk değişiklikleri gözlemlenebilir.


## 16 -

### 16- Sıcaklık Sensörü Kullanımı (3 Pinli)

**Örnek Açıklama:**
Sıcaklık sensörleri, ortamın sıcaklığını ölçmek için kullanılır. Bu örnekte, 3 pinli bir sıcaklık sensörü kullanarak ortamın sıcaklık değerini okuma işlemi yapılacaktır. Sensörün çıkışı analog olacak şekilde, sıcaklık değerini belirli bir pin üzerinden okuyacağız.

**Örnek Bağlantıları:**
- Sıcaklık sensörünün 3 pini vardır: VCC (5V), GND, ve A0 (analog çıkış).
- VCC pini 5V'a bağlanır.
- GND pini GND'ye bağlanır.
- A0 pini, Arduino'nun A0 pinine bağlanır.

**Bağlantı Şeması:**
1. Sıcaklık sensörünün VCC pinini 5V'a bağla.
2. Sıcaklık sensörünün GND pinini GND'ye bağla.
3. Sıcaklık sensörünün çıkış pinini (A0) Arduino'nun A0 pinine bağla.

**Örnek Kodu:**

```cpp
int tempPin = A0;  // Sıcaklık sensörünün bağlı olduğu analog pin
int tempValue = 0; // Sıcaklık sensöründen alınan analog değer

void setup() {
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  tempValue = analogRead(tempPin);  // Sıcaklık sensöründen veri oku
  Serial.println(tempValue);  // Okunan değeri seri port üzerinden yazdır
  delay(1000);  // 1 saniye bekle
}
```

Bu örnekte, sıcaklık sensöründen alınan analog değeri seri monitörde görebilirsiniz. Değer, ortamın sıcaklık seviyesini yansıtır ve daha sonra sıcaklık değerine göre bir işlem yapılabilir.


## 17 -

### 17- Sıcaklık Sensörü (Basit RGB LED ile Uyarı ve LCD Ekrana Yazdırmalı) – Yeni Şablon

**Örnek Açıklama:**
Bu örnekte, sıcaklık sensörü ile ortamın sıcaklık değerini ölçüp, sıcaklık çok yüksekse kırmızı renk, normal sıcaklıkta ise yeşil renk gösteren RGB LED'ler kullanacağız. Ayrıca, sıcaklık LCD ekrana yazdırılacak.

**Örnek Bağlantıları:**
- Sıcaklık sensörünün VCC pini 5V'a bağlanır.
- Sıcaklık sensörünün GND pini GND'ye bağlanır.
- Sıcaklık sensörünün çıkış pini (A0) Arduino'nun A0 pinine bağlanır.
- RGB LED'in kırmızı pini 9 numaralı dijital pine, yeşil pinini 10 numaralı dijital pine ve mavi pinini 11 numaralı dijital pine bağla.
- LCD ekranın bağlantısı:
  - VCC pini 5V'a,
  - GND pini GND'ye,
  - SDA ve SCL pinleri sırasıyla A4 ve A5 pinlerine bağlanır.

**Bağlantı Şeması:**
1. Sıcaklık sensörünün VCC pinini 5V'a bağla.
2. Sıcaklık sensörünün GND pinini GND'ye bağla.
3. Sıcaklık sensörünün çıkış pinini A0 pinine bağla.
4. RGB LED'in kırmızı pinini 9 numaralı dijital pine, yeşil pinini 10 numaralı dijital pine ve mavi pinini 11 numaralı dijital pine bağla.
5. LCD ekranın VCC pinini 5V'a, GND pinini GND'ye bağla.
6. LCD ekranın SDA ve SCL pinlerini sırasıyla A4 ve A5 pinlerine bağla.

**Örnek Kodu:**

```cpp

#include <Adafruit_LiquidCrystal.h>  // LCD ekran için gerekli kütüphane

int tempPin = A0;  // Sıcaklık sensörünün bağlı olduğu analog pin
int redPin = 9;    // RGB LED'in kırmızı pini
int greenPin = 10; // RGB LED'in yeşil pini
int bluePin = 11;  // RGB LED'in mavi pini
int tempValue = 0; // Sıcaklık sensöründen alınan analog değer
Adafruit_LiquidCrystal lcd_1(0);  // LCD ekran nesnesi

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  lcd_1.begin(16, 2);  // LCD ekranı başlat
  lcd_1.print("Sicaklik: ");  // LCD ekrana yazı yazdır
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  tempValue = analogRead(tempPin);  // Sıcaklık sensöründen veri oku
  int temperature = map(tempValue, 0, 1023, 0, 100);  // Analog değeri sıcaklığa dönüştür

  // LCD ekranda sıcaklık değerini göster
  lcd_1.setCursor(0, 1);
  lcd_1.print(temperature);
  lcd_1.print(" C");

  Serial.println(temperature);  // Seri port üzerinden sıcaklık değerini yazdır

  if (temperature > 30) {  // Eğer sıcaklık 30°C'den yüksekse
    // Kırmızı ışık yak
    analogWrite(redPin, 255);
    analogWrite(greenPin, 0);
    analogWrite(bluePin, 0);
  } else {  // Sıcaklık düşükse
    // Yeşil ışık yak
    analogWrite(redPin, 0);
    analogWrite(greenPin, 255);
    analogWrite(bluePin, 0);
  }

  delay(1000);  // 1 saniye bekle
}
```

Bu kodda, sıcaklık sensöründen alınan değeri LCD ekranda görüntüleyeceksin. Ayrıca sıcaklık değeri 30°C'yi aştığında RGB LED kırmızıya dönecek, aksi takdirde yeşil renkte yanacak.


## 18 -

### 18- Toprak Nem Sensörü Kullanımı (3 Pinli)

**Örnek Açıklama:**
Toprak nem sensörü, toprakta ne kadar nem olduğunu ölçer ve bu veriyi kullanarak bitkilerin sulanma ihtiyacını belirleyebilirsiniz. Bu örnekte, 3 pinli toprak nem sensörünü kullanarak, toprağın nem seviyesini okuyacağız ve bu değeri bir LCD ekrana yazdıracağız.

**Örnek Bağlantıları:**
- Toprak nem sensörünün VCC pini 5V'a bağlanır.
- Toprak nem sensörünün GND pini GND'ye bağlanır.
- Toprak nem sensörünün çıkış pini (A0) Arduino'nun A0 pinine bağlanır.
- LCD ekranın bağlantısı:
  - VCC pini 5V'a,
  - GND pini GND'ye,
  - SDA ve SCL pinleri sırasıyla A4 ve A5 pinlerine bağlanır.

**Bağlantı Şeması:**
1. Toprak nem sensörünün VCC pinini 5V'a bağla.
2. Toprak nem sensörünün GND pinini GND'ye bağla.
3. Toprak nem sensörünün çıkış pinini A0 pinine bağla.
4. LCD ekranın VCC pinini 5V'a, GND pinini GND'ye bağla.
5. LCD ekranın SDA ve SCL pinlerini sırasıyla A4 ve A5 pinlerine bağla.

**Örnek Kodu:**

```cpp

#include <Adafruit_LiquidCrystal.h>  // LCD ekran için gerekli kütüphane

int soilMoisturePin = A0;  // Toprak nem sensörünün bağlı olduğu analog pin
int moistureValue = 0;      // Toprak nem sensöründen alınan analog değer
Adafruit_LiquidCrystal lcd_1(0);  // LCD ekran nesnesi

void setup() {
  lcd_1.begin(16, 2);  // LCD ekranı başlat
  lcd_1.print("Toprak Nem: ");  // LCD ekrana yazı yazdır
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  moistureValue = analogRead(soilMoisturePin);  // Toprak nem sensöründen veri oku
  int moisturePercentage = map(moistureValue, 0, 1023, 0, 100);  // Analog değeri nem yüzdesine dönüştür

  // LCD ekranda toprak nem seviyesini göster
  lcd_1.setCursor(0, 1);
  lcd_1.print(moisturePercentage);
  lcd_1.print(" %");

  Serial.println(moisturePercentage);  // Seri port üzerinden nem yüzdesini yazdır

  delay(1000);  // 1 saniye bekle
}
```

Bu örnekte, toprak nem sensöründen alınan analog değeri LCD ekranda yüzde olarak göstereceksiniz. Ayrıca, nem seviyesini seri port üzerinden de izleyebilirsiniz.



## 19 -

### 19- Toprak Nem Sensörü (Basit RGB LED ile Uyarı ve LCD Ekrana Yazdırmalı)

**Örnek Açıklama:**
Bu örnekte, toprak nem sensöründen alınan veriye göre RGB LED'in rengini değiştireceğiz. Eğer toprak çok kuruysa kırmızı LED yansa, nem seviyesi yeterli ise yeşil LED yanacak. Ayrıca, toprak nem seviyesi LCD ekrana yazdırılacak.

**Örnek Bağlantıları:**
- Toprak nem sensörünün VCC pini 5V'a bağlanır.
- Toprak nem sensörünün GND pini GND'ye bağlanır.
- Toprak nem sensörünün çıkış pini (A0) Arduino'nun A0 pinine bağlanır.
- RGB LED'in kırmızı pini 9 numaralı dijital pine, yeşil pinini 10 numaralı dijital pine ve mavi pinini 11 numaralı dijital pine bağla.
- LCD ekranın bağlantısı:
  - VCC pini 5V'a,
  - GND pini GND'ye,
  - SDA ve SCL pinleri sırasıyla A4 ve A5 pinlerine bağlanır.

**Bağlantı Şeması:**
1. Toprak nem sensörünün VCC pinini 5V'a bağla.
2. Toprak nem sensörünün GND pinini GND'ye bağla.
3. Toprak nem sensörünün çıkış pinini A0 pinine bağla.
4. RGB LED'in kırmızı pinini 9 numaralı dijital pine, yeşil pinini 10 numaralı dijital pine ve mavi pinini 11 numaralı dijital pine bağla.
5. LCD ekranın VCC pinini 5V'a, GND pinini GND'ye bağla.
6. LCD ekranın SDA ve SCL pinlerini sırasıyla A4 ve A5 pinlerine bağla.

**Örnek Kodu:**

```cpp
#include <Wire.h>  // LCD ekran için gerekli kütüphane
#include <Adafruit_LiquidCrystal.h>  // LCD ekran için gerekli kütüphane

int soilMoisturePin = A0;  // Toprak nem sensörünün bağlı olduğu analog pin
int redPin = 9;             // RGB LED'in kırmızı pini
int greenPin = 10;          // RGB LED'in yeşil pini
int bluePin = 11;           // RGB LED'in mavi pini
int moistureValue = 0;      // Toprak nem sensöründen alınan analog değer
Adafruit_LiquidCrystal lcd_1(0);  // LCD ekran nesnesi

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  lcd_1.begin(16, 2);  // LCD ekranı başlat
  lcd_1.print("Toprak Nem: ");  // LCD ekrana yazı yazdır
  Serial.begin(9600);  // Seri haberleşmeyi başlat
}

void loop() {
  moistureValue = analogRead(soilMoisturePin);  // Toprak nem sensöründen veri oku
  int moisturePercentage = map(moistureValue, 0, 1023, 0, 100);  // Analog değeri nem yüzdesine dönüştür

  // LCD ekranda toprak nem seviyesini göster
  lcd_1.setCursor(0, 1);
  lcd_1.print(moisturePercentage);
  lcd_1.print(" %");

  Serial.println(moisturePercentage);  // Seri port üzerinden nem yüzdesini yazdır

  // Toprak nem seviyesine göre RGB LED rengini ayarla
  if (moisturePercentage < 30) {  // Eğer toprak kuruysa
    analogWrite(redPin, 255);    // Kırmızı ışık yak
    analogWrite(greenPin, 0);
    analogWrite(bluePin, 0);
  } else {  // Eğer toprak yeterli nem seviyesine sahipse
    analogWrite(redPin, 0);      // Yeşil ışık yak
    analogWrite(greenPin, 255);
    analogWrite(bluePin, 0);
  }

  delay(1000);  // 1 saniye bekle
}
```

Bu örnekte, toprak nem sensöründen alınan değer, LCD ekranda nem yüzdesi olarak görüntülenir. Toprak çok kuruysa RGB LED kırmızıya döner, nem seviyesinin yeterli olduğu durumda ise yeşil renkte yanar.



Tabii, bonus olarak bir örnek hazırlayalım! Bu örnekte, bir servo motor kullanarak bir nesnenin konumunu değiştireceğiz. Servo motorun açısını kontrol etmek için bir potansiyometre (değişken direnç) kullanacağız.

###  Servo Motor Kullanımı

**Örnek Açıklama:**
Bu örnekte, bir potansiyometre ile servo motorun açısını kontrol edeceğiz. Potansiyometreyi döndürdükçe, servo motorun açısı değişecek ve bu sayede nesneleri hareket ettirebileceğiz.

**Örnek Bağlantıları:**
* Servo motorun VCC pini 5V'a bağlanır.
* Servo motorun GND pini GND'ye bağlanır.
* Servo motorun kontrol pini (sinyal pini) Arduino'nun 9 numaralı dijital pinine bağlanır.
* Potansiyometrenin bir ucu 5V'a bağlanır.
* Potansiyometrenin diğer ucu GND'ye bağlanır.
* Potansiyometrenin ortadaki çıkış pini (sinyal pini) A0 pinine bağlanır.

**Bağlantı Şeması:**
1. Servo motorun VCC pinini 5V'a bağla.
2. Servo motorun GND pinini GND'ye bağla.
3. Servo motorun kontrol pinini (sinir pini) 9 numaralı dijital pine bağla.
4. Potansiyometrenin bir ucunu 5V'a, diğer ucunu GND'ye bağla.
5. Potansiyometrenin ortadaki çıkış pinini (sinyal pini) A0 pinine bağla.

**Örnek Kodu:**

```cpp
#include <Servo.h>  // Servo motor kütüphanesini dahil et

int potPin = A0;    // Potansiyometrenin bağlı olduğu analog pin
int potValue = 0;    // Potansiyometreden alınan analog değer
int servoPin = 9;    // Servo motorun bağlı olduğu dijital pin
Servo myServo;       // Servo motor nesnesi oluşturuluyor

void setup() {
  myServo.attach(servoPin);  // Servo motoru belirtilen pime bağla
  Serial.begin(9600);        // Seri haberleşmeyi başlat
}

void loop() {
  potValue = analogRead(potPin);  // Potansiyometreden veri oku
  int angle = map(potValue, 0, 1023, 0, 180);  // Potansiyometreyi 0-180 derece arasında harita

  myServo.write(angle);  // Servo motoru potansiyometreye göre hareket ettir

  Serial.print("Potansiyometre Değeri: ");
  Serial.print(potValue);
  Serial.print(" - Servo Açısı: ");
  Serial.println(angle);  // Seri port üzerinden potansiyometre ve servo açısını yazdır

  delay(15);  // Servo motorun hareketini tamamlaması için kısa bir bekleme
}
```

**Açıklamalar:**
* **Servo Motor**: Servo motorun açısını `myServo.write(angle)` komutu ile ayarlıyoruz. Burada `angle` değişkeni 0 ile 180 arasında bir değer alır.
* **Potansiyometre**: Potansiyometre ile analog bir sinyal alıyoruz. Bu değeri `analogRead(potPin)` komutuyla okuyoruz. Bu değeri 0 ile 1023 arasında bir değere dönüştürüyoruz ve ardından 0 ile 180 arasında bir açıya çeviriyoruz.

Bu örneği kullanarak, potansiyometreyi döndürdükçe servo motorun açısının nasıl değiştiğini gözlemleyebilirsiniz.
