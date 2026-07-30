## Challenge Overview

In this challenge, I was given a large **server.log** file containing thousands of log entries. The objective was to analyze the logs, identify the relevant information, and reconstruct the hidden flag.

## Analysis

### Initial Reconnaissance

To understand the contents of the log file, I first searched for informational log entries:

```bash
cat server.log | grep INFO
```

This provided a better understanding of the log structure and helped identify patterns within the data.

### Identifying the Flag Fragments

While examining the logs, I noticed that every fragment of the flag was marked with the keyword **FLAGPART**.

To display only those entries, I used:

```bash
cat server.log | grep FLAGPART
```

The output contained all of the flag fragments, but many of them appeared multiple times.

## Solution

To remove duplicate entries, I used the **uniq** command. However, I learned that **uniq** only removes **adjacent** duplicate lines. Therefore, the data must first be sorted so that identical lines are grouped together.

The final command became:

```bash
cat server.log | grep FLAGPART | sort | uniq
```

After sorting and removing duplicate entries, I combined the remaining unique flag fragments to reconstruct the final flag.

---

## Key Takeaway

This challenge demonstrates how combining simple Linux commands can make log analysis much more efficient.

- **grep** filters lines matching a specific pattern.
- **sort** groups identical lines together by ordering the output.
- **uniq** removes only **consecutive** duplicate lines, making it most effective after `sort`.
- Chaining commands with pipes (`|`) is a powerful technique for processing large log files.

> **Note:** Since `grep` can read files directly, the command can also be simplified to:
>
> ```bash
> grep FLAGPART server.log | sort | uniq
> ```

---

## Challenge Özeti

Bu labda, binlerce satırdan oluşan büyük bir **server.log** dosyası verildi. Amaç, log kayıtlarını analiz ederek flag'i oluşturan parçaları bulmak ve bunları birleştirmekti.

## Analiz

### İlk İnceleme

Öncelikle log dosyasının yapısını anlamak için bilgi içeren kayıtları filtreledim:

```bash
cat server.log | grep INFO
```

Bu sayede logların genel yapısını inceleyerek hangi formatta tutulduklarını gözlemledim.

### Flag Parçalarının Bulunması

Logları incelerken, flag'i oluşturan parçaların **FLAGPART** etiketiyle işaretlendiğini fark ettim.

Bunun üzerine yalnızca bu satırları listelemek için şu komutu kullandım:

```bash
cat server.log | grep FLAGPART
```

Ancak elde edilen çıktı içerisinde aynı satırların birçok kez tekrar ettiğini gördüm.

## Çözüm

Tekrar eden satırları kaldırmak için **uniq** komutunu kullandım. Ancak **uniq**, yalnızca **yan yana bulunan** aynı satırları kaldırabilir. Bu nedenle önce satırların sıralanması gerekiyordu.

Son komut şu şekilde oldu:

```bash
cat server.log | grep FLAGPART | sort | uniq
```

Bu komut sayesinde aynı kayıtlar yan yana getirildi, tekrar eden satırlar kaldırıldı ve geriye kalan benzersiz flag parçalarını birleştirerek nihai flag'e ulaştım.

---

## Çıkarım

Bu challenge, Linux komutlarının birlikte kullanıldığında log analizi için ne kadar güçlü olduğunu göstermektedir.

- **grep**, belirli bir deseni içeren satırları filtrelemek için kullanılır.
- **sort**, aynı satırları yan yana getirerek veriyi sıralar.
- **uniq**, yalnızca **ardışık** tekrar eden satırları kaldırır; bu nedenle genellikle `sort` ile birlikte kullanılır.
- Pipe (`|`) operatörü, bir komutun çıktısını diğerine aktararak büyük dosyaların hızlı ve etkili şekilde analiz edilmesini sağlar.

> **Not:** `grep` dosyaları doğrudan okuyabildiği için aynı işlem daha kısa şekilde de yazılabilir:
>
> ```bash
> grep FLAGPART server.log | sort | uniq
> ```
