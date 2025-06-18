# 🧙‍♀️ Morgana’s Trial – Round Table 2.0

This solution was developed for **Morgana’s Trial**, a .NET challenge focused on integrating **Umbraco CMS 15** with a secure, scalable **.NET Web API**. The goal was to expose selected Umbraco Management API features to external clients via a custom API layer.

---

## 🧱 Solution Structure

### 1. `UmbracoCMS` (Umbraco 15 CMS)
- **Framework:** .NET 9
- **CMS:** Umbraco 15
- **Database:** SQLite (included in the solution)
- **Purpose:** Hosts the CMS and Management API endpoints.

### 2. `UmbracoBridge` (.NET Web API)
- **Framework:** .NET 9
- **Purpose:** Exposes endpoints to interact with Umbraco’s Management API securely via client credentials.
- **Features:**
  - Validated endpoints.
  - Token-based authentication with caching.
  - OpenAPI / Swagger documentation.
  - Clean architecture: services, commands, queries, validators, contracts.
  - Unit testing using `xUnit` and `Moq`.

---

## 🌐 Exposed Endpoints

| Method | Route                               | Description                                      |
|--------|--------------------------------------|--------------------------------------------------|
| GET    | `/api/management/healthcheck`       | Gets Umbraco health status                      |
| POST   | `/api/management/documenttype`      | Creates a new Document Type in Umbraco         |
| DELETE | `/api/management/delete/{id}`       | Deletes a Document Type by ID                  |

### ✅ Validation Rules for POST `/documenttype`:
- `alias`, `name`, and `description` must **not be empty**.
- `icon` must **start with `icon-`**.

---

## 🧪 Unit Tests

Unit tests using `xUnit` and `Moq` cover:
- `ManagementController`: success and failure scenarios.
- `TokenService`: token acquisition and error handling.
- `HealthCheckService`: Umbraco connectivity errors.
- `DocumentTypeValidator`: for all rule cases (valid, missing fields, wrong icon format, etc.)

Run tests with:

```bash
dotnet test
⚙️ Setup Instructions
Prerequisites:
.NET 9 SDK

Git

Visual Studio 2022+ or Rider

🔧 Local Setup
Clone the repo:

bash
Copy
Edit
git clone https://github.com/your-username/morgana-trial.git
cd morgana-trial
Restore & build:

bash
Copy
Edit
dotnet restore
dotnet build
Run both apps:

bash
Copy
Edit
dotnet run --project UmbracoCMS
dotnet run --project UmbracoBridge
Access Swagger docs:

Umbraco: https://localhost:{PORT}/umbraco/swagger/index.html

Bridge API: https://localhost:{PORT}/swagger/index.html

📬 Example Requests
✅ POST /api/management/documenttype
json
Copy
Edit
{
  "alias": "customAlias",
  "name": "My Document",
  "description": "Document for testing",
  "icon": "icon-notepad",
  "allowedAsRoot": true,
  "variesByCulture": false,
  "variesBySegment": false,
  "collection": null,
  "isElement": true
}
Response:

json
Copy
Edit
{
  "id": "6bfba926-609f-44b6-89ea-97fe4f89baf9"
}
❌ Invalid POST Example
json
Copy
Edit
{
  "alias": "",
  "name": "",
  "description": "",
  "icon": "notepad"
}
Response:

json
Copy
Edit
{
  "errors": {
    "alias": "Alias must not be empty.",
    "name": "Name must not be empty.",
    "description": "Description must not be empty.",
    "icon": "Icon must start with 'icon-'."
  }
}
✅ Verification Checklist
 UmbracoCMS runs with SQLite and exposes /umbraco/swagger.

 UmbracoBridge runs and consumes Umbraco's management API.

 Token is requested only if not cached or expired.

 Validators in place for POST payloads.

 Swagger is enabled and works for both APIs.

📂 Notes on .gitignore
Make sure the following are included (not ignored):

umbraco.db (SQLite)

appsettings.json (with placeholders if needed)

Any config files that affect bootstrapping (like Startup.cs, Program.cs, etc.)
