---

# Lab Guide: Password Auditing with John the Ripper

This guide outlines the methodology for evaluating password strength using **John the Ripper (JtR)**. In security auditing, "strength" is verified by the level of effort (time and complexity) required to compromise a hash.

---

## 1. Prerequisites & Environment
*   **Target Tool:** John the Ripper (v1.9.0-Jumbo)
*   **Environment:** Linux (Ubuntu/Kali)
*   **Hash Algorithm:** MD5 (Raw)

### Installation
For the widest support of hash formats (including `raw-md5`), use the Jumbo version via Snap:
```bash
sudo snap install john-the-ripper
```

---

## 2. Experimental Setup
We will test two distinct passwords to verify the difference between "Weak" and "Strong" configurations.

### Create Hash Files
```bash
# Weak Password (common pattern)
printf 'apple123' | md5sum | awk '{print $1}' > weak_hash.txt

# Strong Password (high entropy)
printf 'S@tyam_2026_!' | md5sum | awk '{print $1}' > strong_hash.txt
```

---

## 3. Attack Scenarios

### Scenario A: Testing the Weak Password
We attempt a **Dictionary Attack** using the default wordlist.
```bash
john-the-ripper --format=raw-md5 --wordlist=/snap/john-the-ripper/current/bin/password.lst weak_hash.txt
```
*   **Observation:** The password is recovered instantly. 
*   **Verification:** Classification is **WEAK** because it exists in a standard dictionary list.

### Scenario B: Testing the Strong Password
We attempt the same Dictionary Attack on the complex hash.
```bash
john-the-ripper --format=raw-md5 --wordlist=/snap/john-the-ripper/current/bin/password.lst strong_hash.txt
```
*   **Observation:** The tool completes the wordlist with **0 guesses found**.
*   **Verification:** Classification is **STRONG** against dictionary-based attacks.

---

## 4. Password Strength Detection Table
The following table defines the verification levels based on the successful attack mode:

| Attack Mode | If Cracked... | Verification Level |
| :--- | :--- | :--- |
| **`--single`** | Extremely Weak | The password is based on the username or simple variations. |
| **`--wordlist`** | Weak | The password is a common word or part of a public leak. |
| **`--rules`** | Medium | The password is a word with common substitutions (e.g., `P4ssw0rd`). |
| **`--incremental`** | Strong | The password is complex enough that only brute force works. |

---

## 5. Verification Commands
To display the "trophy" (the cracked password) for your report:
```bash
john-the-ripper --show --format=raw-md5 weak_hash.txt
```

To view the current progress or status of a long-running brute-force session:
*   Press **Enter** or **Space** while the command is running.
*   To abort, press **Ctrl+C**.

---

> **Security Note:** These exercises are for academic purposes. Password auditing should only be performed on systems and data for which you have explicit authorization.