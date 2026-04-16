# 🗂️ PICO Cloud – Multi-Tenant Samba File Server (Ubuntu 24.04)

## 📌 Overview

This project demonstrates the implementation of a **centralized, multi-tenant Samba file server** designed to simulate a cloud storage provider (**PICO Cloud**).

The system provides **secure, role-based access control (RBAC)** for multiple office locations, ensuring that users can only access their designated storage while administrators maintain full visibility.

---

## 🏗️ Architecture Summary

* **Platform:** Ubuntu 24.04 (Server)
* **File Sharing Protocol:** SMB/CIFS (Samba)
* **Storage Management:** LVM (Logical Volume Manager)
* **Access Control:** Linux Groups + Samba Authentication
* **Network Access:** TCP Port 445 (SMB)

---

## 🎯 Objectives

* Provide **centralized storage** for multiple office branches
* Enforce **strict access isolation** between departments
* Enable **administrator-level full access**
* Ensure **secure authentication (no anonymous access)**
* Allocate **dedicated storage per office using LVM**

---

## 🧑‍💻 User Roles & Access Control

| Role          | Access                   |
| ------------- | ------------------------ |
| Mirpur Users  | Mirpur + Common          |
| Gulshan Users | Gulshan + Common         |
| Uttara Users  | Uttara + Common          |
| Admin         | Full Access (All Shares) |

---

## 💾 Storage Design (LVM)

The storage is divided using LVM for better scalability and isolation:

```
samba_vg (Volume Group)
├── mirpur_lv   (20GB) → /srv/samba/mirpur
├── gulshan_lv  (20GB) → /srv/samba/gulshan
├── uttara_lv   (20GB) → /srv/samba/uttara
├── common_lv   (40GB) → /srv/samba/common
└── admin_lv    (remaining space) → /srv/samba/admin
```

---

## 📂 Samba Shares

| Share Name | Path               | Access                 |
| ---------- | ------------------ | ---------------------- |
| Mirpur     | /srv/samba/mirpur  | mirpur_grp, admin_grp  |
| Gulshan    | /srv/samba/gulshan | gulshan_grp, admin_grp |
| Uttara     | /srv/samba/uttara  | uttara_grp, admin_grp  |
| Common     | /srv/samba/common  | All groups             |
| Admin      | /srv/samba/admin   | admin_grp only         |

---

## 🔐 Security Features

* 🔒 User-based authentication (`smbpasswd`)
* 🚫 Guest access disabled
* 🧩 Group-based authorization
* 🔑 Restricted share visibility
* 🌐 SMB protocol limited to secure versions
* 🔥 Firewall configured for Samba ports only

---

## 🌐 Network Access

Clients can access the server using:

```
\\<server-ip>
```

Example:

```
\\<ip>\<username>
```

---

## ⚠️ Important Notes

* Windows does not allow multiple sessions to the same server with different credentials simultaneously
* Use `net use * /delete` to clear sessions before switching users
* Samba should not be exposed directly to the public internet without VPN

---

## 🧪 Testing

Each user is assigned:

* A dedicated folder within their department share
* Access validation through real-time file operations


---

## 📚 Technologies Used

* Ubuntu 24.04
* Samba
* LVM
* Linux File Permissions & ACL
* Networking (SMB/CIFS)

---

NB. Documentation organized by ChatGpt for better visualization 
