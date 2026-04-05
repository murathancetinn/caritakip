# Cari Takip Programi

Bu proje tek dosyali bir cari takip uygulamasidir. Artik Supabase ile kullanici girisi yapabilir, verileri bulutta saklayabilir ve GitHub Pages uzerinden yayinlanabilir.

## Ne Hazirlandi

- `index.html`
  Uygulamanin tum arayuzu, cari takip mantigi, Supabase giris ekrani ve bulut senkronu burada bulunur.
- `supabase-config.js`
  Supabase `url` ve `anon key` bilgilerini buraya gireceksiniz.
- `supabase-config.example.js`
  Ornek ayar dosyasi.
- `supabase-schema.sql`
  Supabase SQL Editor icinde calistiracaginiz tablo ve RLS politikalarini icerir.

## Yayin Mimarisinin Mantigi

Bu kurulumda dogrudan GitHub ile Supabase arasinda ozel bir bag kurmuyorsunuz. Akis su sekilde calisir:

1. Kod GitHub deposunda tutulur.
2. GitHub Pages `index.html` dosyasini internetten yayinlar.
3. Tarayicida acilan uygulama Supabase'e baglanir.
4. Kullanici e-posta ve sifre ile giris yapar.
5. Her kullanicinin verisi Supabase tablosunda kendi `user_id` degeri altinda saklanir.

Yani:

- GitHub: kodu barindirir
- Supabase: giris ve veritabani katmani olur
- GitHub Pages: uygulamayi yayinlar

## Sirayla Yapilacaklar

### 1. GitHub deposunu hazirla

Bu klasor zaten bir git deposu. GitHub'da yeni bir repo olusturun ve bu klasoru ona gonderin.

Ornek:

```bash
git remote add origin REPO_URL
git branch -M main
git add .
git commit -m "Supabase cloud sync added"
git push -u origin main
```

### 2. Supabase projesi olustur

1. [Supabase](https://supabase.com) uzerinde yeni proje olusturun.
2. Proje acildiktan sonra su sayfalari kullanacaksiniz:
   - `Project Settings > API`
   - `Authentication > Providers`
   - `SQL Editor`

### 3. Veritabani tablosunu olustur

`supabase-schema.sql` dosyasinin tamamini Supabase `SQL Editor` icine yapistirin ve calistirin.

Bu dosya su isi yapar:

- `app_user_data` tablosunu olusturur
- Her kullaniciya sadece kendi verisini okuma/yazma izni verir

### 4. E-posta sifre girisini ac

Supabase panelinde:

1. `Authentication > Providers > Email`
2. `Enable Email provider` acik olsun
3. Isterseniz e-posta dogrulamayi acik birakabilirsiniz

Not:

- E-posta dogrulamayi acik birakirsaniz yeni kullanici olusturduktan sonra mail onayi gerekir
- Testi hizli yapmak istiyorsaniz gecici olarak mail onayini kapatabilirsiniz

### 5. API bilgilerini projeye gir

Supabase panelinde:

1. `Project Settings > API`
2. Su iki bilgiyi alin:
   - `Project URL`
   - `anon public key`

Sonra bu projedeki `supabase-config.js` dosyasini acin ve doldurun:

```js
window.SUPABASE_CONFIG = {
  url: "https://YOUR-PROJECT.supabase.co",
  anonKey: "YOUR_SUPABASE_ANON_KEY"
};
```

Onemli:

- Buraya `service_role` key koymayin
- Sadece `anon` key koyun
- `anon` key istemci tarafinda kullanilmak icin tasarlanmistir

### 6. Kodu tekrar GitHub'a gonder

```bash
git add .
git commit -m "Configure Supabase project"
git push
```

### 7. GitHub Pages ile yayinla

GitHub repo ayarlarinda:

1. `Settings > Pages`
2. `Source` olarak `Deploy from a branch`
3. Branch olarak `main`
4. Folder olarak `/ (root)`
5. Kaydedin

Birkac dakika sonra GitHub size bir yayin linki verecek:

```text
https://kullaniciadi.github.io/repo-adi/
```

## Uygulamada Nasil Kullanacaksiniz

### Ilk cihazda

1. Uygulamayi acin
2. `Bulut Girisi` butonuna basin
3. E-posta ve sifre ile `Hesap Olustur`
4. Gerekirse e-posta onayi yapin
5. Sonra `Giris Yap`
6. Verileri kullanmaya baslayin

Bu andan sonra kayitlar Supabase'e yazilir.

### Diger cihazda

1. Ayni GitHub Pages linkini acin
2. Ayni e-posta ve sifre ile giris yapin
3. Veriler otomatik buluttan yuklenir

## Uygulamanin Yeni Bulut Davranisi

- Kullanici giris yapmadiysa uygulama yerel modda calisir
- Kullanici giris yaptiysa veri kaynagi bulut olur
- Yerelde de onbellek tutulur
- Cikis yapildiginda yerel gorunum temizlenir

## Kod Icinde Ne Degisti

- `index.html`
  Supabase istemcisi, auth paneli, giris/hesap olusturma/cikis ve otomatik bulut kayit akisi eklendi.
- `saveData()`
  Artik hem local cache hem de bulut senkronu icin kullaniliyor.
- `loadData()`
  Yerel veriyi aciyor, sonra Supabase oturumu varsa kullaniciya ait veriyi cekiyor.

## Supabase Tablo Yapisi

Tablo:

- `app_user_data`

Alanlar:

- `user_id`
  Supabase auth kullanici kimligi
- `data`
  Tum uygulama verisinin JSON hali
- `updated_at`
  Son guncelleme tarihi

Bu yapinin avantaji:

- Mevcut tek dosyali uygulamayi hizli sekilde buluta tasir
- Tum uygulama durumunu tek kayit olarak saklar
- Ilk asamada kompleks tablo tasarimi gerektirmez

## Ben Simdi Ne Yapmaliyim

Asagidaki sirayi birebir takip et:

1. Supabase'de yeni proje ac
2. `supabase-schema.sql` dosyasini SQL Editor'de calistir
3. Email auth'u ac
4. `Project URL` ve `anon key` bilgisini kopyala
5. `supabase-config.js` dosyasina yapistir
6. Degisiklikleri GitHub'a push et
7. GitHub Pages'i ac
8. Yayindaki linkten uygulamayi ac
9. Hesap olustur
10. Giris yap
11. Baska cihazdan ayni hesapla giris yapip test et

## Test Senaryosu

Yayin sonrasi su testi yapin:

1. Bir cihazdan giris yapin
2. Yeni cari ekleyin
3. Bir is girin
4. Bir odeme girin
5. Tarayiciyi kapatin
6. Baska cihazdan ayni hesapla giris yapin
7. Ayni cari ve kayitlar gorunuyor mu kontrol edin

## Sonraki Asamada Yapilabilecek Iyilestirmeler

Isterseniz ikinci asamada sunlari da yapabiliriz:

- Rol bazli kullanici sistemi
- Ortak firma ici birden fazla kullanici
- Supabase Storage ile otomatik yedek dosyalari
- PDF ve Excel raporlarini sunucu destekli hale getirme
- Olay bazli audit log
- Gercek coklu tablo muhasebe veritabani yapisi

## Not

Bu yapida `supabase-config.js` dosyasindaki `anon key` gizli degildir ve istemci tarafinda kullanilabilir. Gizli tutulmasi gereken tek anahtar `service_role` key'dir; onu kesinlikle bu projeye eklemeyin.
