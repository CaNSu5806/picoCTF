#English

## Challenge Overview

In this challenge, the goal was to recover a hidden flag embedded inside a **JPG** image. Since the challenge involved hiding information within an image, the first step was to inspect the file for metadata and other potential clues.

## Analysis

I began by examining the image metadata using **ExifTool**.

```bash
exiftool img.jpg
```

Among the metadata fields, I found a **Base64-encoded** string stored in the **Comment** field.

To decode it, I used:

```bash
echo -n "encoded_text" | base64 -d
```

The decoded output revealed **another Base64-encoded string**. After decoding this second layer with the same command, I obtained the password required for **steghide**.

## Solution

With the password recovered, I extracted the hidden file from the image using the following command:

```bash
steghide extract -sf img.jpg
```

### Command Breakdown

- **extract** → Extracts hidden data from the stego file.
- **-s / -sf (stego file)** → Specifies the image containing the hidden data.
- **-f (file)** → Indicates that a filename follows.

After entering the recovered password, **steghide** extracted a file named **flag.txt**.

Finally, I displayed its contents with:

```bash
cat flag.txt
```

The output contained the challenge flag.

---

## 💡 Key Takeaway

This challenge highlights the fundamentals of **Steganography**, the practice of hiding information inside another file.

- **ExifTool** is useful for inspecting image metadata and discovering hidden clues.
- Information may be encoded in multiple layers, requiring repeated decoding.
- **Steghide** is a popular steganography tool for embedding and extracting hidden files from images and audio files.
- Always inspect both the **metadata** and the **embedded content** when analyzing suspicious files.

---

# 🇹🇷 Türkçe

## Challenge Özeti

Bu labda amaç, bir **JPG** dosyasının içerisine gizlenmiş olan flag'i ortaya çıkarmaktı. Görsel dosyalarının içerisine veri gizleme yöntemi **Steganografi** olarak adlandırılır. Bu nedenle ilk adım olarak dosyanın metadata bilgilerini inceleyerek olası ipuçlarını aradım.

## Analiz

İlk olarak görselin metadata bilgilerini **ExifTool** ile inceledim.

```bash
exiftool img.jpg
```

Metadata içerisindeki **Comment** alanında **Base64** ile kodlanmış bir metin bulunduğunu fark ettim.

Bu metni çözmek için aşağıdaki komutu kullandım:

```bash
echo -n "encoded_text" | base64 -d
```

İlk çözümleme sonucunda tekrar **Base64** ile kodlanmış ikinci bir metin elde ettim. Aynı işlemi bir kez daha uygulayarak **steghide** tarafından istenen parolaya ulaştım.

## Çözüm

Bulduğum parolayı kullanarak görselin içine gizlenmiş dosyayı çıkarmak için şu komutu çalıştırdım:

```bash
steghide extract -sf img.jpg
```

### Komutun Açıklaması

- **extract** → Görsel içerisine gizlenmiş veriyi çıkarır.
- **-s / -sf (stego file)** → Gizli veriyi barındıran dosyayı belirtir.
- **-f (file)** → Ardından bir dosya adının geleceğini ifade eder.

Parolayı girdikten sonra **steghide**, **flag.txt** isimli dosyayı oluşturdu.

Son olarak dosyanın içeriğini görüntülemek için şu komutu kullandım:

```bash
cat flag.txt
```

Komutun çıktısında challenge'ın flag'i yer alıyordu.

---

## 💡 Çıkarım

Bu challenge, **Steganografi** yönteminin temel mantığını göstermektedir.

- **ExifTool**, görsellerin metadata bilgilerini inceleyerek gizlenmiş ipuçlarını bulmak için oldukça kullanışlıdır.
- Kodlanmış veriler birden fazla katmandan oluşabilir; bu nedenle tek bir çözümleme yeterli olmayabilir.
- **Steghide**, görsel ve ses dosyalarına veri gizlemek veya bu verileri çıkarmak için yaygın olarak kullanılan bir araçtır.
- Şüpheli dosyaları analiz ederken yalnızca dosyanın kendisini değil, **metadata** bilgilerini ve içerisine gömülü verileri de incelemek önemlidir.
