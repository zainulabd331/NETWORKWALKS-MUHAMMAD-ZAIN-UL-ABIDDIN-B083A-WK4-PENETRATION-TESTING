# NETWORKWALKS-MUHAMMAD-ZAIN-UL-ABIDDIN-B083A-WK4-PENETRATION-TESTING

## Project Overview
This repository contains the comprehensive penetration testing and digital forensics assessment report for Mediroza General Hospital (`medirozahospital.com`), structured chronologically across Modules 1 to 3.

---

## Table of Contents
* [Module 1: Patient Portal Reconnaissance & Access](#module-1-patient-portal-reconnaissance--access)
* [Module 2: Multi-File PDF Password Recovery & Analysis](#module-2-multi-file-pdf-password-recovery--analysis)
* [Module 3: Infrastructure Reconnaissance & Database Exposure](#module-3-infrastructure-reconnaissance--database-exposure)
* [Security Recommendations](#security-recommendations)

---

## Module 1: Patient Portal Reconnaissance & Access
* **Target Endpoint:** Mediroza Hospital Patient Portal (`/patient/portal.php`).
* **Vulnerability:** SQL Injection in the authentication parameter.
* **Payload:** `admin' --`
* **Outcome:** Successfully bypassed backend validation controls, granting unauthorized administrative and viewing access to the patient document repository.

---

## Module 2: Multi-File PDF Password Recovery & Analysis
Following portal access, three encrypted patient laboratory reports were acquired and decrypted:

1. **`patient_report_1.pdf` (Patient: Sipho Dlamini)**
   * **Lab Ref:** `LR-2024-1187` (2024-11-04)
   * **Findings:** Elevated White Cell Count (11.8 x10^9/L).
   * **Password:** `123456`
2. **`patient_report_2.pdf` (Patient: Priya Reddy)**
   * **Lab Ref:** `LR-2024-1192` (2024-11-05)
   * **Findings:** Lipid panel showing elevated Total Cholesterol, LDL Cholesterol, and Triglycerides.
   * **Password:** `password`
3. **`patient_report_3.pdf` (Patient: Emily Thompson)**
   * **Lab Ref:** `LR-2024-1205` (2024-11-06)
   * **Findings:** Low Haemoglobin, Ferritin, and Vitamin D levels.
   * **Hash Configuration:** Extracted via `pdf2john.py` with signed-integer iteration normalization (`4294967292` / `-4`).
   * **Password:** `!@#$%^&`

---

## Module 3: Infrastructure Reconnaissance & Database Exposure
* **Target Application:** Mediroza CMS 1.4.2
* **Enumeration Findings:** Unprotected legacy directories (`/old/` and `/staff/`) hosting exposed administrative scripts and database backups.
* **Exposed Data (`mediroza_db_backup_2019.sql`):**
  * **Staff Table:** 30 complete personnel records including corporate contact details, national IDs, join dates, and exact monthly salaries in ZAR (e.g., Chief Pathologist Dr. Rajesh Naidoo earning R138,000/month; Medical Director Dr. Johan van der Merwe earning R160,000/month).
  * **Shareholders Table:** Corporate equity and share distributions across ordinary and preferential holders (including Cedar Health Holdings and the Reddy Family Trust).

---

## Security Recommendations
1. **Remediate SQL Injection:** Implement parameterized queries (prepared statements) across all input forms to block authentication bypass vectors like `admin' --`.
2. **Secure Directory Permissions:** Disable global web server directory indexing and purge exposed backup scripts from public-facing directories.
3. **Enforce Strong Credentials:** Replace trivial default passwords and weak patient report keys with strict complexity policies.
4. **Upgrade Encryption Frameworks:** Migrate legacy RC4/PDF 1.4 security wrappers to modern AES-256 standards.
