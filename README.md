````markdown
# 📞 Phonebook Application (Python)

A full-stack Phonebook Application built using **Vue.js, FastAPI, PostgreSQL, SQLAlchemy, Docker, and Nginx**.

## Architecture

```text
Browser
   ↓
Nginx (Vue.js Frontend)
   ↓  /api
FastAPI (Python)
   ↓
SQLAlchemy
   ↓
PostgreSQL
````

## Features

* User registration and login
* PostgreSQL-based authentication
* Protected API endpoints
* Add, view, update, and delete contacts
* User-specific contact ownership
* Search contacts
* Google-style numbered pagination
* Import contacts using CSV
* CSV column mapping and validation
* Export contacts as CSV
* PostgreSQL database storage
* Up to 1000 contacts for testing

## Run Locally

### Requirements

* Docker Desktop
* Git

Clone the repository:

```bash
git clone https://github.com/Harsh280705/phonebook_python.git
cd phonebook_python
```

Build and start the application:

```bash
docker compose up --build
```

Open the application:

```text
http://localhost
```

## Stop the Application

```bash
docker compose down
```

To rebuild after backend or frontend changes:

```bash
docker compose up --build
```

## Populate Fake Contacts

With the stack running:

```bash
docker compose exec backend python -m app.scripts.populate_contacts
```

This adds contacts until the database has approximately 1000 contacts. Existing contacts are preserved.

## Testing

### Backend API Tests

Run the backend API tests:

```bash
docker compose exec backend pytest
```

## Playwright End-to-End Tests

From the `frontend` directory:

```bash
cd frontend
```

Run all Playwright tests:

```bash
npm run test:e2e
```

Run tests with the browser visible:

```bash
npm run test:e2e:headed
```

Run a specific test file:

```bash
npx playwright test tests/auth.spec.js
```

View the HTML test report:

```bash
npm run test:e2e:report
```

The Playwright tests cover authentication, contacts, import/export, search, and pagination.

## API Tests

### 1. `GET /`

* `test_root_endpoint` — Verifies that the API root endpoint is running and returns the expected response.

## Authentication APIs

### 2. `POST /api/auth/register`

* `test_register_valid_user` — Tests successful user registration.
* `test_register_duplicate_username_or_email` — Tests duplicate username/email rejection.
* `test_register_invalid_data` — Tests invalid registration data.

### 3. `POST /api/auth/login`

* `test_login_by_username_and_email` — Tests login using both username and email.
* `test_invalid_login` — Tests rejection of invalid credentials.

### 4. `GET /api/auth/me`

* `test_current_user_requires_authentication` — Tests authenticated and unauthenticated access.

### 5. `POST /api/auth/logout`

* `test_logout_invalidates_session` — Tests that logout invalidates the user's session.

## Contact APIs

### 6. `GET /api/contacts/`

* `test_protected_contact_endpoints_require_authentication` — Tests that unauthenticated users cannot access contacts.
* `test_create_get_single_and_list_contacts` — Tests listing contacts.
* `test_search_and_pagination` — Tests search results and pagination.
* `test_users_cannot_access_each_others_contacts` — Tests user-specific contact access.

### 7. `POST /api/contacts/`

* `test_protected_contact_endpoints_require_authentication` — Tests authentication protection.
* `test_create_get_single_and_list_contacts` — Tests successful contact creation.
* `test_create_invalid_contact_data` — Tests invalid contact data.
* `test_duplicate_phone_and_email_are_rejected` — Tests duplicate phone/email rejection.

### 8. `GET /api/contacts/{contact_id}`

* `test_create_get_single_and_list_contacts` — Tests retrieving a contact.
* `test_delete_contact_and_missing_contact` — Tests non-existent contact handling.
* `test_users_cannot_access_each_others_contacts` — Tests that another user's contact cannot be accessed.
* `test_protected_contact_endpoints_require_authentication` — Tests authentication protection.

### 9. `PUT /api/contacts/{contact_id}`

* `test_update_contact` — Tests successful contact updates.
* `test_protected_contact_endpoints_require_authentication` — Tests authentication protection.
* `test_users_cannot_access_each_others_contacts` — Tests that another user's contact cannot be updated.

### 10. `DELETE /api/contacts/{contact_id}`

* `test_delete_contact_and_missing_contact` — Tests contact deletion and non-existent contact handling.
* `test_protected_contact_endpoints_require_authentication` — Tests authentication protection.
* `test_users_cannot_access_each_others_contacts` — Tests that another user's contact cannot be deleted.

## Search and Pagination

### 11. `GET /api/contacts/?search=...`

* `test_search_and_pagination` — Tests contact search and correct filtered results.

### 12. `GET /api/contacts/?page=2&limit=10`

* `test_search_and_pagination` — Tests page number, page size, total count, and final-page results.

## CSV Import API

### 13. `POST /api/contacts/import`

* `test_import_requires_authentication` — Tests authentication protection.
* `test_import_valid_csv_and_persists_contacts` — Tests valid CSV import and database persistence.
* `test_import_external_columns_and_excel_phone_text` — Tests external column mapping and Excel-style phone values.
* `test_import_invalid_rows_and_scientific_notation` — Tests invalid phone, email, name, and scientific notation handling.
* `test_import_duplicates_are_skipped` — Tests duplicate contact handling.
* `test_import_rejects_missing_headers` — Tests missing CSV headers.
* `test_import_rejects_non_csv_and_empty_files` — Tests invalid and empty files.

## CSV Export API

### 14. `GET /api/contacts/export`

* `test_export_requires_authentication` — Tests authentication protection.
* `test_export_returns_only_authenticated_users_contacts` — Tests successful CSV export, CSV content type, correct headers and contact data, and ensures only the authenticated user's contacts are exported.

## Summary

**Total backend API tests: 24**

**All implemented backend API endpoints are covered by automated tests.**

