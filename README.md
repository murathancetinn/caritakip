# Cari Takip Programi

Bu proje artik Supabase ile giris yapabilen, verileri bulutta saklayabilen ve GitHub Pages uzerinden yayinlanabilen bir cari takip uygulamasidir.

Bu README, hicbir sey bilmiyormussun gibi yazildi. Adim adim ilerlersen kurarsin.

## Sistem Nasil Calisiyor

Burada 3 ayri parca var:

1. GitHub
   Kodunu tutar.
2. GitHub Pages
   `index.html` dosyasini internette yayinlar.
3. Supabase
   Kullanici girisi ve veritabani tarafidir.

Yani akisin mantigi su:

- Kod GitHub'da durur
- Site GitHub Pages'te acilir
- Site acilinca Supabase'e baglanir
- Sen e-posta ve sifre ile giris yaparsin
- Verilerin Supabase'te hesabina ozel saklanir

## Bu Projede Hangi Dosya Ne Ise Yariyor

- [index.html](/Users/murathan/Desktop/Adsız/index.html)
  Uygulamanin kendisi.
- [supabase-config.js](/Users/murathan/Desktop/Adsız/supabase-config.js)
  Supabase proje bilgilerini buraya yazacaksin.
- [supabase-config.example.js](/Users/murathan/Desktop/Adsız/supabase-config.example.js)
  Ornek ayar dosyasi.
- [supabase-schema.sql](/Users/murathan/Desktop/Adsız/supabase-schema.sql)
  Supabase veritabani tablosunu ve guvenlik kurallarini olusturan SQL.

## En Onemli Kisim: Supabase API Key Hangisi?

Supabase'de `Project Settings > API` alanina girdiginde birden fazla key goreceksin. Karistirilan kisim burasi.

Supabase'in resmi dokumanina gore:

- `Publishable key`
  Tarayici tarafinda, yani bu proje gibi frontend uygulamalarda kullanilmasi gereken yeni anahtardir.
- `Legacy anon key`
  Eski tip client key'dir. Hala calisabilir ama Supabase yeni projelerde `Publishable key` kullanilmasini oneriyor.
- `Secret key`
  Sadece backend/server icindir. Bu projede kullanilmaz.
- `service_role`
  Sadece backend/server icindir. Bu projede kullanilmaz.

Bu proje icin kullanacagin bilgiler sunlar:

1. `Project URL`
2. `Publishable key`

Almayacaklarin:

- `service_role`
- `Secret key`

Mecbur kalmadikca kullanmayacaklarin:

- `Legacy anon key`

## Kisa Cevap

Senin bu proje icin alman gerekenler:

1. `Project URL`
2. `Publishable key`

Ve bunlari [supabase-config.js](/Users/murathan/Desktop/Adsız/supabase-config.js) dosyasina yazacaksin.

## Supabase'de Tam Olarak Nereye Tiklayacaksin?

### 1. Supabase hesabi ac

