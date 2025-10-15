öğrenci no: 250541023
öğrenci ad soyad : Ezgi Efsa Güleç

BAŞLA

// --- 1. KULLANICI GİRİŞİ ---
YAZ "E-ticaret sitesine hoş geldiniz."
YAZ "Giriş yapmak için 1, Kayıt olmak için 2 seçin:"
GİRİS_SECENEK ← GİR

EĞER GİRİS_SECENEK = 1 İSE
    YAZ "E-posta ve şifre giriniz:"
    EMAIL ← GİR
    SIFRE ← GİR
    EĞER KullanıcıDoğrula(EMAIL, SIFRE) = DOĞRU İSE
        YAZ "Giriş başarılı."
    DEĞİLSE
        YAZ "Giriş başarısız. Tekrar deneyin."
        GİT BAŞA
    BİTİR EĞER
DEĞİLSE EĞER GİRİS_SECENEK = 2 İSE
    YAZ "Yeni kullanıcı kaydı oluşturuluyor..."
    KULLANICI_BİLGİLERİ ← GİR
    KayıtOl(KULLANICI_BİLGİLERİ)
    YAZ "Kayıt başarılı. Giriş yapabilirsiniz."
    GİT BAŞA
BİTİR EĞER

// --- 2. ÜRÜN SEÇİMİ VE SEPETE EKLEME ---
DÖNGÜ
    YAZ "Ürün kataloğu gösteriliyor..."
    URUN ← ÜrünSeç()
    MİKTAR ← GİR("Kaç adet eklemek istiyorsunuz?")

    EĞER StokKontrol(URUN, MİKTAR) = DOĞRU İSE
        SepeteEkle(URUN, MİKTAR)
        YAZ "Ürün sepete eklendi."
    DEĞİLSE
        YAZ "Yetersiz stok. Lütfen miktarı azaltın."
    BİTİR EĞER

    YAZ "Başka ürün eklemek istiyor musunuz? (E/H)"
    CEVAP ← GİR
    EĞER CEVAP = "H" İSE
        ÇIK DÖNGÜ
    BİTİR EĞER
BİTİR DÖNGÜ

// --- 3. SEPET YÖNETİMİ ---
YAZ "Sepetinizdeki ürünler:"
SepetiGoster()

DÖNGÜ
    YAZ "1: Miktar değiştir, 2: Ürün sil, 3: Devam et"
    ISLEM ← GİR
    EĞER ISLEM = 1 İSE
        URUN ← GİR("Hangi ürün?")
        YENI_MIKTAR ← GİR("Yeni miktar:")
        EĞER StokKontrol(URUN, YENI_MIKTAR) = DOĞRU İSE
            SepetGüncelle(URUN, YENI_MIKTAR)
        DEĞİLSE
            YAZ "Yetersiz stok!"
        BİTİR EĞER
    DEĞİLSE EĞER ISLEM = 2 İSE
        URUN ← GİR("Silinecek ürün:")
        SepettenSil(URUN)
    DEĞİLSE EĞER ISLEM = 3 İSE
        ÇIK DÖNGÜ
    DEĞİLSE
        YAZ "Geçersiz seçim!"
    BİTİR EĞER
BİTİR DÖNGÜ

// --- 4. İNDİRİM KODU UYGULAMA ---
YAZ "İndirim kodunuz var mı? (E/H)"
KOD_CEVAP ← GİR
EĞER KOD_CEVAP = "E" İSE
    KOD ← GİR("İndirim kodunu giriniz:")
    EĞER KuponKontrol(KOD) = GEÇERLİ İSE
        İNDİRİM ← KuponUygula(KOD)
        YAZ "İndirim uygulandı."
    DEĞİLSE
        YAZ "Kod geçersiz veya süresi dolmuş."
    BİTİR EĞER
BİTİR EĞER

// --- 5. KARGO ÜCRETİ HESAPLAMA ---
ADRES ← GİR("Teslimat adresinizi girin:")
KARGO_UCRET ← KargoHesapla(ADRES)
TOPLAM ← SepetToplam() - İNDİRİM + KARGO_UCRET

EĞER TOPLAM > 1000 İSE
    YAZ "Kargo ücretsiz!"
    KARGO_UCRET ← 0
    TOPLAM ← SepetToplam() - İNDİRİM
BİTİR EĞER

YAZ "Toplam tutar: ", TOPLAM

// --- 6. ÖDEME AŞAMASI ---
YAZ "Ödeme yöntemi seçin: 1-Kredi Kartı 2-Kapıda Ödeme"
ODEME_TIPI ← GİR

EĞER ODEME_TIPI = 1 İSE
    YAZ "Kart bilgilerini giriniz."
    KART_BILGI ← GİR
    EĞER ÖdemeOnayı(KART_BILGI, TOPLAM) = BAŞARILI İSE
        YAZ "Ödeme başarılı."
        StokGüncelle()
        SiparişOluştur()
        YAZ "Siparişiniz onaylandı. Teşekkür ederiz!"
    DEĞİLSE
        YAZ "Ödeme başarısız! Lütfen tekrar deneyin."
    BİTİR EĞER
DEĞİLSE EĞER ODEME_TIPI = 2 İSE
    YAZ "Kapıda ödeme seçildi. Sipariş oluşturuluyor..."
    SiparişOluştur()
    YAZ "Siparişiniz kaydedildi. Ödeme teslimatta alınacaktır."
DEĞİLSE
    YAZ "Geçersiz ödeme tipi!"
BİTİR EĞER

// --- 7. SİPARİŞ TAMAMLAMA ---
YAZ "Sipariş durumu: Hazırlanıyor."
YAZ "Kargo bilgileri daha sonra e-posta ile gönderilecektir."

BİTİR
