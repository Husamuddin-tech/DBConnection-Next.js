
# DBConnection‑Next.js

A minimal boilerplate / utility for initializing a MongoDB connection in a Next.js / Node.js project using TypeScript.  

---

## 📂 Contents

| File | Purpose |
|------|---------|
| `lib/db.ts` | Contains connection logic, caching, and reconnect handling |
| `types.d.ts` | Shared TypeScript types (if any) |
| `.env.example` | Example environment variables needed to run the project |

---

## 🚀 Getting Started

### Prerequisites

- Node.js (v14+ recommended)  
- MongoDB (Atlas or local)  

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Husamuddin-tech/DBConnection-Next.js.git
   cd DBConnection-Next.js
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Create environment file**

   Copy `.env.example` to `.env` and fill in your MongoDB URI, e.g.:

   ```text
   MONGODB_URI=Your mongoDB URI
   ```

4. **Run the project**

   For development:
   ```bash
   npm run dev
   # or
   yarn dev
   ```

---

## 🧩 Usage

Here’s how the DB connection logic generally works:

- You import `connectDB` or your DB helper in your API routes or server-side code.
- The first call will attempt to connect using the URI from the environment.
- The connection is cached, so subsequent calls reuse the same connection (avoiding repeated connection overhead).
- Errors during connect are caught; the promise is reset so retries can occur.

### Example (pseudo-code in Next.js API route)

```ts
import { NextApiRequest, NextApiResponse } from "next";
import connectDB from "../lib/db";

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  try {
    const db = await connectDB();
    // do stuff with db / models
    res.status(200).json({ message: "Connected to DB!" });
  } catch (err) {
    console.error("API route DB error:", err);
    res.status(500).json({ error: "Database connection failed" });
  }
}
```

---

## 🎯 Features & Design Decisions

- **Connection caching** — doesn’t open a new connection every time.
- **Error handling** — catch failures, reset state, and propagate meaningful error messages.
- **TypeScript-friendly** — types in `types.d.ts` for strong typing.
- **Minimal dependency footprint** — only core dependencies needed for MongoDB + Next.js setup.

---

## ⚠️ Notes & Best Practices

- Do **not** commit your real `.env` file to version control.
- In production, ensure your `MONGODB_URI` is secure (use environment secrets or vaults).
- Monitor connection pool usage (e.g. via `maxPoolSize`) to avoid overwhelming MongoDB with too many simultaneous connections.
- Consider adding retry logic (with delay / backoff) or fallback in case of transient network issues.

---

## 🛠 Enhancements (To Do)

- Add automatic retry with exponential backoff  
- Support for multiple database connections (for microservices)  
- Health-check endpoint to verify DB connectivity  
- Logging via a structured logger (e.g. Pino, Winston) instead of `console`  

---

## 🧾 License

This project is open-source and free to use. _(Feel free to apply your preferred license, e.g. MIT)_

---

## ✅ Summary

This repo provides a simple, clean, reusable setup for **connecting MongoDB in a Next.js / TypeScript** environment with caching, error handling, and best practices in mind.  
