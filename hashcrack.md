# MD5 Password Hash Analysis and Recovery

## 🇬🇧 English

### Challenge Goal

In this challenge, I encountered a system where access depended on recovering a password stored as a **cryptographic hash**.

The objective was to identify the hashing algorithm and determine whether the original plaintext password could be recovered from the provided hash.

> **Note:** Hashing and encryption are different concepts. Encryption is designed to be reversible with the correct key, while hashing is designed to be a **one-way operation**.

---

### Identifying the Hash

The first step was to determine which hashing algorithm had been used.

I analyzed the provided hash using tools such as `hashid`, which identifies possible hash types based on characteristics such as their length and format.

The analysis indicated that the hash was likely:

```text
MD5 (Message Digest 5)
```

MD5 produces a **128-bit (16-byte)** digest, commonly represented as a **32-character hexadecimal string**.

For example:

```text
5f4dcc3b5aa765d61d8327deb882cf99
```

---

### Why MD5 Is Insecure for Password Storage

MD5 was historically used for integrity checking and other cryptographic purposes, but it is now considered **cryptographically broken** and unsuitable for password storage.

There are two important problems:

- **Collision weaknesses:** Different inputs can be constructed to produce the same MD5 hash.
- **High computation speed:** MD5 is extremely fast, allowing attackers to test very large numbers of password candidates efficiently.

For password storage, speed is actually a disadvantage. Modern password-hashing algorithms are intentionally designed to make large-scale guessing attempts computationally expensive.

---

### Password Recovery

After identifying the hash as MD5, I checked it using **CrackStation**, which maintains a large database of previously computed and cracked hash values.

Instead of mathematically reversing MD5, the service attempts to find a known plaintext value whose MD5 hash matches the supplied hash.

Conceptually, the process looks like this:

```text
Provided MD5 Hash
        ↓
Search Against Known Hashes
        ↓
Matching Hash Found
        ↓
Associated Plaintext Password
```

Because the password/hash combination already existed in the database, the plaintext password could be recovered quickly.

---

### The Importance of Salting

One of the major weaknesses in this challenge was that the password hash was **unsalted**.

A salt is a unique random value combined with a password before it is hashed.

Without a salt:

```text
password123 → MD5 → Hash A
password123 → MD5 → Hash A
password123 → MD5 → Hash A
```

The same password repeatedly produces the same hash.

With unique salts:

```text
password123 + Salt A → Hash X
password123 + Salt B → Hash Y
password123 + Salt C → Hash Z
```

Even though the underlying password is identical, the resulting hashes are different.

This makes precomputed hash databases significantly less effective.

---

### What I Learned

This challenge demonstrated why **fast general-purpose hash functions such as MD5 should never be used directly for password storage**.

The overall process was:

```text
Provided Hash
     ↓
Hash Identification
     ↓
MD5 Detected
     ↓
Hash Lookup
     ↓
Plaintext Password Recovered
     ↓
Server Access
```

The challenge also highlighted the importance of using modern password-hashing algorithms such as **Argon2id, bcrypt, scrypt, or PBKDF2**, together with a unique salt for each password.

---

## 🇹🇷 Türkçe

### Challenge'ın Amacı

Bu challenge'da erişimin bir **kriptografik hash** olarak saklanan parola ile korunduğu bir sistemle karşılaştım.

Amacım, verilen hash'in hangi algoritmayla oluşturulduğunu tespit etmek ve orijinal düz metin parolanın elde edilip edilemeyeceğini araştırmaktı.

> **Not:** Hashleme ve şifreleme (encryption) aynı şey değildir. Şifreleme doğru anahtar kullanılarak geri döndürülebilirken, hashleme temelde **tek yönlü** olacak şekilde tasarlanmıştır.

---

### Hash Türünün Tespit Edilmesi

İlk olarak verilen hash'in hangi algoritmaya ait olabileceğini belirlemem gerekiyordu.

Bunun için `hashid` gibi araçlardan yararlanarak hash'in uzunluğunu ve biçimini analiz ettim.

Analiz sonucunda hash'in büyük ihtimalle:

