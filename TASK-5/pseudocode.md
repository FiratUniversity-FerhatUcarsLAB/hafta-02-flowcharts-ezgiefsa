öğrenci no:25041023
öğrenci ad soy ad:ezgi efsa güleç

FONKSIYON AkıllıEvGüvenlikSistemi()

    // Sistem Değişkenlerinin Başlatılması
    Durum = "Hazır"
    AlarmAktif = YANLIŞ
    TehlikeSeviyesi = 0
    BildirimGerekiyor = YANLIŞ

    // Sonsuz Döngü: Sistem 7/24 çalışır.
    DÖNGÜ SÜREKLİ (DOĞRU)

        // 1. SENSÖR OKUMA VE VERİ ANALİZİ
        OkuTümSensörler() // Hareket, kapı/pencere, duman vb. verileri toplar.
        TehlikeSeviyesi = AnalizEtSensörVerisi(SensörVerileri) // Verilere göre tehdit seviyesini belirler.

        // 2. TEHDİT KONTROLÜ VE AKSİYON ALMA
        EĞER (TehlikeSeviyesi > 0) İSE
            // Tehdit Algılandı
            AlarmAktif = DOĞRU
            
            // Tehlike Seviyesine Göre Farklı Aksiyonlar
            EĞER (TehlikeSeviyesi == 1) İSE
                YazLog("Düşük seviyeli şüpheli aktivite.")
                GönderBildirim("Şüpheli aktivite algılandı (Düşük).")
            DEĞİLSE EĞER (TehlikeSeviyesi == 2) İSE
                YazLog("Orta seviyeli ihlal algılandı.")
                ÇalAlarm(DÜŞÜK_SES)
                GönderBildirim("Güvenlik ihlali (Orta).")
            DEĞİLSE EĞER (TehlikeSeviyesi == 3) İSE
                YazLog("YÜKSEK SEVİYELİ ACİL DURUM (GİRİŞ/YANGIN)!")
                ÇalAlarm(YÜKSEK_SES)
                KaydetVideo()
                GönderBildirim("ACİL DURUM! Yüksek tehlike. Polis/İtfaiye uyarılıyor.")
            SON
            
        DEĞİLSE
            // Sistem Güvenli
            YazLog("Sistem güvenli. Tüm sensörler normal.")
        SON

        // 3. ALARM VE SIFIRLAMA DÖNGÜSÜ
        EĞER (AlarmAktif) İSE
            Durum = "Alarm Aktif"
            YazLog("Alarm aktif durumda. Kullanıcı komutu bekleniyor.")
            
            // Kullanıcıdan Gelen Sıfırlama Komutunu Kontrol Et
            EĞER (KontrolEtKomut("Sıfırla")) İSE
                AlarmAktif = YANLIŞ
                TehlikeSeviyesi = 0
                Durum = "Hazır"
                SıfırlaAlarmSesi()
                YazLog("Alarm sıfırlandı. Sistem Hazır.")
            DEĞİLSE
                // Alarm Çalmaya Devam Eder (Sıfırlama gelene kadar)
                Bekle(500) 
            SON
        SON
        
        Bekle(1000) // Döngü beklemesi (1 saniye)
        
    SON DÖNGÜ
SON FONKSIYON

// --- Yardımcı Fonksiyonlar ---
// Fonksiyonların detayları pseudocode'da atlanmıştır, 
// ancak bir geliştirme ortamında tanımlanmalıdır.

FONKSIYON OkuTümSensörler() 
FONKSIYON AnalizEtSensörVerisi(Veriler) İADE TehlikeSeviyesi
FONKSIYON ÇalAlarm(SesSeviyesi)
FONKSIYON SıfırlaAlarmSesi()
FONKSIYON GönderBildirim(Mesaj)
FONKSIYON YazLog(Mesaj)
FONKSIYON KaydetVideo()
FONKSIYON KontrolEtKomut(Komut) İADE DOĞRU/YANLIŞ // Kullanıcı arayüzünden/API'dan komut alır.
