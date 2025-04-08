# From Beginner to Expert: Application of Raspberry Pi 5

## Chapter 1: Introduction to Raspberry Pi 5

Raspberry Pi 5 is a powerful and affordable single-board computer that opens doors to endless possibilities in programming, automation, and DIY electronics. This book aims to guide readers from the fundamentals to advanced applications, drawing from real experiences and practical usage examples.

In this journey, we explore how Raspberry Pi 5 evolves from a learning board into a functional controller, analysis engine, and even a mini server. The focus will span across hardware setup, Linux OS use, performance benchmarking, automation, bot development, and system monitoring.

## Chapter 2: Hardware Setup

### 2.1 Selecting the Case and Accessories

We use the **Pironman case** with built-in **NVMe SSD support card**, an excellent option for enthusiasts looking for performance and aesthetics. This setup improves airflow and supports SSD boot for faster operation.

### 2.2 Boot Configuration for NVMe SSD

- Used Raspberry Pi Imager to flash the OS onto SD card  
- Booted from SD first, then cloned OS to SSD using `piclone`  
- Edited `/boot/firmware/config.txt` to set boot order:
  ```bash
  sudo -E rpi-eeprom-config --edit
  BOOT_ORDER=0xf146  # Priority: NVMe > USB > SD
  ```
- Verified boot device with:
  ```bash
  lsblk
  mount | grep ' / '
  ```

### 2.3 Troubleshooting Boot from SSD

- Red screen error: "Unable to read partition as FAT"  
- Resolved by ensuring SSD has the correct partition format  
- Final `BOOT_ORDER` used was `0xf146` (where 6 refers to NVMe)  
- Used:
  ```bash
  vcgencmd bootloader_config
  sudo rpi-eeprom-update
  ```

### 2.4 SSD Performance Testing

- Used tools:
  ```bash
  sudo apt install hdparm fio
  sudo hdparm -t /dev/nvme0n1
  dd if=/dev/zero of=testfile bs=1M count=1024 conv=fdatasync status=progress
  ```
- Attempted `iozone3`, needed to:
  ```bash
  sudo apt install iozone3
  ```
- Example performance results:
  - Read: 434 MB/s  
  - Write: 355 MB/s  
  - IOPS Read: 110,702  
  - IOPS Write: 78,467  

---

###  3: Software Installation and Package Management
3.1 Installing Python Modules
bash
Copy
Edit
pip3 install yfinance matplotlib pandas requests beautifulsoup4 python-telegram-bot
3.2 Handling Install Errors
To fix locale issues:

bash
Copy
Edit
sudo apt install locales
sudo dpkg-reconfigure locales
For pip installation restrictions:

bash
Copy
Edit
pip3 install [package] --break-system-packages
3.3 Installing Tools for Benchmarking
bash
Copy
Edit
sudo apt install hdparm fio
###  4: Telegram Chatbot for Stock Analysis
4.1 Creating the Bot
Created bot using BotFather

Retrieved BOT_TOKEN and CHAT_ID

4.2 Bot Features
/full <TICKER> command returns:

Financial summary

Latest stock quote, PE ratio, 52-week high/low

RSI + SMA + Buy/Sell signal chart

Earnings as % of stock price

News links (if available)

4.3 Fixes and Enhancements
Fixed Unicode encoding for financial summary

Combined RSI and stock price in one plot (dual y-axis)

Converted news feed to use clickable links

Error-handling improvements for empty data

Used feedparser, BeautifulSoup, yfinance, and matplotlib

###  5: Monitoring Raspberry Pi Health
5.1 CPU & GPU Monitoring Script
Python script checks load every 30 min

Sends Telegram alert if CPU or GPU > 80%

5.2 Crontab Setup
bash
Copy
Edit
crontab -e
# Add this line:
*/30 * * * * /usr/bin/python3 /home/thomas/check_temp.py
5.3 Health Monitoring Command Line Tools
bash
Copy
Edit
top            # Real-time CPU & memory usage
htop           # Enhanced top interface (if installed)
vcgencmd measure_temp   # Check CPU temp
free -h        # Memory usage
uptime         # System load averages
df -h          # Disk usage
lsblk          # Storage device tree
sudo rpi-eeprom-config --edit  # Bootloader settings
sudo rpi-eeprom-update         # Update bootloader
5.4 Scheduling Regular Shutdown
You can schedule your Raspberry Pi to automatically shut down at a specific time using cron.

Example: Shutdown Daily at 11:30 PM
bash
Copy
Edit
crontab -e
# Add this line:
30 23 * * * /sbin/shutdown -h now
Optional Schedules:
Only weekdays:

arduino
Copy
Edit
30 23 * * 1-5 /sbin/shutdown -h now
Only weekends:

arduino
Copy
Edit
30 23 * * 6,0 /sbin/shutdown -h now
To cancel a scheduled shutdown (if initiated manually), run:

bash
Copy
Edit
sudo shutdown -c
###  6: Advanced Bot Commands and Features
6.1 Planned Enhancements
Smart command alias lookup (e.g. company name → ticker)

Portfolio tracking with summaries

Market screener alerts

Simple web dashboard interface

###  7: Interactive Brokers & Trading API
Interactive Brokers offers TWS API and IB Gateway API

Supports Python via ib_insync or IBAPI

Capable of automated trading, order execution, portfolio tracking

###  8: NAS Functions and File Sharing
Raspberry Pi 5 can act as a lightweight NAS system while keeping other functions like your Telegram bot or GPIO scripts running.

8.1 Options
Samba (SMB) — Easy file sharing with Windows/macOS/Linux

OpenMediaVault — Full-featured NAS system with web GUI

Nextcloud — Personal cloud server

8.2 Recommended: Samba
Samba allows your Pi to expose folders over your local network with low resource usage.

bash
Copy
Edit
sudo apt install samba samba-common-bin
sudo nano /etc/samba/smb.conf
Example share:

ini
Copy
Edit
[PiShare]
   path = /home/thomas/shared
   writeable = yes
   create mask = 0777
   directory mask = 0777
   public = yes
Create the shared directory:

bash
Copy
Edit
mkdir ~/shared
sudo systemctl restart smbd
Access from Finder (Mac): smb://raspberrypi.local/shared
Access from Windows: \\raspberrypi\shared

###  9: Future Expansion
Continue tracking questions asked

Daily chapter update based on new developments

Upcoming plans: add MQTT, Home Assistant, and web interface for full-stack projects


