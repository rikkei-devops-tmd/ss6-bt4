BAI 4: QUAN LY TIEN TRINH NEN VOI NOHUP VA TIN HIEU KILL

MUC TIEU
- Biet cach khoi chay tien trinh nen doc lap voi phien Terminal bang nohup va &.
- Thanh thao giam sat tien trinh bang ps, top/htop.
- Hieu co che va su dung lenh kill voi cac tin hieu he thong.

BOI CANH
Chay script giam sat he thong lien tuc duoi nen,
dam bao van hoat dong khi ngat ket noi SSH.


1. NOI DUNG FILE LOOP-MONITOR.SH

```bash
#!/bin/bash
while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
```


2. CẤP QUYEN THUC THI VA CHAY NEN

Cap quyen thuc thi cho script:

```bash
chmod +x loop-monitor.sh
```

Chay an duoi nen doc lap bang nohup:

```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```

Giai thich:
- nohup         : giu tien trinh chay sau khi dong terminal (bo qua tin hieu SIGHUP).
- > /dev/null   : chuyen huong stdout vao /dev/null (bo qua output).
- 2>&1          : gop stderr vao stdout (cung bo qua).
- &             : chay tien trinh o nen.

Ket qua mau:
[1] 12345
(PID = 12345 la vi du)


3. KIEM TRA TIEN TRINH DANG CHAY

Kiem tra tien trinh:

```bash
ps aux | grep loop-monitor.sh
```

Ket qua mau:

```
ubuntu   12345  0.0  0.0  13236  1024 ?  S  08:35  0:00 /bin/bash ./loop-monitor.sh
ubuntu   12350  0.0  0.0  14432   932 pts/0  S+  08:35  0:00 grep --color=auto loop-monitor.sh
```

Lay PID nhanh bang pgrep:

```bash
pgrep -f loop-monitor.sh
```

Ket qua: 12345


4. KIEM TRA FILE LOG DANG DUOC GHI

```bash
tail -n 10 /tmp/monitor.log
```

Ket qua mau:

```
System time: Sun Oct  5 08:35:05 UTC 2026
System time: Sun Oct  5 08:35:10 UTC 2026
System time: Sun Oct  5 08:35:15 UTC 2026
System time: Sun Oct  5 08:35:20 UTC 2026
System time: Sun Oct  5 08:35:25 UTC 2026
System time: Sun Oct  5 08:35:30 UTC 2026
System time: Sun Oct  5 08:35:35 UTC 2026
System time: Sun Oct  5 08:35:40 UTC 2026
System time: Sun Oct  5 08:35:45 UTC 2026
System time: Sun Oct  5 08:35:50 UTC 2026
```

Log duoc ghi deu dang moi 5 giay, xac nhan tien trinh hoat dong chinh xac.


5. TAT TIEN TRINH AN TOAN

Gui tin hieu SIGTERM (15) - yeu cau tat nhe nhang:

```bash
kill -15 12345
```

Xac nhan tien trinh da tat:

```bash
ps aux | grep loop-monitor.sh
```

Ket qua: Chi con dong grep, khong con tien trinh loop-monitor.sh.

Neu tien trinh khong tat sau SIGTERM, dung SIGKILL (9):

```bash
kill -9 12345
```


6. TONG KET CAC LENH DA THUC HIEN

```bash
chmod +x loop-monitor.sh
nohup ./loop-monitor.sh > /dev/null 2>&1 &
ps aux | grep loop-monitor.sh
pgrep -f loop-monitor.sh
tail -n 10 /tmp/monitor.log
kill -15 12345
ps aux | grep loop-monitor.sh
```

PID su dung trong bai: 12345 (vi du minh hoa, PID thuc te lay tu ket qua pgrep khi chay).
