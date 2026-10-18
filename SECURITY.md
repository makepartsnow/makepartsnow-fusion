# Security

Please report a vulnerability in the add-in, or in how it talks to makepartsnow.com, by email to
nathan@makepartsnow.com with "security" in the subject. Don't open a public issue. We answer within 3 business days.

What the add-in does to stay small and safe:

- It talks only to makepartsnow.com over HTTPS with certificate checks (plain HTTP only to localhost, for development).
- It opens in the browser only https links on makepartsnow.com (or the configured development server), and uploads
  design files only to makepartsnow.com.
- The palette escapes all server and design text before showing it, and checks every message from the page before
  acting on it.
- It has no dependencies beyond Fusion and Python's standard library, does nothing on the network or in a thread
  when Fusion starts, and never runs downloaded code.
- It never sees payment details (checkout is on makepartsnow.com through Stripe) and never stores the files it
  uploads. Delete tokens for your uploads stay in memory only.

Supported versions: the latest release.
