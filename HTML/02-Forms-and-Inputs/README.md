# 📝 02 — Forms and Inputs

> *"Forms are the conversation between user and application."*

---

## 📌 What Is a Form?

A form is how users **talk to your application**. Every login, every search, every "Contact Us" message flows through a form. It's a contract:

1. The user **types** information
2. The user **submits** (presses a button)
3. The server **receives** and **processes** it

If the form is broken, that conversation is broken.

---

## 🏗️ Anatomy of an HTML Form

```
┌─────────────────────────────────────────────────────────────┐
│  <form action="/submit" method="POST">                       │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ LABEL              [ INPUT FIELD              ]      │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ LABEL              [ INPUT FIELD              ]      │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│                         [ SUBMIT BUTTON ]                   │
│                                                             │
│  </form>                                                    │
└─────────────────────────────────────────────────────────────┘
```

**Key attributes:**
- `action`: The URL that receives the data (`/submit`)
- `method`: HTTP method — `GET` (retrieve/search) or `POST` (create/submit sensitive data)

---

## 🎛️ Input Types Reference

| Type | Purpose | Example |
|------|---------|---------|
| `text` | Single-line text | Name, username |
| `email` | Email address (validates format) | user@example.com |
| `password` | Hidden characters | Login passwords |
| `number` | Numeric input with spinner | Age, quantity |
| `tel` | Telephone number | +1-555-0100 |
| `url` | URL input | https://example.com |
| `date` | Date picker | Birth date |
| `checkbox` | Binary on/off | "Remember me" |
| `radio` | Single selection from group | Gender selection |
| `file` | File upload | Profile photo |
| `hidden` | Data sent but not shown | CSRF token |

---

## ✅ What TO Do

### DO: Always Use Labels
```html
<!-- Good: Label explicitly associated with input -->
<label for="email">Email Address</label>
<input type="email" id="email" name="email" required>

<!-- Bad: No label — screen readers can't connect them -->
<input type="email" placeholder="Enter email">
```

### DO: Use `for` and `id` Attributes
```html
<!-- The for="id" connects label to input — click label focuses input -->
<label for="username">Username</label>
<input type="text" id="username" name="username">
```

### DO: Add `required` for Mandatory Fields
```html
<input type="text" name="name" required aria-required="true">
```

### DO: Use Semantic Types for Mobile Keyboards
```html
<!-- On mobile: shows numeric keyboard -->
<input type="tel" placeholder="Phone">

<!-- On mobile: shows email keyboard with @ -->
<input type="email" placeholder="Email">
```

---

## 🚫 What NOT to Do

### DON'T: Use Placeholder as Label
```html
<!-- Bad: Placeholder disappears when user types -->
<input type="text" placeholder="First Name">

<!-- Good: Visible label always present -->
<label for="firstname">First Name</label>
<input type="text" id="firstname" placeholder="e.g., Jane">
```

### DON'T: Use `<div>` for Buttons
```html
<!-- Bad: Not keyboard accessible, no enter key behavior -->
<div onclick="submit()">Submit</div>

<!-- Good: Native button element -->
<button type="submit">Submit</button>
```

### DON'T: Submit Passwords via GET
```html
<!-- Bad: Password visible in URL (bookmarks, logs, browser history) -->
<form method="GET" action="/login">
  <input type="password" name="password">
</form>

<!-- Good: POST sends data in request body, not URL -->
<form method="POST" action="/login">
  <input type="password" name="password">
</form>
```

---

## 🔐 Security Best Practices

### The `autocomplete` Attribute
```html
<!-- Helps browsers autofill correctly (and helps users!) -->
<input type="email" name="email" autocomplete="email">
<input type="password" name="password" autocomplete="current-password">
```

### `type="hidden"` for CSRF Tokens
```html
<!-- Always include a CSRF token in forms that modify data -->
<input type="hidden" name="csrf_token" value="abc123...">
```

---

## 🎯 Why This Matters

### In the Workplace: Accessibility
Without labels, a **screen reader user** has no idea what an input is for. They hear "Edit text" — which is useless. With labels, they hear "Email address, edit text, required."

### In the Workplace: Conversion
Forms that are confusing or lack clear labels **increase drop-off rates**. Users abandon when they don't understand what to enter. Clear labels = more completed signups, purchases, and inquiries.

---

## 🧠 Mental Model: The Reception Desk

| Real-World Reception | HTML Form |
|---------------------|-----------|
| The visitor fills out a form | User types in `<input>` |
| A clerk checks ID and fills in blanks | Browser validates input |
| The clerk stamps and submits to management | Form submits to `action` URL |
| If form is unclear, visitor leaves | Bad forms = user abandonment |

---

## 📚 Technical Glossary

- **`action`**: The endpoint URL that receives form data on submission.
- **`method`**: HTTP verb — `GET` for retrieval, `POST` for creation/update.
- **`required`**: HTML5 attribute that prevents submission if field is empty.
- **`autocomplete`**: Hints to the browser about expected values for autofill.
- **CSRF (Cross-Site Request Forgery)**: An attack where a malicious site submits a form on behalf of a logged-in user. Prevented with CSRF tokens.

---

[⬅️ Previous: 01-Semantic-Web](../01-Semantic-Web/README.md) | [⬅️ Back to Parent](../README.md) | [➡️ Next: 03-SEO-and-Metadata](../03-SEO-and-Metadata/README.md)
