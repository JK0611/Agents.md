Modular agent skills (Scraper, Analytics, and Security) actively running the [EPFTracker.xyz](https://epftracker.xyz/) production pipeline
| Agent | Core Responsibilities | Key Capabilities |
| --- | --- | --- |
| `scraper-agent` | Data Ingestion & Pipeline ETL | • Incremental crawling of Bursa disclosures (122k+ rows)<br>• Anti-throttling & stealth puppeteer automation<br>• Self-healing parser recovery & automated Git sync |
| `analytics-agent` | PostHog Telemetry & UX Insights | • Direct HogQL API queries for visitor drop-offs<br>• Device & hardware profiling (iOS/Android/resolutions)<br>• Privacy-first: enforces client-side PII masking |
| `cybersecurity-guard` | OWASP Defense & Edge Hardening | • Universal dynamic DOM sanitization (`escapeHTML`)<br>• Vercel Edge CSP whitelist & security headers<br>• Static scanning for zero secret/credential leaks |
