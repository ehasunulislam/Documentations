# Nodemailer

This guide provides step-by-step instructions for setting up and using **Nodemailer** in a Node.js application with **OAuth2 authentication** using `ClientID` and `ClientSecret`, along with generating a refresh token using the **OAuth 2.0 Playground**.

---

# Prerequisites

* Node.js is installed on your machine.
* A Google account to generate OAuth2 credentials (`ClientID` and `ClientSecret`).
* Access to the Google API Console to create OAuth2 credentials.

---

# Getting OAuth2 Credentials

### 1. Go to the Google API Console

* Navigate to the **Google API Console**
* Create a new project or select an existing one.

### 2. Enable Gmail API

* Go to the **Library** section.
* Search for **Gmail API** and enable it.

### 3. Create OAuth2 Credentials

* Go to the **Credentials** section.
* Click **Create Credentials** and choose **OAuth 2.0 Client ID**.
* Set the application type to **Web Application**.
* Under **Authorized redirect URIs**, add:

```
http://localhost
https://developers.google.com/oauthplayground
```

* After creating, you will get your **ClientID** and **ClientSecret**.

---

# Generating the Refresh Token Using OAuth 2.0 Playground

### 1. Access OAuth 2.0 Playground

Open the **OAuth 2.0 Playground** in your browser.

### 2. Configure OAuth 2.0 Playground

* In the top-right corner click the **settings icon**.
* Under **OAuth 2.0 configuration**, select **Use your own OAuth credentials**.
* Enter your `ClientID` and `ClientSecret`.
* Set **Access type** to `offline` to obtain a refresh token.

### 3. Select Scopes

In Step 1 select the Gmail scope:

```
https://mail.google.com/
```

### 4. Authorize APIs

Click **Authorize APIs** and log in with your Google account.

### 5. Exchange Authorization Code for Tokens

Click **Exchange authorization code for tokens**.

This will generate:

* Access Token
* Refresh Token

### 6. Copy Refresh Token

Copy the **refresh token** and save it in your `.env` file.

---

# Installation

### 1. Initialize a Node.js Project

```bash
npm init -y
```

### 2. Install Nodemailer

```bash
npm install nodemailer
```

### 3. Install dotenv (for environment variables)

```bash
npm install dotenv
```

---

# Configuration

## 1. Create a `.env` File

Create a `.env` file in the root of your project.

```
CLIENT_ID=your_client_id
CLIENT_SECRET=your_client_secret
REFRESH_TOKEN=your_refresh_token
EMAIL_USER=your_email@gmail.com
```

Replace the placeholders with your actual OAuth2 credentials.

---

## 2. Set Up Nodemailer with OAuth2

Create a file `email.js`.

```javascript
require("dotenv").config();
const nodemailer = require("nodemailer");

const transporter = nodemailer.createTransport({
  service: "gmail",
  auth: {
    type: "OAuth2",
    user: process.env.EMAIL_USER,
    clientId: process.env.CLIENT_ID,
    clientSecret: process.env.CLIENT_SECRET,
    refreshToken: process.env.REFRESH_TOKEN,
  },
});

transporter.verify((error, success) => {
  if (error) {
    console.error("Error connecting to email server:", error);
  } else {
    console.log("Email server is ready to send messages");
  }
});

module.exports = transporter;
```

---

## 3. Create a Function to Send Emails

Add this function in the same `email.js` file.

```javascript
const sendEmail = async (to, subject, text, html) => {
  try {
    const info = await transporter.sendMail({
      from: `"Your Name" <${process.env.EMAIL_USER}>`,
      to,
      subject,
      text,
      html,
    });

    console.log("Message sent:", info.messageId);
    console.log("Preview URL:", nodemailer.getTestMessageUrl(info));
  } catch (error) {
    console.error("Error sending email:", error);
  }
};

module.exports = sendEmail;
```

---

## 4. Use the Email Function in Your Application

Example usage in your main file (`app.js`).

```javascript
const sendEmail = require("./email");

sendEmail(
  "recipient@example.com",
  "Test Email Subject",
  "This is a test email sent with Nodemailer using OAuth2.",
  "<p>This is a test email sent with <b>Nodemailer</b> using OAuth2.</p>"
);
```

---

# Running the Application

Run your Node.js application:

```bash
node app.js
```

If everything is configured correctly, you should see a message indicating that the email was sent successfully.

---

# Troubleshooting

**Invalid Credentials Error**

* Ensure your `CLIENT_ID`, `CLIENT_SECRET`, and `REFRESH_TOKEN` are correct.
* Make sure they belong to the same Google account.

**Email Not Sent**

* Check if the OAuth2 scopes include Gmail API access.
* Verify that the refresh token has the necessary permissions.

---

# References

1. Nodemailer Documentation
   https://nodemailer.com/

2. Google OAuth2 Documentation
   https://developers.google.com/identity/protocols/oauth2

3. OAuth 2.0 Playground
   https://developers.google.com/oauthplayground/
