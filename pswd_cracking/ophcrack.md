# Lab Guide: Password Auditing with Ophcrack

## 1. Concept: Rainbow Tables & Space-Time Tradeoff

Ophcrack is a specialized tool that uses **Rainbow Tables** to crack passwords. Unlike brute-force methods that calculate every hash on the fly, Rainbow Tables use a "Space-Time Tradeoff". 

*   **Precomputation:** Massive databases of hashes are created in advance (Space).
*   **Instant Lookup:** During the audit, the tool simply "looks up" the hash in the database (Time).
*   **Cryptographic Chains:** These tables use reduction functions to store starting and ending points of hash chains, significantly reducing the disk space required compared to a raw hash-to-password list.



---

## 2. Environment Setup

### Installation
Ensure the tool is installed on your Linux environment:
```bash
sudo apt update
sudo apt install ophcrack
```

### Rainbow Table Requirements
Ophcrack requires external table files (e.g., `vista_free` or `xp_free`) to function. Without these tables loaded, the tool will result in a "Not Found" status.

---

## 3. Hash Preparation (PWDUMP Format)

Ophcrack expects hashes in the **PWDUMP** format, which includes the username, RID, LM hash, and NTLM hash.

### Generate a Test Hash File
Use Python to generate an NTLM hash (MD4 + UTF-16LE) and save it in the correct format:
```bash
# Define user and password
USER="Satyam"
PASS="apple123"

# Generate NTLM hash and create win_hashes.txt
NT_HASH=$(python3 -c "import hashlib,binascii; print(binascii.hexlify(hashlib.new('md4', '$PASS'.encode('utf-16le')).digest()).decode())")
printf "$USER:1001:aad3b435b51404eeaad3b435b51404ee:$NT_HASH:::\n" > win_hashes.txt
```

---

## 4. Loading Rainbow Tables

To perform the audit, you must point Ophcrack to your table data.

### GUI Method
1.  Launch the tool: `sudo ophcrack`.
2.  Click **Tables** -> **Install**.
3.  Select the directory containing the table files.
4.  **Verification:** In the table selection window (as seen in `image_0d9414.png`), the status indicator must turn **Green**.

### CLI Method
```bash
ophcrack -f win_hashes.txt -t /path/to/your/tables
```

---

## 5. Password Strength Verification Table

Subject the hash to the audit and use the result to determine the verification level:

| Result in Ophcrack | If Cracked... | Verification Level |
| :--- | :--- | :--- |
| **Instant Match** | Extremely Weak | The password is short (under 7–8 chars) and exists in the smallest tables. |
| **Cracked (Multi-table)** | Weak | The password is a common alphanumeric string found in standard free tables. |
| **Partial Crack** | Medium | Only the LM portion was cracked, or the password uses characters not in your current table. |
| **Not Found** | Strong | The password is either too long (>14 chars) or uses complex symbols not covered by the table. |

---

## 6. Interpreting "Not Found"

If the audit returns "Not Found" (as seen in `image_0d9bb6.png`), it indicates one of the following:
*   **Table Limitation:** The password contains characters or is a length not indexed in the current Rainbow Table.
*   **Salting:** Rainbow Tables are ineffective against "salted" hashes because the salt changes the output, requiring a new table for every possible salt.
*   **Resilience:** The password possesses sufficient entropy to resist precomputed lookup attacks.

---

> **Note:** For modern Windows environments (Vista and above), always prioritize **Vista** tables over XP tables, as they target the **NTLM** hash format rather than the legacy LM format.

One single follow-up: Since your previous scan returned "not found," were you able to locate the `vista_free` table folder on your system to install it?