öğrenci no :250541023
öğrenci ad soyad:ezgi efsa güleç

BAŞLA

   // 1. Kimlik Doğrulama
   YAZ "TC Kimlik Numaranızı Giriniz:"
   TC ← GİRİŞ_AL()
   YAZ "Şifrenizi Giriniz:"
   SIFRE ← GİRİŞ_AL()
   
   EĞER TC ve SIFRE veritabanında DOĞRU İSE
       YAZ "Kimlik doğrulama başarılı."
   DEĞİLSE
       YAZ "Hatalı kimlik veya şifre. Tekrar deneyiniz."
       BİTİR
   BİTİR_EĞER

   // 2. İşlem Seçimi
   YAZ "Yapmak istediğiniz işlemi seçiniz:"
   YAZ "1 - Randevu İşlemleri"
   YAZ "2 - Tahlil Sonuçları Görüntüleme"
   ISLEM ← GİRİŞ_AL()

   // 3. Randevu Sistemi
   EĞER ISLEM = "1" İSE
       POLIKLINIK_LISTESI ← ["Dahiliye", "Kardiyoloji", "Göz", "Ortopedi", "KBB"]
       YAZ "Lütfen poliklinik seçiniz:"
       HER BİR POLIKLINIK içinde POLIKLINIK_LISTESI
           YAZ POLIKLINIK
       DÖNGÜ_SON
       
       SEÇİLEN_POLIKLINIK ← GİRİŞ_AL()
       DOKTOR_LISTESI ← DOKTORLARI_GETİR(SEÇİLEN_POLIKLINIK)
       YAZ "Mevcut doktorlar:"
       HER BİR DOKTOR içinde DOKTOR_LISTESI
           YAZ DOKTOR.AD_SOYAD
       DÖNGÜ_SON
       
       SEÇİLEN_DOKTOR ← GİRİŞ_AL("Randevu almak istediğiniz doktoru giriniz:")
       UYGUN_SAATLER ← RANDEVU_SAATLERİ(SEÇİLEN_DOKTOR)
       YAZ "Uygun saatler:"
       HER BİR SAAT içinde UYGUN_SAATLER
           YAZ SAAT
       DÖNGÜ_SON
       
       SEÇİLEN_SAAT ← GİRİŞ_AL("Randevu saatinizi seçiniz:")
       YAZ "Randevuyu onaylıyor musunuz? (E/H):"
       ONAY ← GİRİŞ_AL()
       
       EĞER ONAY = "E" İSE
           RANDEVU_KAYDET(TC, SEÇİLEN_DOKTOR, SEÇİLEN_SAAT)
           YAZ "Randevunuz başarıyla oluşturuldu."
           TELEFON ← TELEFON_GETİR(TC)
           SMS_METNI ← "Sayın hasta, " + SEÇİLEN_DOKTOR + " ile " + SEÇİLEN_SAAT + " tarihinde randevunuz onaylanmıştır."
           SMS_GÖNDER(TELEFON, SMS_METNI)
           YAZ "Bilgilendirme SMS'i gönderildi."
       DEĞİLSE
           YAZ "Randevu işlemi iptal edildi."
       BİTİR_EĞER
   BİTİR_EĞER

   // 4. Tahlil Sonuçları Sistemi
   EĞER ISLEM = "2" İSE
       EĞER TAHLIL_VAR_MI(TC) = DOĞRU İSE
           EĞER SONUC_HAZIR_MI(TC) = DOĞRU İSE
               YAZ "Tahlil sonucunuz hazır."
               YAZ "Görüntülemek için 1, PDF indirmek için 2 giriniz:"
               SECIM ← GİRİŞ_AL()
               EĞER SECIM = "1" İSE
                   TAHLIL_GORUNTULE(TC)
               DEĞİLSE EĞER SECIM = "2" İSE
                   PDF_INDİR(TC)
               DEĞİLSE
                   YAZ "Geçersiz seçim."
               BİTİR_EĞER
           DEĞİLSE
               YAZ "Tahlil sonucunuz henüz hazır değil. Lütfen daha sonra tekrar deneyiniz."
           BİTİR_EĞER
       DEĞİLSE
           YAZ "Sistemde tahlil kaydınız bulunamadı."
       BİTİR_EĞER
   BİTİR_EĞER

BİTİR
```
