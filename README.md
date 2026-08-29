# DRipACS Teleradiology

Secure DICOM study sharing portal with a browser-based DICOM viewer, study/series APIs, password-protected patient links, and report entry.

## Features

- Flask web application
- DICOM receiving through pynetdicom
- SQLite study/link/report storage
- Expiring secure study links
- Password-protected patient access
- Browser DICOM viewer using Cornerstone and DICOM Parser
- Study, series, image metadata and report APIs
- Windows startup script
- Environment-based configuration

## Project layout

```text
.
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
├── start_server.bat
├── index.html
└── templates/
    └── viewer.html
```

## Local setup

1. Install Python 3.11+.
2. Copy `.env.example` to `.env` and set your secrets/configuration.
3. Create a virtual environment and install dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

4. Start the server:

```powershell
python app.py
```

Or on Windows, run `start_server.bat`.

The Flask web server defaults to port `5000`; the DICOM listener defaults to `11112`.

## Environment variables

| Variable | Purpose | Default |
|---|---|---|
| `PUBLIC_URL` | Public base URL used in secure links | `https://dripacs.is-a.dev` |
| `FLASK_SECRET_KEY` | Flask session signing secret | Random per process |
| `LINK_PASSWORD` | Password for generated patient links | `siva` |
| `ADMIN_PASSWORD` | Admin page password | `change-me` |
| `DICOM_AE_TITLE` | DICOM Application Entity title | `SECURELINK` |
| `DICOM_PORT` | DICOM listener port | `11112` |
| `PORT` | Flask HTTP port | `5000` |
| `DRIPACS_DB` | SQLite database path | `secure_links.db` |

## Security

Do not commit `.env`, `secure_links.db`, or patient DICOM files. The repository `.gitignore` excludes these by default. Use a strong `FLASK_SECRET_KEY` and `ADMIN_PASSWORD` outside local testing.

This project is intended to be deployed behind HTTPS and appropriate network access controls. Review authentication, authorization, audit logging, encryption, retention, and regulatory requirements before clinical production use.

## DICOM workflow

A DICOM sender can send studies to AE Title `SECURELINK` on the configured DICOM port. Received studies are indexed into SQLite and can be exposed through a secure token link. The browser viewer loads the study through the protected API endpoints.

## License

See `LICENSE` for project licensing terms.
