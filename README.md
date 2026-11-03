# Web Server Security Lab
## Exploitation and Mitigation of Four Web Vulnerability Categories

**Course:** Cyber Security (IT-30001)    
**Project type:** Group assignment  
**Application stack:** Python, Flask, SQLite, HTML, CSS, Jinja2

---

## 1. Project Overview

This project demonstrates four web application vulnerability categories through five independent variants. Each module contains a deliberately vulnerable implementation, a controlled demonstration, and a corresponding mitigation.

The project is intended for an academic demonstration of:
- SQL Injection (SQLi)
- OS Command Injection
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)

Each variant runs as a separate Flask application on its own localhost port. This allows group members to demonstrate and explain their assigned implementation independently.

> **Safety notice:** These applications intentionally contain vulnerable routes. Run them only on your own computer and test only against these local applications. Do not deploy them, expose them to a public or shared network, or use real credentials or personal data. The development servers use Flask debug mode and are not production servers.

---

## 2. Group Allocation and Ownership

The uploaded project contains the following five variants. Fill in the member names and roll numbers before submission.

| Member | Vulnerability category | Variant | Folder | Port |


| Member 1 | SQL Injection | Authentication bypass | `sqli_auth_bypass/` | 5001 |

| Member 2 | OS Command Injection | In-band (command output returned) | `cmdi_inband/` | 5002 |

| Member 3 | Cross-Site Scripting (XSS) | Reflected XSS | `xss_reflected/` | 5003 |

| Member 4 | CSRF | GET-based forged action | `csrf_get/` | 5004 |

| Member 5 | CSRF | POST-based forged action (auto-submitting form) | `csrf_post/` | 5005 |

### Ownership declaration

| Varshith | IIT2024151 | SQL Injection | Authentication bypass |

| Thanishq | IIT2024197 | OS Command Injection | In-band |

| Vikram | IIT2024116 | XSS | Reflected XSS |

| Manoj Kumar|IIT2024148| CSRF | GET-based forged action |

| Jaswanth | IIT2024189 | CSRF | POST-based forged action |

The member listed for each variant should be the person who implemented it and can independently explain its vulnerable code, exploit, root cause, mitigation, and alternative mitigations during the viva.

---

## 3. Project Structure

```text
cs_assign/
├── requirements.txt
├── run_all.py
│
├── sqli_auth_bypass/
│   ├── app.py
│   ├── init_db.py
│   ├── app.db
│   ├── README.md
│   └── templates/
│
├── cmdi_inband/
│   ├── app.py
│   ├── README.md
│   └── templates/
│
├── xss_reflected/
│   ├── app.py
│   ├── README.md
│   └── templates/
│
├── csrf_get/
│   ├── app.py
│   ├── init_db.py
│   ├── app.db
│   ├── README.md
│   └── templates/
│
└── csrf_post/
    ├── app.py
    ├── init_db.py
    ├── app.db
    ├── README.md
    └── templates/
```

Each module's own README contains its detailed vulnerable implementation, demonstration steps, root cause, mitigation, and theory of other mitigation options.

---

## 4. Requirements

- Python 3.8 or later
- `pip`
- A modern web browser
- Linux or Ubuntu/WSL is recommended for the command-injection demonstration because it uses the Linux `ping -c` option.

Python dependencies are listed in `requirements.txt`:
- Flask
- MarkupSafe
- Bleach

---

## 5. Installation

Open a terminal in the `cs_assign` project root.

### Linux / Ubuntu / WSL

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

If the virtual environment already exists, activate it and install the requirements:

```bash
source venv/bin/activate
pip install -r requirements.txt
```

### Windows PowerShell

```powershell
py -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

---

## 6. Running the Applications

### Option A — Run one assigned variant

Run each module from its own directory. Open a separate terminal for each server you want to run.

**1. SQL Injection — Authentication Bypass**

```bash
cd sqli_auth_bypass
python init_db.py
python app.py
```

Open: `http://127.0.0.1:5001`

**2. OS Command Injection — In-band**

```bash
cd cmdi_inband
python app.py
```

Open: `http://127.0.0.1:5002`

**3. Reflected XSS**

```bash
cd xss_reflected
python app.py
```

Open: `http://127.0.0.1:5003`

**4. CSRF — GET-based forged action**

```bash
cd csrf_get
python init_db.py
python app.py
```

Open: `http://127.0.0.1:5004`

**5. CSRF — POST-based forged action**

```bash
cd csrf_post
python init_db.py
python app.py
```

Open: `http://127.0.0.1:5005`

Stop an individual server with `Ctrl + C`.

