# English

## Challenge Overview

In this challenge, we were provided with a Python script that processes song lyrics. Instead of starting from the beginning of the file, the program began execution at the **[VERSE1]** section. The hidden flag, however, was stored at **line index 0**, making it unreachable through the program's intended execution flow.

The objective was to analyze the source code, identify the underlying vulnerability, and redirect the program to the hidden section.

## Analysis

After reviewing the Python source code, I identified two important behaviors:

### Line Splitting

Each line was split using the semicolon (`;`) character, and every resulting segment was interpreted sequentially.

### Jump Mechanism

Whenever the interpreter encountered a command in the following format:

```text
RETURN <line_number>
```

it immediately jumped to the specified line index.

The challenge was that any input entered in the **CROWD** section was followed by an automatic increment of the line pointer (`lip += 1`), causing user-supplied commands to be skipped before they could be executed.

## Solution

To bypass this behavior, I took advantage of the program's semicolon parsing logic by submitting the following payload:

```text
;RETURN 0
```

The interpreter processed the payload as two separate commands:

1. An empty command before the semicolon (ignored).
2. `RETURN 0`, which was executed immediately afterward.

Because the jump occurred **before** the interpreter advanced to the next line, the program successfully redirected execution to **line index 0**, revealing the hidden flag.

---

##  Conclusion

This challenge demonstrates how seemingly harmless parsing logic can introduce serious vulnerabilities.

- Improper input parsing can allow unintended command execution.
- Delimiters such as `;` may enable **command injection** when user input is not properly validated.
- Understanding the program's execution flow is often more valuable than simply reading the source code.
- Careful analysis of parser behavior can reveal unexpected ways to manipulate program execution.

---

# Türkçe

## Challenge Özeti

Bu labda bize şarkı sözlerini işleyen bir Python programı verildi. Program dosyayı en baştan okumak yerine **[VERSE1]** bölümünden başlatıyordu. Ancak flag, dosyanın **0. satırında** gizlenmişti ve normal program akışıyla bu bölüme ulaşmak mümkün değildi.

Amaç, kaynak kodunu analiz ederek bu kısıtlamayı aşacak bir yöntem bulmaktı.

## Analiz

Kaynak kodunu incelediğimde iki önemli davranış fark ettim.

### Satırların Parçalanması

Program, her satırı **noktalı virgül (`;`)** karakterine göre parçalıyor ve oluşan her parçayı sırayla yorumluyordu.

### Atlama Mekanizması

Eğer yorumlanan parçalardan biri aşağıdaki formatta bir komut içeriyorsa,

```text
RETURN <satır_numarası>
```

program doğrudan belirtilen satıra atlıyordu.

Ancak kullanıcı **CROWD** bölümüne bir giriş yaptığında, program hemen ardından satır işaretçisini (`lip += 1`) artırıyor ve yazılan komut çalıştırılmadan bir sonraki satıra geçiyordu.

## Çözüm

Bu davranışı aşmak için programın satırları **`;` karakterine göre ayırma** özelliğinden faydalandım ve aşağıdaki girdiyi kullandım:

```text
;RETURN 0
```

Program bu girdiyi iki ayrı parça olarak değerlendirdi:

1. Noktalı virgülden önceki boş ifade (hiçbir işlem yapılmadı).
2. `RETURN 0` komutu.

İkinci komut, satır işaretçisi bir sonraki satıra geçmeden önce çalıştırıldığı için program doğrudan **0. satıra** yönlendirildi ve burada saklanan flag başarıyla görüntülendi.

---

## Sonuç Olarak;

Bu challenge, basit görünen ayrıştırma (parsing) mekanizmalarının nasıl güvenlik açıklarına yol açabileceğini göstermektedir.

- Kullanıcı girdilerinin doğru şekilde doğrulanmaması beklenmeyen komutların çalıştırılmasına neden olabilir.
- `;` gibi ayırıcı karakterler, uygun kontroller yapılmadığında **command injection** benzeri zafiyetlere yol açabilir.
- Bir uygulamanın **çalışma akışını (execution flow)** anlamak, güvenlik açıklarını keşfetmede kaynak kodunu okumak kadar önemlidir.
- Parser'ın çalışma mantığını dikkatlice analiz etmek, programın davranışını beklenmedik şekillerde değiştirmeyi mümkün kılabilir.
