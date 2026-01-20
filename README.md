# Process ve Disk Yönetimi Araştırması

**Hazırlayan:** Ayşe Çamır
**Konu:** İşletim Sistemlerinde Süreç ve Disk Yönetimi Derinlemesine İnceleme

---

## 1. Giriş
Bu projede, Linux tabanlı işletim sistemlerinde sistemin belkemiğini oluşturan **Process (İşlem)** ve **Disk Yönetimi** kavramları araştırılmıştır. Bir programın çalıştırıldığında nasıl bir işlem haline geldiği ve verilerin diskte nasıl tutulduğu incelenmiştir.

---

## 2. Process (İşlem) Yönetimi

### Process Nedir?
Diskte duran pasif kod parçasına "Program", bu programın belleğe yüklenip işlemci tarafından çalıştırılan aktif haline "Process" denir. Her işlemin kendine ait benzersiz bir kimlik numarası (**PID**) vardır.

### Sistemdeki İşlemlerin Görüntüsü (Kanıt)
Aşağıda, kendi sistemimde `top` komutu ile aldığım anlık işlemci süreçleri görülmektedir:


### Process Durumları (Lifecycle)
Bir işlem hayatı boyunca şu durumlardan geçer:
1.  **Running:** Çalışıyor.
2.  **Sleeping:** Beklemede.
3.  **Zombie:** İşlem bitmiş ama kaydı silinmemiş.

---

## 3. Disk Yönetimi

### Linux Dosya Sistemi
Linux'ta "Her şey bir dosyadır". C: veya D: sürücüleri yerine tek bir kök (`/`) dizini vardır.

### Disk Doluluk Oranı Görüntüsü (Kanıt)
Aşağıda, `df -h` komutu ile aldığım disk kullanım oranları yer almaktadır:

### Inode Kavramı
Her dosyanın bir kimlik kartı vardır, buna **Inode** denir. Dosyanın izni, sahibi ve boyutu burada tutulur.

---

## 4. Kaynakça
* Linux Man Pages
* GeeksforGeeks OS Tutorials
