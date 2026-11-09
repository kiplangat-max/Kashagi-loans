# Kashagi Loans website

A responsive loan enquiry/application website for Zimbabwe residents, with a private staff dashboard and server-side Telegram/email notifications.

## Included

- Public loan application form (USD 10–1,000; Zimbabwe only).
- Captures name, mobile number, optional email, requested amount and purpose.
- Private staff dashboard at `/admin.html` to review applications and update statuses.
- Telegram notifications through a bot and email notifications through SMTP.
- Secrets stay on the server; no bot token is sent to the browser.
- Rate limiting, password-protected admin session, secure cookies in production, Helmet security headers and basic input validation.
- SQLite database. On Render, use the persistent disk configured in `render.yaml`.

## Important safety choices

This form intentionally does **not** request EcoCash PINs, banking passwords, or OTPs. Never ask applicants to disclose those credentials. The site does not promise loan approval or advertise a fixed interest rate; applicants are told to discuss rates and repayment terms directly before agreeing.

Before accepting real applications, add and publish your business identity, eligibility criteria, privacy notice, complaints contact and legally required lending disclosures for the jurisdictions where you operate. Confirm that you are authorized to offer loans to Zimbabwe residents and comply with applicable Zimbabwean law and data-protection requirements.

## Deploy on Render

1. Create a GitHub repository and upload all files in this folder.
2. Sign in to Render and choose **New → Blueprint**.
3. Connect the repository containing `render.yaml`.
4. Render will ask for the secret values marked `sync: false`.
5. Set:
   - `ADMIN_PASSWORD`: a unique password of at least 12 characters.
   - `TELEGRAM_BOT_TOKEN`: your BotFather token.
   - `TELEGRAM_CHAT_ID`: your personal Telegram chat ID.
   - `SMTP_USER`: your Gmail address.
   - `SMTP_PASS`: a Google App Password (not your normal Google password).
   - `SMTP_FROM`: usually the same Gmail address as `SMTP_USER`.
6. Deploy. The persistent disk stores the database and sessions.

Render's persistent disk may require a paid service plan. Do not remove the disk after real applications are stored, or local SQLite data may be lost.

## Telegram setup

1. Open your bot in Telegram and press **Start**.
2. Send it a test message.
3. Obtain your personal numeric chat ID from `@userinfobot`.
4. Put the token and chat ID into Render's Environment settings. Do not paste the token into frontend files, GitHub, or a public chat.

## Email setup (Gmail SMTP)

1. Enable 2-Step Verification on the Google account used to send notifications.
2. Create a Google App Password in your Google Account security settings (if eligible).
3. Use that 16-character App Password as `SMTP_PASS`. Never use your normal Gmail password.
4. `NOTIFY_EMAIL` is prefilled as `Kiplangatm20@gmail.com`.

## Local testing

Requires Node.js 20 or newer.

```bash
npm install
cp .env.example .env
# Fill in all required values in .env; do not commit .env
npm start
```

Open `http://localhost:3000`. To keep `.env` out of Git, add it to `.gitignore`.

## Before launch

- Test with a dummy application and verify both notification channels.
- Change the admin password from any example value.
- Add a privacy policy and terms before collecting personal information.
- Restrict staff dashboard access to authorized staff; use a strong unique password.
- Consider adding verified phone ownership using a legitimate SMS provider. Do not forward one-time codes or PINs to administrators.
