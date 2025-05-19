# Домашнее задание к занятию «Уязвимости и атаки на информационные системы»
sys-37 Пономарев Игорь

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.
Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.
Просканируйте эту виртуальную машину, используя **nmap**.
Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.
Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.
Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:
- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
 
*Приведите ответ в свободной форме.*

### Решение 1

Разрешены следующие сетевые службы:

  PORT     STATE SERVICE
* 21/tcp   open  ftp         vsftpd 2.3.4
* 22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
* 23/tcp   open  telnet      Linux telnetd
* 25/tcp   open  smtp        Postfix smtpd
* 53/tcp   open  domain      ISC BIND 9.4.2
* 80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
* 139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
* 445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
* 512/tcp  open  exec?
* 513/tcp  open  login       OpenBSD or Solaris rlogind
* 514/tcp  open  tcpwrapped
* 1099/tcp open  java-rmi    GNU Classpath grmiregistry
* 1524/tcp open  bindshell   Metasploitable root shell
* 2049/tcp open  nfs         2-4 (RPC #100003)
* 2121/tcp open  ftp         ProFTPD 1.3.1
* 3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
* 5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
* 5900/tcp open  vnc         VNC (protocol 3.3)
* 6000/tcp open  X11         (access denied)
* 6667/tcp open  irc         UnrealIRCd
* 8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
* 8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1


Были обнаружены следующие уязвимости:
1. [vsftpd 2.3.4 - Backdoor Command Execution](https://www.exploit-db.com/exploits/49757). 
2. [OpenSSH < 7.7 - User Enumeration (2)](https://www.exploit-db.com/exploits/45939). 
3. [MySQL 5.0.x - IF Query Handling Remote Denial of Service](https://www.exploit-db.com/exploits/30020).

---

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*

### Решение 2
### SYN-сканирование
Сканирование SYN отправляет SYN-пакеты на порты, что позволяет определить, какие порты открыты.
- nmap -sS 192.168.1.141
- ![syn](https://github.com/user-attachments/assets/d496255b-ed31-42f9-a270-dea8397ab5a7)

### FIN-сканирование
FIN-сканирование отправляет пакеты с флагом FIN. Это не завершает соединение и может обойти некоторые системы обнаружения.
- nmap -sF 192.168.1.141
- ![FIN](https://github.com/user-attachments/assets/4ba2b620-320f-4805-b1c4-21d4afe89ce7)

### Xmas-сканирование
Xmas-сканирование отправляет пакеты с флагами FIN, PSH и URG. Это позволяет скрыть сканирование от некоторых систем.
- nmap -sX 192.168.1.141
- ![XMAS](https://github.com/user-attachments/assets/86141267-4c42-4266-b84f-eb08fabda379)

### UDP-сканирование
UDP-сканирование используется для проверки доступности UDP-портов.
- nmap -sU 192.168.1.141
- ![UDP](https://github.com/user-attachments/assets/de475f97-5646-4b40-a47f-f4f4d293fc85)

Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- SYN scan: Отправка только SYN-пакетов, используется для обнаружения открытых портов.
- FIN scan: Отправка FIN-пакетов, которые могут быть проигнорированы или отклонены.
- Xmas scan: Использует несколько флагов (FIN, PSH, URG), может быть более скрытным.
- UDP scan: Проверяет доступность UDP-портов, т.к. UDP не использует установление соединений, как TCP.

### Как отвечает сервер?
- Сервер может посылать различные типы ответов в зависимости от режима сканирования: SYN-ACK, RESET, или вообще не отвечать, если порт закрыт.

---
