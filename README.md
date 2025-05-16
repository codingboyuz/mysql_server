MySQL serverni Windowsda yaratish va local tarmoqdagi barcha kompyuterlardan kira olish uchun quyidagi bosqichlarni bajaramiz:

---

### **1. MySQL Serverni O'rnatish**  
- **MySQL Installer** yuklab oling: [MySQL Official Site](https://dev.mysql.com/downloads/installer/)  
- **Server** va **Workbench**ni tanlab o'rnating.  
- O'rnatish jarayonida:  
  - **Root password**ni belgilang.  
  - **MySQL as a Windows Service**ni tanlang.  

---

### **2. MySQL Konfiguratsiyasi (my.ini-ni tahrirlash)**  
1. `my.ini` faylini tahrirlash:  
   Odatda quyidagi manzilda bo'ladi:  
   ```
   C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
   ```
2. `my.ini` faylini oching va quyidagilarni qo'shing yoki tahrirlang:
   ```ini
   [mysqld]
   bind-address = 0.0.0.0
   skip-networking = 0
   ```
   Bu sozlama boshqa kompyuterlardan ulanishga ruxsat beradi.

3. MySQL serverni qayta ishga tushuring:  
   ```bash
   net stop mysql
   net start mysql
   ```
---

### **3. MySQLda Database va User Yaratish**  
1. **MySQL Workbench** yoki **Command Line** orqali MySQLga `root` bilan ulaning:
   ```bash
   mysql -u root -p
   ```
2. **Database yaratish:**  
   ```sql
   CREATE DATABASE my_database;
   ```
3. **User yaratish va huquq berish:**  
   ```sql
   CREATE USER 'my_user'@'%' IDENTIFIED BY 'mypassword';
   GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';
   FLUSH PRIVILEGES;
   ```
   - `'%'` – barcha IP-lardan ulanishga ruxsat beradi.  
   - Xavfsizlik uchun tarmoq ichida faqat muayyan IP-larga ruxsat berishni tavsiya qilaman.

4. **Jadval yaratish:**  
   ```sql
   USE my_database;
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100),
       email VARCHAR(100) UNIQUE,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```
---

MySQL’da biror **user (foydalanuvchi)** ga qanday **bazalar (databases)** va **jadval (tables)** ga ruxsat (access) berilganini ko‘rish uchun quyidagi amallarni bajarish mumkin:

---


## ✅ 2. **Foydalanuvchilar ro‘yxatini ko‘rish:**

```sql
SELECT User, Host FROM mysql.user;
```

Bu bilan barcha foydalanuvchilar va qaysi manzildan ulanganini ko‘rasiz.

---

## ✅ 3. **Userning ruxsatlarini ko‘rish:**

Masalan, `your_user` foydalanuvchisi uchun:

```sql
SHOW GRANTS FOR 'your_user'@'localhost';
```

Natijada sizga shunga o‘xshash narsa chiqadi:

```
GRANT SELECT, INSERT ON your_database.* TO 'your_user'@'localhost';
```

---

## ✅ 4. **Barcha mavjud bazalarni ko‘rish:**

```sql
SHOW DATABASES;
```

---

## ✅ 5. **Bazadagi barcha jadval (table)larni ko‘rish:**

Avval kerakli bazaga o‘ting:

```sql
USE your_database;
```

Keyin:

```sql
SHOW TABLES;
```

---

## ✅ 6. **Jadval tuzilmasini ko‘rish:**

```sql
DESCRIBE your_table;
```

yoki:

```sql
SHOW COLUMNS FROM your_table;
```

---

## 🔍 Agar sizga **boshqa foydalanuvchilar qanday ruxsatga ega** ekanini ko‘rish kerak bo‘lsa — root userdan quyidagi so‘rov bilan barcha ruxsatlarni tekshiring:

```sql
SELECT * FROM mysql.db WHERE User = 'your_user';
```

---



### **4. Windows Firewall Sozlamalari**  
- **Windows Defender Firewall**da **MySQL porti (standart: 3306)** uchun inbound ruxsat qo'shing:  
  1. **Control Panel > Windows Defender Firewall > Advanced settings**ga kiring.  
  2. **Inbound Rules > New Rule** tanlang.  
  3. **Port**ni tanlang va port raqami sifatida **3306** ni kiriting.  
  4. **Allow the connection** ni tanlang.  
  5. **Rule Name** sifatida "MySQL Remote Access" yozing va saqlang.  

---

### **5. Ulanishni Test Qilish**  
- **Boshqa kompyuterda** `MySQL Workbench` yoki terminal orqali ulaning:  
```bash
mysql -u my_user -p -h [MySQL_server_IP] -D my_database
```
**Misol:**  
```bash
mysql -u my_user -p -h 192.168.1.10 -D my_database
```

Agar hammasi to'g'ri bo'lsa, muvaffaqiyatli ulanasiz.

---

### ✅ **Qo'shimcha Tavsiyalar:**  
- **MySQL user**lari uchun `IP` cheklovini qo'ying (`'my_user'@'192.168.1.%'`).  
- **Database zaxirasini** (backup) muntazam saqlang.  
- **`root` foydalanuvchisi** uchun faqat lokal (`localhost`) ulanishga ruxsat bering.  

---


