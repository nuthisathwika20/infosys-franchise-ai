
# Infosys Springboard Internship 7.0 – Milestone 1

## User Authentication Module (Streamlit + JWT + ngrok + Gmail OTP)

## 📌 Project Overview

This project implements a secure and user-friendly **User Authentication Module** using Python and Streamlit as part of the **Infosys Springboard Internship 7.0 – Milestone 1**.

The application provides secure user registration, login, password recovery, JWT-based session management, account lockout protection, and role-based dashboards. It integrates a corporate UI theme using Infosys Blue (`#007cc3`) and is designed to run in Google Colab with public access through ngrok.

## ✨ Features

### 1. User Sign Up
- Mandatory field-by-field validation for all input fields.
- Strict email format validation (`ab@cd.ef` structure).
- Password complexity enforcement:
  - Minimum 8 characters.
  - At least one uppercase letter.
  - At least one lowercase letter.
  - At least one digit.
  - At least one special character.
- Security question selection and case-sensitive answer verification.
- Secure password and security answer hashing using `bcrypt`.
- Database-level unique constraints for usernames and email addresses.

### 2. User Login & Account Lockout Protection
- Secure authentication using `bcrypt` password hashing.
- Tracks failed login attempts, including blank-field submissions.
- Automatically locks accounts after 5 failed attempts.
- Enforces a **10-minute account lockout**.
- Displays the following message when an account is locked:

  `Account is blocked for 10 minutes. Try again later.`

- Generates cryptographically signed JSON Web Tokens (JWTs).
- JWT session tokens expire after 2 hours.

### 3. Forgot Password & Account Recovery

#### Security Question Recovery
- Allows users to reset passwords by answering their registered security question.
- Uses hashed security answers for verification.
- Enforces strict case-sensitive matching.
- Example: `DOG` and `dog` are treated as different answers.

#### Gmail OTP Recovery
- Generates a 6-digit One-Time Password (OTP).
- Uses a short-lived JWT token with a 5-minute expiry to protect the OTP verification flow.
- Sends verification codes through Gmail SMTP.
- Uses Gmail App Password authentication.

#### Recovery Rate Limiting
- Tracks failed password recovery attempts.
- Allows a maximum of 5 failed attempts before applying a 10-minute lockout.
- Helps prevent brute-force attacks against the recovery workflow.

### 4. User Dashboard
- Protected by JWT session verification.
- Displays Plotly-based performance index gauges.
- Shows activity indicators and performance metrics.
- Provides an authenticated user interface.

### 5. Admin Dashboard
- Supports role-based access control.
- Uses a unified SQLite database for regular users and administrators.
- Differentiates account types through a `role` attribute:
  - `user`
  - `admin`
- Displays an aggregated user registry table.
- Excludes password hashes and security answer hashes from the displayed registry.

### 6. Public Tunneling & Secret Management
- Uses `pyngrok` to expose the local Streamlit application through a public HTTPS URL.
- Integrates with Google Colab's Secrets vault.
- Keeps sensitive configuration values outside the application source code.

## 🛠️ Tech Stack

| Category | Technologies |
|---|---|
| Programming Language | Python |
| Frontend / UI | Streamlit |
| UI Components | Streamlit Option Menu |
| Data Visualization | Plotly |
| Authentication | JWT (PyJWT) |
| Password Hashing | bcrypt |
| Database | SQLite3 |
| Email Service | Gmail SMTP (`smtplib`) |
| Public Tunneling | pyngrok |
| Development Environment | Google Colab |

## 📂 Project Structure

```text
Milestone1/
│
├── screenshots/
│   ├── Admin login.jpeg
│   ├── Admin dashboard.jpeg
│   ├── Create Account.jpeg
│   ├── User login.jpeg
│   ├── Reset Password.jpeg
│   ├── Reset Password by OTP.jpeg
│   └── Reset Password by Security Question.jpeg
│
├── README.md
└── app.ipynb
```

## 🔐 Setup & Configuration

### Step 1: Open the Notebook

Open `app.ipynb` in Google Colab.

### Step 2: Configure Google Colab Secrets

Open the **Secrets** tab (🔑) in Google Colab and add the following secrets. Enable notebook access for each secret.