[Supabase](https://supabase.com) sitesine gir.

### 2. Yeni proje olustur

1. `New project`
2. Bir organization sec
3. Proje adini gir
4. Database password olustur
5. Region sec
6. `Create new project`

Biraz bekle. Proje hazir olunca dashboard acilacak.

### 3. SQL Editor'den veritabani tablosunu kur

Sol menuden:

1. `SQL Editor`
2. `New query`
3. [supabase-schema.sql](/Users/murathan/Desktop/Adsız/supabase-schema.sql) dosyasinin tamamini kopyala
4. SQL Editor'a yapistir
5. `Run`

Bu islem su tablonun kurulmasini saglar:

- `app_user_data`

Ve her kullanicinin sadece kendi verisini okuyup yazmasini saglayan guvenlik kurallarini da acmis olur.

### 4. E-posta ve sifre ile girisi ac

Sol menuden:

1. `Authentication`
2. `Providers`
3. `Email`

Burada sunlari kontrol et:

- `Enable Email provider` acik olsun

Istersen su secenek olabilir:

- Email confirmation acik ise, yeni hesap olusturunca mail onayi gerekir
- Test icin istersen bunu gecici kapatabilirsin

### 5. API bilgilerini bul

Sol menuden:

1. `Project Settings`
2. `API`

Bu sayfada birden fazla kisim goreceksin.

#### Aradigin bilgi 1: Project URL

Sayfada `Project URL` veya benzer sekmede su tip bir deger olur:

```text
https://abcxyzcompany.supabase.co
```

Bunu kopyala.

#### Aradigin bilgi 2: Publishable key

API ekraninda genelde iki mantik vardir:

- `API Keys`
- `Legacy API Keys`

Senin bakacagin kisim:

- `API Keys` icindeki `Publishable key`

Eger `Publishable key` goruyorsan bunu kopyala.

Bu anahtar genelde sunun gibi baslar:

```text
sb_publishable_...
```

#### Legacy sekmesinde ne var?

`Legacy API Keys` altinda genelde su eski anahtarlar olur:

- `anon`
- `service_role`

Burada:

- `anon`
  Eski istemci anahtari
- `service_role`
  Asla frontend'e koyulmaz

Bu projede tercih edilen:

- `Publishable key`

Yani ozet:

- `Publishable key` al
- `Legacy anon` alma
- `service_role` kesinlikle alma

## Supabase Config Dosyasini Nasil Dolduracaksin?

[supabase-config.js](/Users/murathan/Desktop/Adsız/supabase-config.js) dosyasini ac.

Simdi su sekilde duzenle:

```js
window.SUPABASE_CONFIG = {
  url: "https://SENIN-PROJE.supabase.co",
  anonKey: "sb_publishable_SENIN_ANAHTARIN"
};
```

Burada `anonKey` alaninin ismi eski isim olarak kaldi ama icine yazacagin sey yeni `Publishable key` olacak.

Yani kafa karistiran kisim su:

- dosya icindeki alan adi: `anonKey`
- ama koyacagin deger: `Publishable key`

Bu teknik olarak sorun degil. Alan adini degistirmedik ama deger olarak publishable key kullaniyoruz.

## Ornek

Ornek doldurulmus hali:

```js
window.SUPABASE_CONFIG = {
  url: "https://abcdwxyz.supabase.co",
  anonKey: "sb_publishable_xxxxxxxxxxxxxxxxx"
};
```

## Hangi Key'i Kesinlikle Kullanmamaliyim?

Asagidakileri bu projeye koyma:

- `service_role`
- `secret key`

Sebebi:

- Bunlar tum veritabanina genis yetki verir
- Tarayiciya koyarsan herkes gorebilir
- Bu ciddi guvenlik hatasi olur

## GitHub'a Nasil Yukleyeceksin?

Eger repo hazirsa terminalde bu klasorde su sirayla:

```bash
git add .
git commit -m "Supabase config and cloud auth setup"
git push
```

Eger remote daha once eklenmediyse:

```bash
git remote add origin REPO_URL
git branch -M main
git add .
git commit -m "Initial cloud version"
git push -u origin main
```

## GitHub Pages Nasil Acilir?

GitHub repo sayfasinda:

1. `Settings`
2. Sol menude `Pages`
3. `Source` olarak `Deploy from a branch`
4. Branch olarak `main`
5. Folder olarak `/root`
6. `Save`

Birkac dakika sonra bir link verir.

Ornek:

```text
https://kullaniciadi.github.io/repo-adi/
```

Bu senin canli siten olacak.

## Uygulamada Ilk Giris Nasil Yapilacak?

Yayina aldiktan sonra:

1. Siteyi ac
2. Ustte `Bulut Girisi` butonuna bas
3. E-posta gir
4. Sifre gir
5. `Hesap Olustur`
6. Gerekirse mail onayi yap
7. Sonra `Giris Yap`

Giris yaptiginda:

- veri senin hesabin icin Supabase'e kaydedilir
- diger cihazlardan ayni hesapla acabilirsin

## Diger Cihazda Nasil Acacaksin?

1. Ayni GitHub Pages linkini ac
2. Ayni e-posta ve sifreyi gir
3. `Giris Yap`
4. Verilerin otomatik gelsin

## Kurulum Sonrasi Test

Bu testi yap:

1. Bir cihazdan giris yap
2. Bir cari ekle
3. Bir is kaydi ekle
4. Bir odeme ekle
5. Sayfayi yenile
6. Kayitlar duruyor mu kontrol et
7. Baska cihazdan ayni hesaba gir
8. Ayni kayitlar geliyor mu kontrol et

## Bir Sorun Varsa En Olası Sebepler

### 1. Publishable key yerine yanlis key koydun

Dogrusu:

- `Publishable key`

Yanlisi:

- `service_role`
- `secret`

### 2. SQL calistirilmadi

[supabase-schema.sql](/Users/murathan/Desktop/Adsız/supabase-schema.sql) dosyasi SQL Editor'de calismadiysa tablo yoktur.

### 3. Email provider acik degil

`Authentication > Providers > Email` acik olmali.

### 4. GitHub Pages eski surumu gosterebilir

Son commit'in yayinlandigindan emin ol.

## Bu Projede Simdilik Veriler Nasil Saklaniyor?

Her kullanici icin Supabase'te tek bir JSON kaydi saklaniyor.

Avantajlari:

- hizli kurulur
- mevcut uygulamayi bozmadan buluta tasir
- tum cihazlarda ayni veri acilir

Ileride istersek bunu daha profesyonel tablo yapiya da tasiyabiliriz:

- `customers`
- `transactions`
- `expenses`
- `closures`
- `archives`

Ama ilk asamada buna gerek yok.

## Son Ozet

Senin yapacagin en kritik 5 sey:

1. Supabase proje ac
2. [supabase-schema.sql](/Users/murathan/Desktop/Adsız/supabase-schema.sql) calistir
3. `Authentication > Providers > Email` ac
4. `Project Settings > API` icinden:
   - `Project URL`
   - `Publishable key`
   al
5. Bunlari [supabase-config.js](/Users/murathan/Desktop/Adsız/supabase-config.js) dosyasina yaz

## Resmi Kaynaklar

- [Understanding API keys - Supabase Docs](https://supabase.com/docs/guides/api/api-keys)
- [REST API - Supabase Docs](https://supabase.com/docs/guides/api)

Resmi dokumana gore istemci tarafinda `Publishable key` kullanilmasi oneriliyor; `anon` legacy olarak geciyor ve `service_role` istemci tarafinda kullanilmamali.
