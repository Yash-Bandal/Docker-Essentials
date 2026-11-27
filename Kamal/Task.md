Goal: Run the app in Docker inside WSL2 on our own Windows machines and deploy it with Kamal, with WSL acting as the server. IIS on Windows maps a local domain (quicklinks.test) and reverse-proxies to kamal-proxy in WSL.

Flow: Browser/Postman (Windows) → https://quicklinks.test → IIS (Windows, :443) → localhost:8080 → kamal-proxy (WSL2) → app container (:3000)
Tasks
Dockerise the app
Multi-stage Dockerfile, non-root user, app on port 3000
/up health endpoint that returns 200
.dockerignore; all config comes from env vars
Build and run it locally in WSL to check
Set up WSL2 as the server
Install WSL2 Ubuntu with systemd enabled (/etc/wsl.conf)
Install Docker Engine and add your user to the docker group
Run openssh-server on port 2222, key-based login only (Kamal deploys over SSH, even to the same machine)
Generate an SSH key in WSL and add it to ~/.ssh/authorized_keys; check that ssh -p 2222 <user>@127.0.0.1 works
Set networkingMode=mirrored in %UserProfile%\.wslconfig so Windows/IIS can reach WSL ports on localhost
Configure Kamal
kamal init, then fill in config/deploy.yml: servers: [127.0.0.1], ssh.port: 2222, registry (Docker Hub/GHCR free account), proxy.host: quicklinks.test, app_port: 3000, ssl: false, proxy.run.http_port: 8080, builder.arch: amd64
Put secrets in .kamal/secrets (git-ignored)
Configure IIS and the local domain (Windows)
Add 127.0.0.1 quicklinks.test to C:\Windows\System32\drivers\etc\hosts (edit as admin)
Install URL Rewrite and ARR, then enable the proxy
Create a site bound to quicklinks.test on 443 with a self-signed cert (IIS Manager → Server Certificates → Create Self-Signed Certificate, or use mkcert), and optionally on port 80
Add a rewrite rule to http://localhost:8080/{R:1} that sets X-Forwarded-Proto: https
Preserve the Host header (preserveHostHeader=true), because kamal-proxy routes by quicklinks.test
Add an HTTP→HTTPS redirect; enable WebSockets if needed
Deploy and test
First deploy with kamal setup, then kamal deploy for each release
Check each hop:
In WSL: curl -H "Host: quicklinks.test" localhost:8080/up
In Windows: https://quicklinks.test/up in a browser and in Postman (turn off SSL certificate verification in Postman settings if you use a self-signed cert)
Change some code, run kamal deploy, and check that the new version is live with no downtime
Acceptance criteria
https://quicklinks.test opens the app from Windows (browser and Postman)
kamal deploy ships a change with zero downtime.

App: "QuickLinks", a tiny URL shortener (ASP.NET Core Minimal API, .NET 10)
Endpoint
What it does
Redis feature it exercises
POST /links { url }
Creates a short code
SET with TTL
GET /{code}
Redirects to the URL and counts the visit
GET + INCR
GET /links/{code}/stats
Shows the visit count
GET
GET /top
Top 10 most-visited links
Sorted set (ZINCRBY / ZREVRANGE)
Any endpoint
Rate limit of 20 requests/min per IP
INCR + EXPIRE
GET /up
Health check for kamal-proxy
PING

It does four jobs:
Stores the links. Each short code maps to its URL (link:abc123 → https://...), optionally with an expiry (TTL) so old links delete themselves. Every redirect is a lookup, and Redis answers in well under a millisecond.
Counts visits. Each redirect runs INCR visits:abc123. The increment is atomic, so the count stays correct even when many people hit the link at the same moment.
Ranks the top links. A sorted set (ZINCRBY top-links 1 abc123) keeps links ordered by visits automatically, so /top is one command (ZREVRANGE) rather than a sort query.
Rate-limits requests. A key per IP (rate:1.2.3.4) is incremented on each request and expires after 60 seconds. Past 20, the API returns 429. This is a standard Redis pattern because the counter cleans itself up.
