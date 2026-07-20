# 🛠️ Write-Up

## 🇬🇧 English

### Challenge Overview

The challenge provided a corrupted image file that could not be opened normally. Instead of assuming the file was completely damaged, I examined its file structure to determine whether the issue was related to the file header.

### Analysis

Every file format starts with a unique sequence of bytes known as a **File Signature** or **Magic Bytes**. These bytes allow operating systems and applications to identify the file type.

A standard **JPEG** image must always begin with the following hexadecimal bytes:

```text
FF D8
```

However, the provided file started with:

```text
5C 78
```

Since the file signature was invalid, image viewers could not recognize it as a JPEG file.

### Solution

I opened the file using an online **Hex Editor** and replaced the incorrect starting bytes:

```text
5C 78  →  FF D8
```

After saving the changes, the file became a valid JPEG image and opened successfully. The hidden flag was then visible inside the image.

---

## 💡 Key Takeaway

This technique is not limited to JPEG files. Almost every file format has its own unique file signature.

| File Type | Magic Bytes |
|-----------|-------------|
| JPEG | `FF D8` |
| PNG | `89 50 4E 47` |
| PDF | `25 50 44 46` |
| ZIP | `50 4B 03 04` |

To quickly inspect the first few bytes of a file in Linux, you can use:

```bash
head -c 16 filename | xxd
```

This command displays the first **16 bytes** of the file in hexadecimal format, making it easy to verify the file signature.

---

# 🇹🇷 Türkçe

## Challenge Özeti

Bu labda bize normal şekilde açılamayan bozuk bir dosya verildi. Dosyanın tamamen bozulduğunu varsaymak yerine, ilk olarak dosya yapısını inceleyerek sorunun dosya başlığından (header) kaynaklanıp kaynaklanmadığını kontrol ettim.

## Analiz

Her dosya türü, **File Signature (Dosya İmzası)** veya **Magic Bytes** adı verilen kendine özgü birkaç bayt ile başlar. İşletim sistemi ve uygulamalar, bir dosyanın hangi formatta olduğunu bu baytlara bakarak belirler.

Standart bir **JPEG** dosyası her zaman şu baytlarla başlar:

```text
FF D8
```

Ancak verilen dosyanın başlangıcında şu değerler bulunuyordu:

```text
5C 78
```

Bu nedenle işletim sistemi dosyayı bir JPEG olarak tanıyamıyor ve görsel açılamıyordu.

## Çözüm

Dosyayı bir **Hex Editor** ile açarak başlangıçtaki hatalı baytları düzelttim.

```text
5C 78  →  FF D8
```

Değişikliği kaydettikten sonra dosya başarıyla bir JPEG görseli olarak açıldı ve içerisinde bulunan flag'e ulaştım.

---

## 💡 Çıkarım

Bu yöntem yalnızca JPEG dosyaları için geçerli değildir. Hemen hemen tüm dosya türlerinin kendilerine ait benzersiz bir **Magic Byte** dizisi bulunur.

| Dosya Türü | Magic Bytes |
|------------|-------------|
| JPEG | `FF D8` |
| PNG | `89 50 4E 47` |
| PDF | `25 50 44 46` |
| ZIP | `50 4B 03 04` |

Linux üzerinde bir dosyanın ilk birkaç baytını hızlıca incelemek için şu komut kullanılabilir:

```bash
head -c 16 dosya_adi | xxd
```

Bu komut, dosyanın ilk **16 baytını** hexadecimal formatta göstererek dosya imzasını kolayca kontrol etmenizi sağlar.
