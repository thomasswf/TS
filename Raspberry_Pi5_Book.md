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

(Chapters 3 through 8 truncated for brevity)