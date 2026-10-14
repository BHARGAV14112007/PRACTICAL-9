# Student Hub 7 - Server-Side Form Processing & JSON Persistence with PHP

> **B.Tech Semester 3 · Practical 7**  
> Server-side registration and contact form data processing using PHP. Includes input sanitization, strict server-side validation, JSON database storage (`LOCK_EX`), and user-friendly success/error messages.

---

## 🌟 Key Features Implemented

### 1. Server-Side Input Sanitization (`php/helpers.php`)
- **String Sanitization (`sanitize_string`)**: Removes full `<script>` and `<style>` blocks, strips HTML tags (`strip_tags`), trims whitespace, and escapes special characters (`htmlspecialchars` with UTF-8).
- **Email Sanitization (`sanitize_email`)**: Cleans invalid characters using `filter_var($email, FILTER_SANITIZE_EMAIL)`.
- **Numeric & Phone Sanitization**: Strips non-digit characters via regex `preg_replace('/[^0-9]/', '', $mobile)`.

### 2. Strict Server-Side Validation
- **Registration Form (`php/process_register.php`)**:
  - **Full Name**: Required, 2–60 characters, regex `/^[A-Za-z]+(?:[ '.-][A-Za-z]+)*$/`.
  - **Email Address**: Required, validated via `filter_var(..., FILTER_VALIDATE_EMAIL)` and duplicate email checking against `data/students.json`.
  - **Mobile Number**: Required 10-digit Indian/campus phone format starting with digits 6–9 (`/^[6-9][0-9]{9}$/`).
  - **Course & Academic Year**: Whitelist validation against allowed courses (`BCA`, `BBA`, `B.Tech`, `Other`) and years (`First Year`, `Second Year`, `Third Year`, `Final Year`).
  - **Gender**: Whitelist validation (`Male`, `Female`, `Prefer not to say`).
  - **Password & Confirmation**: Minimum 8 characters, exact match required, securely hashed via `password_hash($password, PASSWORD_DEFAULT)`.
  - **Terms & Conditions**: Explicit acceptance verification.
- **Contact Form (`php/process_contact.php`)**:
  - **Name**: Required, min 2 characters.
  - **Email**: Format validation via `FILTER_VALIDATE_EMAIL`.
  - **Subject**: Required, 3–120 characters.
  - **Message**: Required, 10–3000 characters.

### 3. File-Based JSON Storage
- **Student Records (`data/students.json`)**: Automatically assigns unique sequential student IDs (`STU-1001`, `STU-1002`, ...), maps academic department and semester, and writes atomically using `file_put_contents` with exclusive file lock `LOCK_EX`.
- **Contact Submissions (`data/contacts.json`)**: Assigns unique message ticket IDs (`MSG-1001`, `MSG-1002`, ...), records timestamp, client IP, and query details into JSON file format.

### 4. User Feedback & Message Display
- **Progressive Enhancement**: Supports both standard HTTP POST submissions (with session flash banners, error lists, and sticky field values) and asynchronous AJAX / Fetch API submissions (with immediate inline feedback and JSON responses).
- **Interactive JSON Records Explorer (`pages/view_records.php`)**: Provides live cards and raw JSON previews of registered students and contact queries.

---

## 📁 Project Structure

```
student hub 7/
├── data/
│   ├── students.json         # JSON database storing registered student records
│   ├── contacts.json         # JSON database storing submitted contact queries
│   ├── events.json           # Categorized campus events dataset
│   └── faqs.json             # Categorized campus FAQs dataset
├── php/
│   ├── helpers.php           # Core sanitization, validation & JSON file handler
│   ├── process_register.php  # Registration form processor & JSON writer
│   └── process_contact.php   # Contact form processor & JSON writer
├── pages/
│   ├── register.php          # Dynamic registration page with PHP validation feedback
│   ├── register.html         # HTML registration page with AJAX PHP integration
│   ├── contact.php           # Dynamic contact page with PHP validation feedback
│   ├── contact.html          # HTML contact page with AJAX PHP integration
│   ├── view_records.php      # Live JSON records dashboard (Students & Messages)
│   ├── profile.html          # Dynamic student profile page
│   ├── event.html            # Campus events page
│   ├── faq.html              # Campus FAQ accordion page
│   ├── login.html            # Login page
│   ├── home.html             # Homepage
│   └── admin.html            # Admin dashboard
├── js/
│   ├── script.js             # Form validation & AJAX submission controller
│   ├── header.js             # Navigation & theme sync
│   └── profile-page.js       # Dynamic profile & student directory renderer
└── css/
    └── style.css             # Unified modern design system
```

---

## 🚀 How to Run with PHP

Start PHP's built-in development server in the project directory:

```bash
php -S 127.0.0.1:8000
```

Then open your browser at:
- **Registration Form:** `http://127.0.0.1:8000/pages/register.php` (or `register.html`)
- **Contact Form:** `http://127.0.0.1:8000/pages/contact.php` (or `contact.html`)
- **JSON Records Explorer:** `http://127.0.0.1:8000/pages/view_records.php`
