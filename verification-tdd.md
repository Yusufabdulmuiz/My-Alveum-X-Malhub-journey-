# Technical Design Document (TDD)

## Feature: NAFDAC Registration Number Verification Service
* **Status:** Proposed
* **Author:** Abū Ridwān 
* **Domain:** Backend API / Database

---

### 1. Feature Overview & Objective
The goal of this feature is to create a backend service that checks if a NAFDAC number entered by a user is authentic. 

This document explains how the application receives the number, safely checks it against our stored records, and sends the correct result back to the user interface. By keeping this verification process separate, we make the app easier to manage and update.

---

### 2. Component Architecture
This feature is built using three main parts that work together:

* **Frontend (React UI):** The web page where the user types the NAFDAC number and clicks "Verify". It handles the visual success or error screens.
* **Backend (Node.js/Express):** The server that receives the number from the frontend, cleans up the text, blocks spammers, and asks the database for a match.
* **Database (PostgreSQL):** The secure storage where all valid NAFDAC numbers and drug details are kept.

---

### 3. Step-by-Step Data Flow
Here is exactly what happens when a user clicks the "Verify" button:

1. **User Request:** The frontend sends the NAFDAC number to the backend server.
2. **Spam Check (Rate Limiter):** The server checks if the user is trying to guess numbers rapidly. If they search too many times in one minute, the server blocks them temporarily.
3. **Data Cleaning:** The server removes any dangerous symbols from the input so hackers cannot break the database (preventing SQL Injection).
4. **Database Search:** The server searches the database for that exact NAFDAC number.
5. **The Result:**
    * **Match Found:** If the number exists, the server tells the frontend it is a real drug and sends the manufacturer's details.
    * **No Match:** If the number does not exist, the server logs the failed attempt and tells the frontend to show a counterfeit warning.

---

### 4. Database Tables
We need two tables in our database to make this work:

#### A. Table: `medication_registry`
This is the main list of all authentic drugs.
* `id` (Primary Key)
* `nafdac_number` (Unique - no duplicates allowed)
* `brand_name` (The name of the drug)
* `active_manufacturer` (The company that makes it)
* `validation_expiry` (When the NAFDAC registration expires)

#### B. Table: `verification_audit_logs`
This is a history book that records every search. We use this to track where fake drugs are being searched from.
* `id` (Primary Key)
* `queried_string` (The exact number the user typed)
* `is_authentic` (True if it was real, False if it was fake)
* `recorded_at` (The exact date and time of the search)

---

### 5. Security Risks & Known Limitations (Technical Debt)

* **Security Risk (Bot Attacks):** Bad actors might use automated bots to guess thousands of numbers until they find real ones. 
  * *Solution:* We will use a Rate Limiter to only allow a few searches per minute per user.
* **Known Limitation (Outdated Data):** The app only knows what is currently inside our database. If NAFDAC approves a new drug today, but we haven't added it to our database yet, the app might falsely tell a user it is fake. In the future, we need a way to automatically update our database with NAFDAC's latest records.
