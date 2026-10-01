<div align="center">

# Vapi Voice Calling Prototype

**A small Flask experiment for initiating Vapi outbound calls from customer records.**

</div>

> **Status: prototype / not production ready.** The checked-in application contains a developer-specific Excel path, enables Flask debug mode, and should be reviewed before any deployment. Do not expose it to the public internet as-is.

## What it does

The single-file Flask app loads customer records from an Excel workbook, accepts a customer ID and E.164 phone number at `POST /initiate-call`, then sends an outbound call request to Vapi using configured assistant and phone-number IDs.

The app currently reads its workbook path from a hard-coded Windows path in `app.py`. Configure it for your environment before running; see the setup notes below.

## Repository contents

- `app.py` — Flask route, Excel lookup, and Vapi call request.

## Requirements

The repository currently has no dependency manifest. Install the imports used by `app.py`:

- Python 3.10+
- Flask
- pandas
- openpyxl
- requests
- python-dotenv

## Configuration

Create a local `.env` file (never commit real credentials):

```dotenv
VAPI_API_KEY=your-vapi-api-key
VAPI_ASSISTANT_ID=your-assistant-id
VAPI_PHONE_NUMBER_ID=your-phone-number-id
```

Edit `EXCEL_FILE_PATH` in `app.py` to point to a workbook containing the columns listed in `EXCEL_COLUMN_MAPPING`. The workbook may contain personal and financial data; keep it out of source control and restrict access.

## Run locally

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows PowerShell
# .venv\Scripts\Activate.ps1

pip install Flask pandas openpyxl requests python-dotenv
python app.py
```

The development server listens on port `5001`.

## API

`POST /initiate-call`

```json
{
  "customer_id": "1234",
  "mobile_number": "+61412345678"
}
```

The phone number must use E.164 format. The app returns the Vapi response and status code.

## Known limitations

- No automated tests, dependency lockfile, or production server configuration is present.
- The Excel file path is hard-coded and must be made configurable.
- Flask debug mode is enabled in the local entry point; disable it outside local development.
- The app logs customer details and request payloads to stdout; remove or redact personal data before any use with real customers.
- The current call request uses a minimal payload and does not pass the loaded customer variables to Vapi.
- Add authentication, request validation, rate limiting, consent controls, and safe error handling before exposing an API.

## Banner

This README uses a typographic project header until a purpose-made visual asset is added. No unrelated image has been borrowed as a banner.