| Secret Key | Description |
|---|---|
| `JWT_SECRET` | Secret key used to sign JWT session and OTP tokens. |
| `NGROK_AUTHTOKEN` | Personal authentication token obtained from the ngrok dashboard. |
| `EMAIL_ADDRESS` | Gmail address used to send OTP verification emails. |
| `EMAIL_PASSWORD` | 16-character Gmail App Password used for SMTP authentication. |

**Important:** Never commit actual secret values, Gmail App Passwords, or JWT signing keys to a public repository.

### Step 3: Install Dependencies

Run the following command in a Google Colab code cell:

```bash
pip install streamlit pyjwt bcrypt pyngrok plotly streamlit-option-menu
```

### Step 4: Initialize the Application

Execute the notebook cells to:

1. Load the required dependencies.
2. Read configuration values from Google Colab Secrets.
3. Generate the `app.py` application file.
4. Initialize the SQLite database (`infosys_portal.db`).
5. Create the required database tables and constraints.

### Step 5: Launch the Application

Run the Streamlit application and execute the ngrok tunneling cell to generate a public HTTPS URL.

Open the generated URL in your browser to access the authentication module.

> **Note:** Keep the Colab notebook session running while using the public URL. The URL may change when a new ngrok tunnel is created.

## 📸 Screenshots

The following screenshots demonstrate the application's user interface and authentication workflows.

### 1. Admin Dashboard

Displays the administrator interface and aggregated user registry.

![Admin Dashboard](screenshots/Admin%20dashboard.jpeg)

### 2. Admin Login

Displays the administrator authentication page.

![Admin Login](screenshots/Admin%20login.jpeg)

### 3. Create Account

Demonstrates the user registration form and input validation.

![Create Account](screenshots/Create%20Account.jpeg)

### 4. Reset Password by OTP

Demonstrates email-based OTP verification for password recovery.

![Reset Password by OTP](screenshots/Reset%20Password%20by%20OTP.jpeg)

### 5. Reset Password by Security Question

Demonstrates case-sensitive security answer verification.

![Reset Password by Security Question](screenshots/Reset%20Password%20by%20Security%20Question.jpeg)

### 6. Reset Password

Displays the password reset interface.

![Reset Password](screenshots/Reset%20Password.jpeg)

### 7. User Login Page

Displays the user authentication interface.

![User Login](screenshots/User%20login.jpeg)

## 🔒 Security Highlights

- **Password Security:** Passwords are hashed using `bcrypt` rather than stored in plaintext.
- **Security Answer Protection:** Security answers are stored as hashes.
- **JWT Authentication:** Signed tokens support session verification and expiry enforcement.
- **Account Lockout:** Failed login and recovery attempts trigger temporary lockouts.
- **Role-Based Access Control:** User and administrator access is separated by role.
- **Input Validation:** Registration and authentication inputs are validated.
- **Secret Management:** Credentials are configured through Google Colab Secrets.
- **Data Minimization:** Sensitive hashes are excluded from the administrator's user registry display.

> **Security note:** For production deployment, additionally enforce HTTPS, use strong randomly generated signing keys, implement server-side rate limiting, protect against account enumeration, and securely manage JWT revocation and OTP replay. A publicly accessible ngrok tunnel alone does not make an application production-ready.

## 🎯 Learning Outcomes

Through this project, the following concepts are demonstrated:

- User authentication and authorization.
- Secure password hashing with `bcrypt`.
- JWT generation, validation, and expiration.
- OTP-based email verification.
- Password recovery using security questions.
- Login attempt tracking and account lockout mechanisms.
- SQLite database design and integrity constraints.
- Role-based access control.
- Streamlit application development.
- Public application tunneling with ngrok.
- Secret management in Google Colab.

## ✅ Conclusion

The **Infosys Springboard Internship 7.0 – Milestone 1** project demonstrates the implementation of a User Authentication Module using Python, Streamlit, SQLite, JWT, and Gmail OTP verification.

It combines user registration, secure login, account recovery, rate limiting, temporary account lockouts, and role-based dashboards within an Infosys-inspired corporate interface.

The project serves as a foundation for learning secure web application development and authentication workflows.


