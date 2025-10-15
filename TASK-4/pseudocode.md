öğrenci no:250541023
öğrenci adı soyadı:Ezgi Efsa Güleç

# Üniversite Ders Kayıt Sistemi Pseudocode

Bu dosya, üniversite ders kayıt sisteminin genel akışını ve kontrollerini içeren detaylı pseudocode'u içerir.  
Sistem, öğrencinin ders seçimi sırasında aşağıdaki kontrolleri yapar:

- Ders kontenjanı
- Ön koşul tamamlanması
- Ders saatleri çakışması
- Kredi limiti
- Danışman onayı

## Pseudocode

```text
BASLA

// 1. Öğrenci girişi
Öğrenci_giris_yap()

// 2. Ders listesi görüntüleme
ders_listesi = dersleri_goruntule()

// 3. Seçilen dersler için boş liste oluştur
secilen_dersler = []

// 4. Ders seçimi döngüsü
DÖNGÜ her ders için ders_listesi:

    EĞER öğrenci_dersi_secmiş_mi(ders) ISE

        devam_et = EVET

        // 4a. Kontenjan kontrolü
        EĞER ders_kontenjani_dolmuş_mu(ders) ISE
            "Ders kontenjanı dolu" mesajı göster
            devam_et = HAYIR
        DEĞILSE
            // 4b. Ön koşul kontrolü
            EĞER ön_kosul_tamamlanmamis_mi(ders) ISE
                "Ön koşul tamamlanmamış" mesajı göster
                devam_et = HAYIR
            DEĞILSE
                // 4c. Zaman çakışması kontrolü
                EĞER zaman_cakisiyor_mu(ders, secilen_dersler) ISE
                    "Ders saatleri çakışıyor" mesajı göster
                    devam_et = HAYIR
                DEĞILSE
                    // 4d. Kredi limiti kontrolü
                    EĞER kredi_limiti_asildi_mi(ders, secilen_dersler) ISE
                        "Kredi limiti aşıldı" mesajı göster
                        devam_et = HAYIR
                    DEĞILSE
                        // 4e. Danışman onayı kontrolü
                        EĞER danisman_onayi_gerekli_mi(ders) ISE
                            EĞER danisman_onayi_alindi_mi(ders) ISE
                                devam_et = EVET
                            DEĞILSE
                                "Danışman onayı bekleniyor" mesajı göster
                                devam_et = HAYIR

        // Eğer tüm kontroller geçildiyse
        EĞER devam_et = EVET ISE
            secilen_dersler.add(ders)
            "Ders seçildi" mesajı göster

// 5. Dersleri onaylama
EĞER secilen_dersler boş değil ISE
    dersleri_onayla(secilen_dersler)
    "Ders kaydı tamamlandı" mesajı göster
DEĞILSE
    "Ders kaydı tamamlanamadı" mesajı göster

BITIR
