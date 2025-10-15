Öğrenci no: 250541023
Öğrenci ad soyad :Ezgi Efsa Güleç


# ATM PARA ÇEKME SİSTEMİ - SÖZDE KOD (TEK SÜTUN)

BASLA  
YAZ "Sisteme hoş geldiniz."  
OKU "Kart takıldı mı? (E/H)" → kart_var  
EĞER-İSE (kart_var == "H")  
 YAZ "Lütfen kartınızı takınız. İşlem sonlandırıldı."  
 BİTİR  
SON  

stored_PIN ← "1234"  
account_balance ← 5000.00  
daily_limit ← 2000.00  
daily_withdrawn ← 0.00  
max_pin_attempts ← 3  
pin_attempts ← 0  
pin_ok ← HAYIR  

DÖNGÜ (pin_attempts < max_pin_attempts) BAŞLA  
 OKU "Lütfen PIN kodunuzu girin:" → entered_PIN  
 pin_attempts ← pin_attempts + 1  
 EĞER-İSE (entered_PIN == stored_PIN)  
  pin_ok ← EVET  
  YAZ "PIN doğrulandı."  
  KIR  
 DEĞİLSE  
  YAZ "Hatalı PIN. Kalan hakkınız: ", max_pin_attempts - pin_attempts  
 SON  
SON DÖNGÜ  

EĞER-İSE (pin_ok == HAYIR)  
 YAZ "3 kez hatalı PIN girildi. Kart bloke edildi."  
 BİTİR  
SON  

TEKRAR ← EVET  

DÖNGÜ (TEKRAR == EVET) BAŞLA  
 YAZ "Bakiyeniz: ", account_balance  
 YAZ "Günlük kalan limit: ", daily_limit - daily_withdrawn  
 OKU "Çekmek istediğiniz tutarı girin (20 TL katları olmalı):" → tutar  

 EĞER-İSE (tutar % 20 != 0)  
  YAZ "Tutar 20 TL’nin katı olmalıdır."  
 DEĞİLSE  
  EĞER-İSE (tutar > account_balance)  
   YAZ "Yetersiz bakiye."  
  DEĞİLSE  
   EĞER-İSE (daily_withdrawn + tutar > daily_limit)  
    YAZ "Günlük limit aşıldı."  
   DEĞİLSE  
    account_balance ← account_balance - tutar  
    daily_withdrawn ← daily_withdrawn + tutar  
    YAZ "Lütfen paranızı alınız."  
    YAZ "Kalan bakiye: ", account_balance  
   SON  
  

 OKU "Başka işlem yapmak ister misiniz? (E/H)" → TEKRAR  
SON DÖNGÜ  

YAZ "İyi günler dileriz."  
BİTİR  
