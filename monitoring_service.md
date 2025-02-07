# Giám sát 1 số service
## giám sát service trên port 8080
**additem**
![image](https://github.com/user-attachments/assets/a47cc852-95e7-437c-addd-4fe509793536)
**addTriggers**
![image](https://github.com/user-attachments/assets/0fb46267-3922-4bd8-95ac-249d89a73fc7)
**Kiểm tra**
![image](https://github.com/user-attachments/assets/37bfdd45-4f50-4c2e-84b0-eb7b3c145910)
chạy ok
## 1 số item khác
**Đếm số process sử dụng cho service**
![image](https://github.com/user-attachments/assets/a5d5fff6-a00c-4f8d-a5ee-7ca6dcd4437f)
Key ở đây được sử dụng là 

```
proc.num[mysqld]
```

**Lượng RAM sử dụng cho service**
![image](https://github.com/user-attachments/assets/cb7f2501-4f35-42d4-a923-9cc162fde79d)

Key ở đây là

```
proc.cpu.util[mysqld]
```

Ở đây phần trăm được tính trên từng core của CPU. Nên nếu service này sử dụng hết CPU 2 core thì giá trị sẽ là 200%.

**Lượng RAM sử dụng cho service**
![image](https://github.com/user-attachments/assets/a2e5115d-6ade-4efe-aede-2aefbf330fd0)


Key được sử dụng ở đây là:

```
proc.mem[mysqld]
```

