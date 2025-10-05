
## Email Send API

> Live Deployment: <https://emailapi-o5je.onrender.com>

Fork adapted from the original SendMail API project. This service provides templated email sending (subscription, contact, quotation) with Gmail OAuth2 + Nodemailer.

## Features

- Dynamic HTML + text email templates
- Dual delivery (internal admin + user confirmation)
- Gmail OAuth2 (no plain passwords)
- File attachment handling (quotation artwork)
- Simple JSON REST endpoints

## Tech Stack

| Layer | Tool |
|-------|------|
| Runtime | Node.js |
| Framework | Express |
| Mail | Nodemailer + Gmail OAuth2 |
| Uploads | Multer |
| Config | dotenv |

## Endpoints Overview

| Method | Path | Purpose |
|--------|------|---------|
| GET | / | Basic service response |
| GET | /health | Health check |
| POST | /subscribe-email | Newsletter subscription |
| POST | /contact-number | Collect mobile number only |
| POST | /contact-email | Full contact form submission |
| POST | /quotation-email | Quotation with optional file upload |

### Example: Subscribe

Request:

```json
{ "email": "user@example.com" }
```

Response:

```json
{ "message": "Email sent successfully" }
```

### Example: Contact

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "message": "Need pricing details",
  "subject": "Pricing",
  "mobile": "+1234567890"
}
```

### Example: Quotation multipart fields

```text
id, productName, lengthh, width, height (optional), quantity, dynamicFields (JSON string), note, artworkName, name, email, phone, estimatedPrice, artwork (file)
```

## Environment Variables

Create a `.env`:

```dotenv
CLIENT_ID="Your Google OAuth Client Id"
CLIENT_SECRET="Your Google OAuth Client Secret"
REDIRECT_URL="https://developers.google.com/oauthplayground"
REFRESH_TOKEN="Your Gmail API Refresh Token"
CarbonCopy="admin@yourdomain.com"
PORT=3000
FRONTEND_URL="http://localhost:5173"
NODE_ENV="development"
```

### Getting OAuth2 Credentials (Gmail)

1. Google Cloud Console → Create project
2. Enable Gmail API
3. Configure OAuth consent screen (External)
4. Create OAuth Client (Web) with redirect URI: `https://developers.google.com/oauthplayground`
5. Use OAuth Playground with scope: `https://www.googleapis.com/auth/gmail.send`
6. Exchange code → copy Refresh Token into `.env`

## Run Locally

```bash
git clone https://github.com/Kevin-kemboi/EmailAPI.git
cd EmailAPI
cp .env.example .env # then edit secrets
npm install
npm start
```

## Test Commands

```bash
curl https://emailapi-o5je.onrender.com/health
curl -X POST https://emailapi-o5je.onrender.com/subscribe-email \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com"}'
```

## Deployment (Render)

Build Command: `npm install`

Start Command: `npm start`

Add the environment variables in the Render dashboard. The platform injects `PORT` automatically.

## Production Notes

- Filesystem is ephemeral; attachments are removed after send.
- Consider memory storage or external object storage for larger files.
- Add rate limiting & validation for public exposure.
- Use proper logging (e.g., morgan) if scaling.

## License

MIT (attribution retained where applicable).

---
Maintained by Kevin-kemboi.
