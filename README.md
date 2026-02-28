// file: package.json
{
  "name": "digimark-dynasty-minimal",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "14.1.0",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  }
}

// file: next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true
};

module.exports = nextConfig;

// file: pages/index.js
import { useEffect, useState } from "react";

export default function Home() {
  const [message, setMessage] = useState("Loading...");
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(true);
    fetch("/api/hello")
      .then((res) => res.json())
      .then((data) => {
        setMessage(data.message);
      })
      .catch(() => {
        setMessage("Error talking to backend.");
      })
      .finally(() => setLoading(false));
  }, []);

  return (
    <main
      style={{
        minHeight: "100vh",
        display: "flex",
        flexDirection: "column",
        alignItems: "center",
        justifyContent: "center",
        background: "#050816",
        color: "#f9fafb",
        fontFamily: "system-ui, -apple-system, BlinkMacSystemFont, sans-serif",
        padding: "2rem",
        textAlign: "center",
      }}
    >
      <h1 style={{ fontSize: "2.5rem", marginBottom: "1rem" }}>
        DigiMark Dynasty · Ava Skye
      </h1>
      <p style={{ maxWidth: "600px", marginBottom: "2rem", opacity: 0.9 }}>
        This is a minimal full‑stack setup on Vercel. The frontend calls a backend API
        route and shows the response. From here, you can plug in your real flows.
      </p>

      <div
        style={{
          padding: "1.5rem 2rem",
          borderRadius: "0.75rem",
          background:
            "linear-gradient(135deg, rgba(59,130,246,0.15), rgba(236,72,153,0.15))",
          border: "1px solid rgba(148,163,184,0.4)",
          marginBottom: "1.5rem",
        }}
      >
        <h2 style={{ fontSize: "1.25rem", marginBottom: "0.5rem" }}>
          Backend says:
        </h2>
        <p style={{ fontSize: "1.1rem" }}>
          {loading ? "Talking to Ava’s backend..." : message}
        </p>
      </div>

      <button
        onClick={() => {
          setLoading(true);
          fetch("/api/hello", { method: "POST" })
            .then((res) => res.json())
            .then((data) => setMessage(data.message))
            .catch(() => setMessage("Error on POST request."))
            .finally(() => setLoading(false));
        }}
        style={{
          padding: "0.75rem 1.5rem",
          borderRadius: "999px",
          border: "none",
          cursor: "pointer",
          background:
            "linear-gradient(135deg, #3b82f6, #ec4899)",
          color: "#f9fafb",
          fontWeight: 600,
          fontSize: "0.95rem",
          boxShadow: "0 10px 25px rgba(15,23,42,0.7)",
        }}
      >
        Ping Ava’s API (POST)
      </button>

      <p style={{ marginTop: "1.5rem", fontSize: "0.85rem", opacity: 0.7 }}>
        Deployed on Vercel · Frontend + Backend in one Next.js project.
      </p>
    </main>
  );
}

// file: pages/api/hello.js
export default function handler(req, res) {
  if (req.method === "POST") {
    return res.status(200).json({
      message:
        "POST received. Ava Skye backend is live and ready to automate your Dynasty.",
    });
  }

  return res.status(200).json({
    message:
      "Hello from your DigiMark Dynasty backend. This API route is running on Vercel.",
  });
}
