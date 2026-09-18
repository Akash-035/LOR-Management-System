# LOR Management System

The LOR Management System gives students a structured way to request letters of recommendation, helps faculty review and approve those requests, and gives administrators controls for managing users and requests. Approved letters can be generated as PDFs, delivered by email, and verified publicly through a QR-linked page.

## Features

- Separate student, faculty, and administrator authentication flows.
- Role-protected dashboards for submitting, reviewing, editing, approving, rejecting, assigning, and managing LOR requests.
- Configurable request purpose and program options.
- PDF generation with digital signatures and QR verification links.
- Public verification route for approved letters.
- Email notifications for LOR status changes and faculty workflows.
- Password reset support, request validation, rate limiting, security headers, compression, and structured request logging.

## Architecture

The repository contains two independently runnable applications:

- `client/` is a React 18 single-page application built with Vite, React Router, Axios, Zod, and Motion.
- `server/` is an Express API backed by MongoDB through Mongoose. It provides JWT authentication, role-based access control, validation, PDF generation, QR codes, and email services.

The client calls the server through `VITE_API_BASE`. The server exposes its versioned routes below `/api/v1` and provides a `/health` endpoint.

## Repository structure

```text
client/   React/Vite frontend
server/   Express/Mongoose backend
```

The project history is preserved under these two directories. The root `.gitignore` prevents new environment files, dependencies, build output, logs, tunnel files, and key material from being added.

## Configuration

The root `.env.example` is a safe overview. The authoritative application examples are:

- `client/.env.example` for `VITE_API_BASE`.
- `server/.env.example` for the API, MongoDB, JWT, administrator, SMTP, and Resend settings.

Create local `.env` files in each application directory and fill in deployment-specific values. Never commit real credentials. `JWT_SECRET`, administrator credentials, SMTP credentials, Resend keys, and database connection details must remain local or be supplied through the deployment environment.

## Local development

Install and run the client:

```bash
cd client
npm install
npm run dev
```

Install and run the server in another terminal:

```bash
cd server
npm install
npm run dev
```

The default development ports are `5173` for Vite and `4000` for the API. A MongoDB instance must be available at the configured `MONGO_URI`. The server can also be started with `npm start`; `npm run dev:watch` enables Node's watch mode.

## Development notes

The public verification flow is available at the client route `/verify/:token`. Authentication and dashboards are routed through `/auth`, `/student`, `/faculty`, and `/admin`. Production deployments should provide HTTPS, a production client URL, a managed MongoDB connection, and real email provider configuration through environment variables.

Some historical client commits contain generated dependency and build artifacts. They remain part of the preserved history; the root ignore rules prevent those artifacts from being added to future commits.

## Future improvements

- Add automated tests for authentication, request transitions, PDF generation, and public verification.
- Add a documented deployment pipeline and environment validation for production releases.
