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
[sistem_durumu.txt](https://github.com/user-attachments/files/24739798/sistem_durumu.txt)
```text
top - 16:56:47 up 4 min,  2 users,  load average: 1.31, 1.05, 0.47
Tasks: 198 total,   1 running, 197 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  6.9 sy,  0.0 ni, 91.7 id,  0.0 wa,  0.0 hi,  1.4 si,  0.0 st 
MiB Mem :   1958.1 total,    277.1 free,   1176.4 used,    666.9 buff/cache     
MiB Swap:   3776.0 total,   3776.0 free,      0.0 used.    781.7 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   3308 ayse      20   0    9592   4608   2688 R   5.9   0.2   0:00.11 top
      1 root      20   0   22448  13972  10100 S   0.0   0.7   0:01.91 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_wo+
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      8 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
      9 root      20   0       0      0      0 I   0.0   0.0   0:00.37 kworker+
     10 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     11 root      20   0       0      0      0 I   0.0   0.0   0:00.19 kworker+
     12 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tas+
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.07 ksoftir+
     17 root      20   0       0      0      0 I   0.0   0.0   0:01.32 rcu_pre+
     18 root      rt   0       0      0      0 S   0.0   0.0   0:00.02 migrati+
     19 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_in+
     20 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0
     21 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1
     22 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_in+
     23 root      rt   0       0      0      0 S   0.0   0.0   0:00.25 migrati+
     24 root      20   0       0      0      0 S   0.0   0.0   0:02.44 ksoftir+
     25 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     26 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     27 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/2
     28 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_in+
     29 root      rt   0       0      0      0 S   0.0   0.0   0:00.27 migrati+
     30 root      20   0       0      0      0 S   0.0   0.0   0:00.09 ksoftir+
     31 root      20   0       0      0      0 I   0.0   0.0   0:00.06 kworker+
     32 root       0 -20       0      0      0 I   0.0   0.0   0:00.20 kworker+
     33 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/3
     34 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_in+
     35 root      rt   0       0      0      0 S   0.0   0.0   0:00.25 migrati+
     36 root      20   0       0      0      0 S   0.0   0.0   0:00.11 ksoftir+
     37 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     38 root       0 -20       0      0      0 I   0.0   0.0   0:00.01 kworker+
     40 root      20   0       0      0      0 I   0.0   0.0   0:02.59 kworker+
     41 root      20   0       0      0      0 I   0.0   0.0   0:00.08 kworker+
     42 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     43 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kdevtmp+
     44 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     45 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kauditd
     46 root      20   0       0      0      0 S   0.0   0.0   0:00.00 khungta+
     47 root      20   0       0      0      0 S   0.0   0.0   0:00.00 oom_rea+
     48 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     49 root      20   0       0      0      0 S   0.0   0.0   0:00.05 kcompac+
     50 root      25   5       0      0      0 S   0.0   0.0   0:00.00 ksmd
     51 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
     52 root      39  19       0      0      0 S   0.0   0.0   0:00.69 khugepa+
     53 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     54 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     55 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     56 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 irq/9-a+
     57 root      20   0       0      0      0 I   0.0   0.0   0:00.84 kworker+
     58 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     59 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     60 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     61 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     62 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kswapd0
     64 root      20   0       0      0      0 I   0.0   0.0   0:00.06 kworker+
     69 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     71 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     72 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     73 root       0 -20       0      0      0 I   0.0   0.0   0:00.18 kworker+
     74 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     79 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     83 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     88 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
     89 root      20   0       0      0      0 I   0.0   0.0   0:00.18 kworker+
     90 root      20   0       0      0      0 I   0.0   0.0   0:00.05 kworker+
    181 root       0 -20       0      0      0 I   0.0   0.0   0:00.20 kworker+
    185 root       0 -20       0      0      0 I   0.0   0.0   0:00.71 kworker+
    186 root      20   0       0      0      0 I   0.0   0.0   0:00.21 kworker+
    188 root      20   0       0      0      0 I   0.0   0.0   0:00.04 kworker+
    189 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    196 root      20   0       0      0      0 S   0.0   0.0   0:00.03 scsi_eh+
    197 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    198 root      20   0       0      0      0 S   0.0   0.0   0:00.03 scsi_eh+
    199 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    202 root      20   0       0      0      0 S   0.0   0.0   0:00.00 scsi_eh+
    203 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    204 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
    205 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
    215 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 irq/18-+
    216 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    219 root      20   0       0      0      0 I   0.0   0.0   0:00.45 kworker+
    221 root       0 -20       0      0      0 I   0.0   0.0   0:00.10 kworker+
    270 root      20   0       0      0      0 S   0.0   0.0   0:00.15 jbd2/sd+
    271 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    329 root      20   0   41580  15744  14848 S   0.0   0.8   0:00.39 systemd+
    371 root      20   0   29712   7568   4880 S   0.0   0.4   0:00.47 systemd+
    375 root      -2   0       0      0      0 S   0.0   0.0   0:00.00 psimon
    456 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    510 root      20   0    8276   6460   1664 S   0.0   0.3   0:00.36 haveged
    522 root      20   0  308844   6728   6088 S   0.0   0.3   0:00.21 account+
    523 message+  20   0    8172   4992   3712 S   0.0   0.2   0:00.86 dbus-da+
    525 polkitd   20   0  381788   9348   6976 S   0.0   0.5   0:00.59 polkitd
    526 root      20   0   17588   8772   7552 S   0.0   0.4   0:00.38 systemd+
    529 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    530 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    532 root      20   0    7048   2432   2304 S   0.0   0.1   0:00.02 cron
    591 root      20   0  333692  19276  16716 S   0.0   1.0   0:00.40 Network+
    606 root      20   0  315992  11580  10044 S   0.0   0.6   0:00.23 ModemMa+
    608 root      20   0  290808   2688   2560 S   0.0   0.1   0:00.17 VBoxSer+
    630 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
    656 root      20   0  380680   7100   6460 S   0.0   0.4   0:00.06 lightdm
    666 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    669 root      20   0    6900   2176   2048 S   0.0   0.1   0:00.02 agetty
    670 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
    672 root      20   0  405296 124240  60836 S   0.0   6.2   0:28.51 Xorg
    677 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker+
    697 root      -2   0       0      0      0 S   0.0   0.0   0:00.00 psimon
    724 rtkit     21   1   20244   2688   2560 S   0.0   0.1   0:00.08 rtkit-d+
    792 root      20   0  234864   7896   7040 S   0.0   0.4   0:00.06 lightdm
    800 ayse      20   0   21088  11776   9600 S   0.0   0.6   0:00.27 systemd
    801 ayse      20   0   20360   3468   2048 S   0.0   0.2   0:00.00 (sd-pam)
    817 ayse       9 -11  101536  11736   7896 S   0.0   0.6   0:00.05 pipewire
    818 ayse      20   0   84960   5376   4608 S   0.0   0.3   0:00.01 pipewire
    819 ayse       9 -11  480084  18304  13440 S   0.0   0.9   0:00.16 wireplu+
    820 ayse       9 -11   99312   8448   7040 S   0.0   0.4   0:00.03 pipewir+
    822 ayse      20   0  313976   9472   8576 S   0.0   0.5   0:00.06 gnome-k+
    823 ayse      20   0    7380   4864   3840 S   0.0   0.2   0:00.51 dbus-da+
    846 ayse      20   0  337096  23584  16256 S   0.0   1.2   0:00.39 xfce4-s+
    898 ayse      20   0   17500   1676   1408 S   0.0   0.1   0:00.00 VBoxCli+
    899 ayse      20   0  215688   3852   3456 S   0.0   0.2   0:00.01 VBoxCli+
    910 ayse      20   0   17500   1676   1408 S   0.0   0.1   0:00.00 VBoxCli+
    912 ayse      20   0  215800   3084   2816 S   0.0   0.2   0:00.62 VBoxCli+
    921 ayse      20   0   17500   1548   1280 S   0.0   0.1   0:00.00 VBoxCli+
    922 ayse      20   0  281852   3084   2688 S   0.0   0.2   0:01.44 VBoxCli+
    934 ayse      20   0    9768   1548    768 S   0.0   0.1   0:00.00 ssh-age+
    944 ayse      20   0  380692   7424   6912 S   0.0   0.4   0:00.01 at-spi-+
    951 ayse      20   0    6876   4352   3968 S   0.0   0.2   0:00.09 dbus-da+
    956 ayse      20   0  307736   6144   5504 S   0.0   0.3   0:00.26 xfconfd
    962 ayse      20   0  233844   7424   6784 S   0.0   0.4   0:00.09 at-spi2+
    978 ayse      20   0   81980   3456   3200 S   0.0   0.2   0:00.01 gpg-age+
    980 ayse      20   0   17500   1548   1280 S   0.0   0.1   0:00.00 VBoxCli+
    981 ayse      20   0  215892   3340   2944 S   0.0   0.2   0:00.19 VBoxCli+
    987 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
    988 ayse      20   0 1264408 119196  78868 S   0.0   5.9   0:09.88 xfwm4
    992 ayse      20   0  312672   7424   6656 S   0.0   0.4   0:00.04 gvfsd
    998 ayse      20   0  457796   6784   6144 S   0.0   0.3   0:00.01 gvfsd-f+
   1017 ayse      20   0  301652  26740  19240 S   0.0   1.3   0:00.30 xfsetti+
   1021 ayse      20   0  532524  40848  32556 S   0.0   2.0   0:01.04 xfce4-p+
   1026 ayse      20   0  411012  23580  16768 S   0.0   1.2   0:00.16 Thunar
   1032 ayse      20   0  480936  61732  35268 S   0.0   3.1   0:01.66 xfdeskt+
   1033 ayse      20   0  531772  41176  31652 S   0.0   2.1   0:00.80 panel-1+
   1040 ayse      20   0  300668  59916  18432 S   0.0   3.0   0:02.26 panel-1+
   1041 ayse      20   0  409824  23880  17280 S   0.0   1.2   0:00.24 panel-1+
   1042 ayse      20   0  338108  27000  20004 S   0.0   1.3   0:01.47 panel-1+
   1043 ayse      20   0  456852  40052  31768 S   0.0   2.0   0:00.36 panel-1+
   1044 ayse      20   0  456820  37604  29964 S   0.0   1.9   0:00.27 panel-1+
   1045 ayse      20   0  383636  38236  30328 S   0.0   1.9   0:00.53 panel-1+
   1047 ayse      20   0  383488  38192  30200 S   0.0   1.9   0:00.24 panel-2+
   1070 root      20   0  316280   8704   7296 S   0.0   0.4   0:00.36 upowerd
   1087 ayse      20   0  406676  18688  16384 S   0.0   0.9   0:00.23 xfce4-n+
   1107 ayse      20   0  262380  23384  16512 S   0.0   1.2   0:00.31 xfce4-p+
   1111 ayse      20   0  589684  51932  29840 S   0.0   2.6   0:01.04 blueman+
   1121 ayse      20   0  335472  22744  16256 S   0.0   1.1   0:00.19 light-l+
   1125 ayse      39  19  869336  27284  18692 S   0.0   1.4   0:00.39 tumblerd
   1131 ayse      20   0  230356   5504   5120 S   0.0   0.3   0:00.05 dconf-s+
   1132 ayse      20   0  922424   8704   7424 S   0.0   0.4   0:00.15 xiccd
   1142 ayse      20   0  256280  15616  13824 S   0.0   0.8   0:00.08 polkit-+
   1147 ayse      20   0   61252  36224  19968 S   0.0   1.8   0:00.71 applet.+
   1152 ayse      20   0  308376   6144   5760 S   0.0   0.3   0:00.01 agent
   1160 ayse      20   0  619332  44484  35760 S   0.0   2.2   0:00.29 nm-appl+
   1172 colord    20   0  315700  13604   9048 S   0.0   0.7   0:00.37 colord
   1177 ayse      20   0   12632   1708   1408 S   0.0   0.1   0:00.04 xcape
   1196 root      20   0  322252   7040   6144 S   0.0   0.4   0:00.05 pcscd
   1303 ayse      20   0  425984  12800  11136 S   0.0   0.6   0:00.08 gvfs-ud+
   1307 root      20   0  468828  13240  11064 S   0.0   0.7   0:00.23 udisksd
   1332 ayse      20   0  307840   6272   5760 S   0.0   0.3   0:00.04 gvfs-go+
   1337 ayse      20   0  307912   6272   5760 S   0.0   0.3   0:00.03 gvfs-mt+
   1342 ayse      20   0  308872   6656   6016 S   0.0   0.3   0:00.04 gvfs-gp+
   1352 ayse      20   0  388936   7936   6912 S   0.0   0.4   0:00.07 gvfs-af+
   1376 ayse      20   0  534128   8576   7552 S   0.0   0.4   0:00.04 gvfsd-t+
   1383 ayse      20   0   46628   7424   6912 S   0.0   0.4   0:00.05 obexd
   1387 ayse      20   0  234408   6016   5632 S   0.0   0.3   0:00.03 gvfsd-m+
   1404 ayse      20   0  455368  96872  83652 S   0.0   4.8   0:01.42 qtermin+
   1421 ayse      20   0   10.8g 351136 159688 S   0.0  17.5   1:12.44 firefox+
   1469 ayse      20   0   10556   5760   3712 S   0.0   0.3   0:00.24 zsh
   1531 ayse      20   0  480244  17920  14976 S   0.0   0.9   0:00.11 xdg-des+
   1536 ayse      20   0  607092   6656   6144 S   0.0   0.3   0:00.02 xdg-doc+
   1540 ayse      20   0  308556   6272   5760 S   0.0   0.3   0:00.01 xdg-per+
   1547 root      20   0    2496   1536   1536 S   0.0   0.1   0:00.00 fusermo+
   1552 ayse      20   0  406108  18244  15684 S   0.0   0.9   0:00.10 xdg-des+
   1604 ayse      20   0  215452  37376  29184 S   0.0   1.9   0:00.15 Socket +
   1671 ayse      20   0 2446748 117276  90444 S   0.0   5.8   0:01.32 Privile+
   1737 ayse      20   0 2438424  98296  78008 S   0.0   4.9   0:01.47 WebExte+
   1825 ayse      20   0 2646840 238464 101892 S   0.0  11.9   0:43.99 Isolate+
   1849 ayse      20   0 2411468  88868  75708 S   0.0   4.4   0:00.78 Isolate+
   1854 ayse      20   0 2409768  82320  67856 S   0.0   4.1   0:00.77 Isolate+
   2014 ayse      20   0 2403668  71100  59452 S   0.0   3.5   0:00.31 Web Con+
   2172 root       0 -20       0      0      0 I   0.0   0.0   0:00.04 kworker+
   2221 ayse      20   0 2403668  71248  59600 S   0.0   3.6   0:00.34 Web Con+
   2246 ayse      20   0 2403664  70872  59352 S   0.0   3.5   0:00.29 Web Con+
```

### Process Oluşturma Mekanizması (Fork & Exec)
Linux'ta yeni bir işlem "yoktan var edilmez", var olan bir işlemden türetilir:
1.  **fork():** Çalışan bir işlem (Parent), kendisinin birebir kopyasını (Child) oluşturur.
2.  **exec():** Kopyalanan Child işlem, kendi hafıza alanını yeni çalıştıracağı programın koduyla değiştirir.
  
### Process Durumları (Lifecycle)
Bir işlem hayatı boyunca şu durumlardan geçer:
1.  **Running:** Çalışıyor.
2.  **Sleeping:** Beklemede.
3.  **Zombie:** İşlem bitmiş ama ana işlem (parent) tarafından kaydı silinmemiş.

---

## 3. Disk Yönetimi

### Linux Dosya Sistemi
Linux'ta "Her şey bir dosyadır". C: veya D: sürücüleri yerine tek bir kök (`/`) dizini vardır.

### Disk Doluluk Oranı Görüntüsü (Kanıt)
Aşağıda, `df -h` komutu ile aldığım disk kullanım oranları yer almaktadır:
[disk_durumu.txt](https://github.com/user-attachments/files/24740225/disk_durumu.txt)

```text
Filesystem      Size  Used Avail Use% Mounted on
udev            938M     0  938M   0% /dev
tmpfs           196M  1.1M  195M   1% /run
/dev/sda3        26G   15G   10G  60% /
tmpfs           980M     0  980M   0% /dev/shm
efivarfs        256K  159K   93K  64% /sys/firmware/efi/efivars
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-udev-load-credentials.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-sysctl.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev-early.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev.service
tmpfs           980M  8.0K  980M   1% /tmp
/dev/sda2       1.9G  101M  1.7G   6% /boot
/dev/sda1        93M  142K   93M   1% /boot/efi
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs           196M  116K  196M   1% /run/user/1000
```

Analiz: Ana kök dizinim (/), 26 GB kapasiteye sahip olup şu an %60 doluluk oranındadır. Sistemde kullanılan fiziksel disk bölümü /dev/sda3 olarak tanımlanmıştır.

"Sistemde ana sistem dosyaları /dev/sda3 üzerinde yer almaktadır."

"Hızlı erişim ve geçici dosyalar için tmpfs (RAM tabanlı dosya sistemi) aktif olarak kullanılmaktadır."

### Inode Kavramı
Her dosyanın bir kimlik kartı vardır, buna **Inode** denir. Dosyanın izni, sahibi ve boyutu burada tutulur.
### Hard Link vs Soft Link Farkı

| Özellik | Hard Link | Soft Link (Sembolik) |
| :--- | :--- | :--- |
| **Mantık** | Dosyanın Inode numarasına doğrudan işaret eder. | Dosyanın yoluna (path) işaret eden bir kısayoldur. |
| **Orijinal Silinirse** | Dosya kaybolmaz (erişilebilir). | Link bozulur (kırık link olur). |

---
### Kritik İşlemler: Süreç Sonlandırma ve Dosya Arama

#### 1. Süreç Sonlandırma (Kill)
`top` komutu ile kaynakları tüketen bir işlemin PID numarası tespit edildikten sonra, o işlemi sonlandırmak için `kill` komutu kullanılır:

* **`kill <PID>`:** İşleme kapanması için sinyal gönderir (Örn: `kill 3308`).
* **`kill -9 <PID>`:** İşlem yanıt vermiyorsa zorla (force) kapatır.

#### 2. Büyük Dosyaları Bulma
Disk doluluk oranlarını `df` ile gördükten sonra, alanda en çok yer kaplayan büyük dosyaları bulmak için `find` komutu kullanılır.

Örnek (Sistemdeki 100 MB'dan büyük dosyaları listeler):
```bash
find / -type f -size +100M
```
## 4. Kaynakça
* Linux Man Pages
* GeeksforGeeks OS Tutorials