### Option B — Run all five applications

The supplied `run_all.py` starts all five servers and expects a virtual environment at `cs_assign/venv`.

From the project root, after creating and activating that environment and installing dependencies, run:

```bash
python run_all.py
```

The applications will be available at:

| Module | Local URL |
|---|---|
| SQL Injection | `http://127.0.0.1:5001` |
| OS Command Injection | `http://127.0.0.1:5002` |
| Reflected XSS | `http://127.0.0.1:5003` |
| CSRF GET | `http://127.0.0.1:5004` |
| CSRF POST | `http://127.0.0.1:5005` |

Press `Ctrl + C` in the `run_all.py` terminal to stop all five servers.

---

## 7. Vulnerability and Mitigation Summary

| Category / variant | Vulnerable behaviour demonstrated | Mitigation implemented |
|---|---|---|
| SQL Injection — authentication bypass | User input is concatenated into a SQL query, allowing the query logic to be altered. | Parameterised SQL query with bound parameters. |
| OS Command Injection — in-band | User input is incorporated into a shell command and command output is returned in the response. | Allow-list validation and argument-vector execution with `shell=False`. |
| Reflected XSS | Request query input is reflected into an HTML response without safe output encoding. | HTML output encoding and normal Jinja2 auto-escaping. |
| CSRF — GET-based forged action | A state-changing account action is triggered by a GET request without proof of user intent. | Require POST and validate a synchronizer CSRF token. |
| CSRF — POST-based forged action | A state-changing POST request is accepted without CSRF token verification. | Validate a per-session synchronizer CSRF token. |

The exact implementation and demonstration procedure are documented in the README inside each module.

---

## 8. Demonstration and Viva Preparation

For **each assigned variant**, the responsible member should be ready to show and explain:

1. **Vulnerable implementation:** Identify the exact source-code path containing the flaw.
2. **Working exploit:** Demonstrate the controlled test against the group's own local application.
3. **Root cause:** Explain why the vulnerable code accepts or executes the input.
4. **Mitigation implemented:** Identify the code change that addresses the vulnerability.
5. **Verification:** Repeat the same test against the mitigated route and show that it is blocked or handled safely.
6. **Other mitigations (theory):** Explain the alternative mitigation techniques listed in the course assignment.

### Suggested video structure

Allow approximately 2–3 minutes per member:
- Introduce the vulnerability and assigned variant.
- Show the vulnerable code and local application.
- Demonstrate the controlled exploit.
- Explain the root cause.
- Show the mitigation and repeat the test.
- Briefly mention alternative mitigations.

Each member should narrate their own segment so individual contributions are clear.

---

## 9. Mitigation Alternatives to Study

In addition to the mitigation implemented in each module, study the alternatives listed in the assignment:

- **SQL Injection:** Parameterised queries, input validation with allow-lists, least-privilege database accounts, and safe error handling.
- **OS Command Injection:** Avoid the shell, use parameterised execution with an argument vector, allow-list validation, and least privilege.
- **XSS:** Context-aware output encoding, framework auto-escaping, HTML sanitisation, Content Security Policy (CSP), HttpOnly cookies, and safe DOM sinks.
- **CSRF:** Synchronizer anti-CSRF tokens, SameSite cookies, re-authentication for sensitive actions, and avoiding state-changing GET requests.

---

## 10. Submission Checklist

- [ ] Confirm the five member names and roll numbers in the ownership declaration.
- [ ] Ensure each member's assigned implementation is present in the repository.
- [ ] Test every vulnerable route in the local lab.
- [ ] Test the corresponding mitigation using the same demonstration input.
- [ ] Capture screenshots of the vulnerable and mitigated results.
- [ ] Record one group video with each member narrating their own 2–3 minute segment.
- [ ] Include this README and the module-specific READMEs in the GitHub repository.
- [ ] Upload the video to the group-specific Google Drive subfolder as instructed in Classroom.
- [ ] Include the assignment PDF in that Drive subfolder.
- [ ] Submit the GitHub repository link in the designated Google Sheet.

---

## 11. Academic Integrity and Limitations

This is a deliberately vulnerable educational lab. Its purpose is to demonstrate security weaknesses and compare them with defensive implementations. It is not intended for production use.

The group must be able to explain its own code and demonstrate its own work during the individual viva. Do not claim that a mitigation is effective without testing it. Do not test shared classroom infrastructure, hosting providers, third-party websites, or systems outside the group's own application.

**Project:** Web Server Security — Exploitation and Mitigation of Four Vulnerability Categories  
**Course:** Cyber Security (IT-30001)  
