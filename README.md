# EauSure Authentication API

Authentication backend for the EauSure platform.

This service provides a centralized identity management system, supporting local credential-based accounts alongside OAuth integrations (Google and GitHub). It interfaces with a MongoDB database for persistence and utilizes standard token-based authentication to securely manage client-server communications.

## Scope

- Local account registration and authentication.
- Google OAuth sign-in and registration flows.
- GitHub OAuth sign-in and registration flows.
- Secure current-user profile lookup for authenticated clients.

## Route Overview

| Method | Path | Purpose |
|------|------|------|
| `GET` | `/` | Basic service response |
| `POST` | `/api/auth/register` | Create a local account |
| `POST` | `/api/auth/login` | Sign in with email and password |
| `GET` | `/api/auth/google` | Start Google login |
| `GET` | `/api/auth/google/register` | Start Google registration |
| `GET` | `/api/auth/google/callback` | Finish Google login |
| `GET` | `/api/auth/google/register/callback` | Finish Google registration |
| `GET` | `/api/auth/github` | Start GitHub login |
| `GET` | `/api/auth/github/register` | Start GitHub registration |
| `GET` | `/api/auth/github/callback` | Finish GitHub login or registration |
| `GET` | `/api/auth/me` | Return the authenticated user profile |

## Authentication Architecture

### Protected Routes
Endpoints handling sensitive user data, such as `/api/auth/me`, enforce strict access controls. The client must attach a valid, unexpired authorization token—issued exclusively during a successful login phase—to interact with these protected resources.

### Identity Providers & OAuth Flows
The system delegates third-party authentication to external providers via standardized OAuth callback endpoints. These redirect URIs securely process the approval or rejection states from the provider before routing the user back to the frontend client with either an active token or an error code.

> **Note on Registration:** Local account creation via `/api/auth/register` provisions the user profile in the database but intentionally requires a subsequent, discrete login request to establish an active session.

## Environment Configuration

The application relies on environmental variables to dictate routing parameters, database targets, and provider integrations without hardcoding sensitive configurations into the source.

**Core Infrastructure:**
- `MONGO_URI`: Database connection target.
- `API_URL`: The public-facing domain of this backend.
- `FRONTEND_URL`: Target URL for authentication state redirects.
- `PORT`: Server listening port.

**Authentication Secrets:**
- `JWT_SECRET`: Cryptographic key utilized for signature validation.
- `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET`: Application credentials for Google integration.
- `GITHUB_CLIENT_ID` / `GITHUB_CLIENT_SECRET`: Application credentials for GitHub integration.