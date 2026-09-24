# 🖥️ Arch Linux Terminal Özelleştirme Rehberi

Bu rehberde Arch Linux terminalini daha modern ve kullanışlı hale getireceğiz.

Yaptığımız başlıca değişiklikler:

- ⭐ **Starship** ile modern terminal prompt'u
- 👤 Kullanıcı ve hostname görünümü
- 📁 Bulunduğun klasörün gösterimi
- ❯ Başarılı/başarısız komut göstergesi
- 🌳 **eza** ile daha düzenli dosya listeleme
- 📄 **glow** ile Markdown dosyalarını terminalde görüntüleme
- 👑 Root kullanıcısı için ayrı Starship ayarı

---

## 📋 İçindekiler

1. [Starship kurulumu](#1-starship-kurulumu)
2. [Bash'e Starship ekleme](#2-bashe-starship-ekleme)
3. [Starship görünümünü özelleştirme](#3-starship-görünümünü-özelleştirme)
4. [Rust ikonunu koruma](#4-rust-ikonunu-koruma)
5. [Root için Starship](#5-root-için-starship)
6. [eza kurulumu](#6-eza-kurulumu)
7. [eza ile klasörleri düzenli gösterme](#7-eza-ile-klasörleri-düzenli-gösterme)
8. [glow ile Markdown görüntüleme](#8-glow-ile-markdown-görüntüleme)

---

# 1. Starship Kurulumu

Starship, terminaldeki klasik:

```text
[yasir@archlinux ~]$
```

görünümünü daha modern ve özelleştirilebilir bir hale getirir.

Kurulum:

```bash
sudo pacman -S starship
```

Kurulumdan sonra kontrol etmek için:

```bash
starship --version
```

Örneğin:

```text
starship 1.26.0
```

gibi bir çıktı görebilirsin.

---

# 2. Bash'e Starship Ekleme

Starship'in Bash ile otomatik başlaması için `~/.bashrc` dosyasına aşağıdaki satırı ekliyoruz:

```bash
eval "$(starship init bash)"
```

Bunu otomatik eklemek için:

```bash
echo 'eval "$(starship init bash)"' >> ~/.bashrc
```

Ardından `.bashrc` dosyasını yeniden yükle:

```bash
source ~/.bashrc
```

Artık Starship aktif olacaktır.

---

# 3. Starship Görünümünü Özelleştirme

Starship ayar dosyamız:

```text
~/.config/starship.toml
```

Önce klasörü oluştur:

```bash
mkdir -p ~/.config
```

Ardından:

```bash
nano ~/.config/starship.toml
```

Dosyanın içine:

```toml
add_newline = false

format = "$username$hostname $directory$character"

[username]
show_always = true
format = "[ITechYou]($style)@"

[hostname]
ssh_only = false
format = "[$hostname]($style)"

[directory]
format = "[$path]($style) "

[character]
success_symbol = "[❯](bold green)"
error_symbol = "[❯](bold red)"
```

yazıyoruz.

Kaydettikten sonra:

```bash
source ~/.bashrc
```

---

## 🎨 Sonuç

Terminal artık yaklaşık olarak:

```text
ITechYou@archlinux ~/Projects ❯
```

şeklinde görünür.

Komut başarılıysa:

```text
ITechYou@archlinux ~/Projects ❯
```

başarısızsa:

```text
ITechYou@archlinux ~/Projects ❯
```

`❯` sembolünün rengi başarı/başarısız duruma göre değişir.

---

# 4. Rust İkonunu Korumak

Starship'in varsayılan modüllerinden bazılarını kendi `format` satırımızı yazdığımız için kaybedebiliriz.

Örneğin Rust ikonunun görünmesini istiyorsak:

```toml
format = "$username$hostname $directory$rust$character"
```

kullanabiliriz.

Tam örnek:

```toml
add_newline = false

format = "$username$hostname $directory$rust$character"

[username]
show_always = true
format = "[ITechYou]($style)@"

[hostname]
ssh_only = false
format = "[$hostname]($style)"

[directory]
format = "[$path]($style) "

[character]
success_symbol = "[❯](bold green)"
error_symbol = "[❯](bold red)"
```

### ⚠️ Rust ikonu neden her zaman görünmez?

Starship, Rust modülünü yalnızca Rust projesi algıladığında gösterir.

Örneğin bir klasörde:

```text
Cargo.toml
```

varsa Rust modülü aktif olabilir.

Normal bir klasörde Rust ikonu görünmemesi normaldir.

---

# 5. Root İçin Starship

Normal kullanıcı ile root kullanıcısının `HOME` dizini farklıdır.

Normal kullanıcı:

```text
/home/yasir
```

Root:

```text
/root
```

Bu yüzden normal kullanıcıya yaptığımız Starship ayarı root'a otomatik uygulanmaz.

Root olarak Starship kurulumu:

```bash
sudo pacman -S starship
```

Root'un `.bashrc` dosyasına Starship'i eklemek için:

```bash
sudo sh -c 'echo '\''eval "$(starship init bash)"'\'' >> /root/.bashrc'
```

Sonra root shell aç:

```bash
sudo -i
```

ve:

```bash
source ~/.bashrc
```

Root için Starship ayar dosyası:

```text
/root/.config/starship.toml
```

Klasörü oluştur:

```bash
mkdir -p /root/.config
```

Sonra:

```bash
nano /root/.config/starship.toml
```

Normal kullanıcıdaki yapılandırmayı burada da kullanabilirsin.

---

# 6. eza Kurulumu

Klasik:

```bash
ls
```

komutu yerine daha modern bir dosya listeleme aracı olan **eza** kullanabiliriz.

Kurulum:

```bash
sudo pacman -S eza
```

Kurulumdan sonra:

```bash
eza --version
```

ile kontrol edebilirsin.

---

# 7. eza ile Klasörleri Düzenli Gösterme

Örneğin:

```bash
eza -l --icons --group-directories-first --time-style=long-iso
```

komutu ile dosyaları daha düzenli gösterebiliriz.

Parametrelerin anlamı:

| Parametre | Görevi |
|---|---|
| `-l` | Ayrıntılı liste |
| `--icons` | Dosya/klasör ikonları |
| `--group-directories-first` | Klasörleri üstte gösterir |
| `--time-style=long-iso` | Tarih/saat formatını düzenler |

Örneğin terminalde:

```text
📁 Projects
📁 Downloads
📁 Documents
📄 README.md
📄 config.toml
```

gibi daha okunabilir bir görünüm elde edebilirsin.

---

## `ls` Yerine eza Kullanmak

İstersen `ls` komutunu eza ile değiştirebilirsin.

`~/.bashrc` dosyasını aç:

```bash
nano ~/.bashrc
```

Sonuna:

```bash
alias ls='eza --icons --group-directories-first'
alias ll='eza -l --icons --group-directories-first --time-style=long-iso'
```

ekle.

Sonra:

```bash
source ~/.bashrc
```

Artık:

```bash
ls
```

yazdığında eza çalışır.

Daha ayrıntılı liste için:

```bash
ll
```

kullanabilirsin.

---

# 8. glow ile Markdown Görüntüleme

Terminal üzerinde `.md` dosyalarını daha güzel görüntülemek için **glow** kullanabiliriz.

Kurulum:

```bash
sudo pacman -S glow
```

Örneğin elimizde:

```text
README.md
```

varsa:

```bash
glow README.md
```

çalıştırabiliriz.

Markdown dosyası terminal içerisinde:

- başlıklar
- listeler
- kod blokları
- tablolar
- bağlantılar

gibi Markdown biçimlendirmeleriyle okunabilir.

---

# 🎯 Sonuç

Bu işlemlerden sonra terminalimiz:

```text
ITechYou@archlinux ~/Projects ❯
```

gibi modern bir prompt'a sahip olur.

Dosyaları:

```bash
ls
```

veya:

```bash
ll
```

ile ikonlu ve düzenli şekilde listeleyebiliriz.

Markdown dosyalarını ise:

```bash
glow README.md
```

ile doğrudan terminal içerisinde okuyabiliriz.

---

# 📁 Kullanılan Dosyalar

Yaptığımız özelleştirmelerde temel olarak şu dosyaları kullandık:

```text
~/.bashrc
~/.config/starship.toml
```

Root için:

```text
/root/.bashrc
/root/.config/starship.toml
```

---

# 🔄 Ayarları Yenileme

`.bashrc` üzerinde değişiklik yaptıktan sonra:

```bash
source ~/.bashrc
```

Starship ayarlarını değiştirdikten sonra da yeni terminal açabilir veya:

```bash
source ~/.bashrc
```

çalıştırabilirsin.

---

# 🧹 Değişiklikleri Geri Alma

Eğer yaptığımız alias'ları kaldırmak istersen:

```bash
nano ~/.bashrc
```

içerisindeki:

```bash
alias ls='eza --icons --group-directories-first'
alias ll='eza -l --icons --group-directories-first --time-style=long-iso'
```

satırlarını silebilirsin.

Starship'i devre dışı bırakmak için de:

```bash
nano ~/.bashrc
```

içerisindeki:

```bash
eval "$(starship init bash)"
```

satırını kaldırabilirsin.

---

## 💡 Öneri

Bu rehberi kendi GitHub/kişisel dokümantasyon sayfanda kullanıyorsan, ileride buraya **Neofetch/Fastfetch, Zsh, Oh My Zsh, terminal renkleri, Git prompt'u ve özel alias'lar** gibi bölümler de ekleyebilirsin.
