\# IT209 - Session 06 - Exercise 4



\## 1. Mục tiêu



Thực hành quản lý tiến trình nền trên Ubuntu VPS bằng `nohup`, `ps`, `pgrep` và `kill`.



\- Tạo shell script chạy liên tục.

\- Ghi thời gian hiện tại vào `/tmp/monitor.log` mỗi 5 giây.

\- Chạy script độc lập với phiên SSH bằng `nohup`.

\- Tìm PID của tiến trình.

\- Kiểm tra tiến trình và log.

\- Dừng tiến trình bằng tín hiệu SIGTERM (15).



\## 2. Tạo script



File `loop-monitor.sh`:



```bash

\#!/bin/bash



while true; do

&#x20;   echo "System time: $(date)" >> /tmp/monitor.log

&#x20;   sleep 5

done



Gán quyền thực thi:



chmod +x loop-monitor.sh



Kết quả:



\-rwxr-xr-x 1 root root 97 Oct 6 16:01 loop-monitor.sh

3\. Khởi chạy tiến trình bằng nohup



Lệnh:



nohup ./loop-monitor.sh > /dev/null 2>\&1 \&



Kết quả:



\[1] 2212



PID của tiến trình là 2212.



4\. Kiểm tra PID



Lệnh:



pgrep -f loop-monitor.sh



Kết quả:



2212

5\. Kiểm tra tiến trình



Lệnh:



ps aux | grep loop-monitor.sh



Kết quả:



root        2212  0.0  0.0   4752  3328 pts/1    S    16:02   0:00 /bin/bash ./loop-monitor.sh



Tiến trình loop-monitor.sh đang hoạt động với PID 2212.



6\. Kiểm tra file log



Lệnh:



tail -n 10 /tmp/monitor.log



Kết quả:



System time: Tue Oct  6 16:03:42 UTC 2026

System time: Tue Oct  6 16:03:47 UTC 2026

System time: Tue Oct  6 16:03:52 UTC 2026

System time: Tue Oct  6 16:03:57 UTC 2026

System time: Tue Oct  6 16:04:02 UTC 2026

System time: Tue Oct  6 16:04:07 UTC 2026

System time: Tue Oct  6 16:04:12 UTC 2026

System time: Tue Oct  6 16:04:17 UTC 2026

System time: Tue Oct  6 16:04:22 UTC 2026

System time: Tue Oct  6 16:04:27 UTC 2026



Log được ghi đều đặn khoảng 5 giây một lần.



7\. Dừng tiến trình bằng SIGTERM



PID của tiến trình:



2212



Thực hiện:



kill -15 2212



Kiểm tra lại:



pgrep -f loop-monitor.sh



Kết quả không còn PID của tiến trình.



Terminal hiển thị:



\[1]+  Terminated  nohup ./loop-monitor.sh > /dev/null 2>\&1



Tiến trình đã được dừng thành công bằng tín hiệu SIGTERM (15).



Không cần sử dụng SIGKILL (kill -9) vì SIGTERM đã dừng được tiến trình.



8\. Kết luận

&#x20;Tạo file loop-monitor.sh.

&#x20;Gán quyền thực thi cho script.

&#x20;Chạy script bằng nohup và \&.

&#x20;Tìm được PID 2212.

&#x20;Kiểm tra tiến trình bằng ps.

&#x20;Kiểm tra PID bằng pgrep.

&#x20;Kiểm tra log bằng tail.

&#x20;Log được ghi định kỳ mỗi 5 giây.

&#x20;Dừng tiến trình bằng kill -15.

&#x20;Không cần sử dụng kill -9.