```text
MD5 (Message Digest 5)
```

olduğunu belirledim.

MD5, **128 bitlik (16 byte)** bir hash değeri üretir ve bu değer genellikle **32 karakterlik hexadecimal** bir ifade olarak gösterilir.

Örneğin:

```text
5f4dcc3b5aa765d61d8327deb882cf99
```

---

### MD5 Parola Saklamak İçin Neden Güvensizdir?

MD5 geçmişte veri bütünlüğü kontrolü ve çeşitli kriptografik işlemler için yaygın olarak kullanılmış olsa da günümüzde **kriptografik olarak kırılmış** kabul edilmektedir ve parola saklamak için uygun değildir.

Bunun iki önemli nedeni vardır:

- **Collision (çakışma) zayıflıkları:** Farklı girdilerin aynı MD5 hash değerini üretmesine yönelik bilinen saldırılar bulunmaktadır.
- **Çok hızlı çalışması:** MD5'in hesaplanması son derece hızlıdır. Bu nedenle çok sayıda parola tahmini kısa sürede test edilebilir.

Parola saklama söz konusu olduğunda algoritmanın hızlı olması bir avantaj değil, güvenlik problemidir. Modern parola hashleme algoritmaları, çok sayıda tahmin yapmayı maliyetli hale getirmek amacıyla özellikle daha yavaş çalışacak şekilde tasarlanır.

---

### Parolanın Elde Edilmesi

Hash türünü MD5 olarak belirledikten sonra hash değerini **CrackStation** üzerinde kontrol ettim.

CrackStation, daha önce hesaplanmış veya kırılmış çok sayıda hash ve bunlarla ilişkili düz metin değerlerini içeren büyük bir veritabanı üzerinden eşleşme arayabilir.

Burada MD5 algoritması matematiksel olarak "tersine çevrilmez". Bunun yerine verilen hash ile eşleşen, daha önceden bilinen bir düz metin değeri aranır.

Süreç basitleştirilmiş şekilde şöyle düşünülebilir:

```text
Verilen MD5 Hash
       ↓
Bilinen Hash'ler Arasında Arama
       ↓
Eşleşmenin Bulunması
       ↓
Düz Metin Parolanın Elde Edilmesi
```

İlgili parola/hash kombinasyonu veritabanında bulunduğu için parolaya kısa sürede ulaşabildim.

---

### Salting Neden Önemlidir?

Challenge'daki önemli güvenlik problemlerinden biri parolanın **salt kullanılmadan hashlenmiş** olmasıydı.

Salt, parola hashlenmeden önce parolayla birlikte kullanılan benzersiz ve rastgele bir değerdir.

Salt kullanılmadığında:

```text
password123 → MD5 → Hash A
password123 → MD5 → Hash A
password123 → MD5 → Hash A
```

Aynı parola her zaman aynı hash değerini üretir.

Benzersiz salt değerleri kullanıldığında ise:

```text
password123 + Salt A → Hash X
password123 + Salt B → Hash Y
password123 + Salt C → Hash Z
```

Parolalar aynı olmasına rağmen ortaya çıkan hash değerleri farklı olur.

Bu durum, önceden hesaplanmış hash veritabanlarının etkinliğini önemli ölçüde azaltır.

---

### Bu Challenge'dan Öğrendiklerim

Bu challenge, **MD5 gibi hızlı ve genel amaçlı hash fonksiyonlarının doğrudan parola saklamak için neden kullanılmaması gerektiğini** gösterdi.

İzlediğim süreç özetle:

```text
Hash Değerinin Elde Edilmesi
        ↓
Hash Türünün Analiz Edilmesi
        ↓
MD5'in Tespit Edilmesi
        ↓
Hash Veritabanında Arama
        ↓
Düz Metin Parolanın Bulunması
        ↓
Sunucuya Erişim
```

Ayrıca güvenli parola saklama sistemlerinde **Argon2id, bcrypt, scrypt veya PBKDF2** gibi parola hashleme için tasarlanmış algoritmaların ve her parola için **benzersiz bir salt** kullanılmasının önemini öğrendim.
