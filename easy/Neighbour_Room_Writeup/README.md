<p align="center">
  <img width="1250" height="280" alt="Image" src="https://github.com/user-attachments/assets/6e3d464b-cb4c-4032-84d6-9aa6aa567972" />
</p>

# THM-Neighbour-Walkthrough: Insecure Direct Object Reference (IDOR)
## Overview
This repository provides a step-by-step walkthrough for the **Neighbour** room on [TryHackMe](https://tryhackme.com/room/neighbour). This is an entry-level challenge focused on identifying and exploiting an **Insecure Direct Object Reference (IDOR)** vulnerability within a web application to retrieve the hidden flag.

* **Room Name:** Neighbour
* **Difficulty:** Easy
* **Core Skills:** Web Application Security, IDOR, Parameter Manipulation.

### Objective

The main objective is to gain unauthorized access to an 'admin' or 'neighbor' user's profile page by manipulating URL parameters after an initial login.

## 1. Initial Access and Reconnaissance
### 1.1. Deploying the Target Machine
The vulnerable machine was deployed via TryHackMe, and the target IP address was obtained (e.g., `10.64.169.133`). Accessing the machine in a web browser reveals a standard login prompt.

<img width="1910" height="1072" alt="Image" src="https://github.com/user-attachments/assets/ee75c148-6774-428e-b76b-107d3be999a1" />
<img width="1910" height="1072" alt="Image" src="https://github.com/user-attachments/assets/39054008-7094-4672-971b-77800628aa69" />

### 1.2. Source Code Analysis

Before attempting to log in, the page's source code was inspected for any exposed information or hints regarding valid credentials.

* **Action:** Viewing the page source (`view-source:http://10.64.169.133/`) or use CTLR U
* **Key Finding:** A crucial HTML comment was discovered at the bottom of the login form, suggesting credentials for a temporary user:
<img width="1910" height="1072" alt="Captura desde 2025-11-24 20-31-59" src="https://github.com/user-attachments/assets/7f0039cf-1798-4a10-ac41-099ea14bd0f8" />
This comment strongly suggested using the credentials: Username: guest / Password: guest

## 2. Gaining Initial Foothold
### 2.1. Logging in as 'guest'
The suggested credentials were used to successfully log into the application.
* **Username** : guest
* **Password** : guest
<img width="1910" height="1072" alt="Captura desde 2025-11-24 20-32-28" src="https://github.com/user-attachments/assets/f74cc4cf-500e-4a88-a363-e1c77036641d" />

### 2.2. Analyzing the Profile URL

Upon successful login, the application redirects the user to their profile page. Crucially, the URL exposes a parameter containing the username:

* URL: http://10.64.169.133/profile.php?user=guest

This structure immediately flags a potential IDOR vulnerability, as the application uses a user-supplied parameter (user=...) to reference a server-side object (the user's profile) without proper access control checks.

## 3. Exploitation: Insecure Direct Object Reference (IDOR)
### 3.1. Parameter Manipulation
The core of the challenge is to exploit the weak access control by manipulating the user parameter to access the profile of the "neighbor" user, who the source code implied was admin.

* Vulnerable Parameter: user=guest
* Exploitation: Change the parameter value to admin.
* Modified URL: http://10.64.169.133/profile.php?user=admin

Navigating to this modified URL resulted in unauthorized access to the administrator's profile page.

<img width="1910" height="1072" alt="Captura desde 2025-11-24 20-33-15" src="https://github.com/user-attachments/assets/d9ae8851-9c97-44ad-b6a0-351c13eb882b" />

## 4. Flag Retrieval and Conclusion
Accessing the administrator's profile page successfully exposed the hidden flag, completing the challenge.

* **Message**: Hi, admin. Welcome to your site. The flag is: flag{66be95c478473d91a5358f2440c7af1f}

<img width="1910" height="1072" alt="Captura desde 2025-11-24 20-33-33" src="https://github.com/user-attachments/assets/07d65723-b528-47b3-a236-b9bd634a9272" />

## Video



https://github.com/user-attachments/assets/59be0e6d-219a-4994-93f5-fbf5c663663f



