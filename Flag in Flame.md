# English

## Challenge Overview

In this challenge, I was provided with a large **logs.txt** file collected by the SOC team. The objective was to analyze the file, uncover any hidden information, and recover the flag.

## Analysis

After examining the contents of the file, I noticed that the data was **Base64 encoded**. Rather than attempting to read it directly, I first decoded it into a new file.

```bash
base64 -d logs.txt > output.txt
```

Once the decoding process was complete, I inspected the output.

```bash
cat output.txt
```

The resulting data was not plain text. Instead, the file contained the signature of a **PNG image**, indicating that the decoded output was actually an image rather than textual log data.

## Solution

After opening the PNG image, I noticed a string written in **Hexadecimal** format.

To decode it, I used **CyberChef** and applied the **From Hex** operation. This converted the hexadecimal string into readable text, revealing the final flag.

---

## 💡 Key Takeaway

This challenge demonstrates the importance of recognizing common encoding and file signatures during forensic analysis.

- **Base64** is an encoding method, not encryption.
- Decoded output is not always plain text—it may represent another file format.
- Identifying **file signatures (magic bytes)** is an effective way to determine the real type of unknown files.
- **CyberChef** is a powerful tool for decoding and transforming encoded or formatted data during forensic investigations.

---

# 🇹🇷 Türkçe

## Challenge Özeti

Bu labda, SOC ekibi tarafından sağlanan büyük bir **logs.txt** dosyasını analiz ederek gizlenmiş bilgileri ortaya çıkarmamız ve flag'i elde etmemiz istendi.

## Analiz

Dosyanın içeriğini incelediğimde verilerin **Base64** ile kodlandığını fark ettim. Bu nedenle ilk olarak veriyi çözüp yeni bir dosyaya aktardım.

```bash
base64 -d logs.txt > output.txt
```

Ardından oluşan dosyayı inceledim.

```bash
cat output.txt
```

Çıktı normal bir metin değildi. Dosyanın başlangıcındaki imza (magic bytes), bunun aslında bir **PNG görseli** olduğunu gösteriyordu. Yani Base64 verisi çözüldüğünde ortaya bir görsel dosyası çıkıyordu.

## Çözüm

PNG dosyasını açtığımda üzerinde **Hexadecimal** formatında yazılmış bir metin bulunduğunu gördüm.

Bu metni **CyberChef** platformuna aktararak **From Hex** işlemini uyguladım. Hexadecimal verisi çözüldüğünde okunabilir metne dönüştü ve böylece nihai flag'e ulaştım.

---

## 💡 Çıkarım

Bu challenge, dijital adli analiz sırasında farklı kodlama yöntemlerini ve dosya imzalarını tanımanın önemini göstermektedir.

- **Base64**, bir şifreleme yöntemi değil, yalnızca bir kodlama (encoding) yöntemidir.
- Decode edilen veriler her zaman düz metin olmayabilir; başka bir dosya türünü temsil edebilir.
- **Magic Bytes (Dosya İmzaları)**, bilinmeyen dosyaların gerçek formatını belirlemede oldukça faydalıdır.
- **CyberChef**, adli analizlerde farklı veri formatlarını çözmek ve dönüştürmek için oldukça güçlü bir araçtır.
