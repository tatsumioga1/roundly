# Radius Website

Static marketing site for `radius.northwindlab.website`.

## Local Preview

The site is static. You can preview it from the repository root with:

```powershell
python -m http.server 8080 -d website
```

Then open `http://localhost:8080`.

## Cloudflare Tunnel Deployment

Recommended shape on the Oracle instance:

1. Copy the `website` folder to the server, for example `/var/www/radius`.
2. Serve it with nginx or Caddy on localhost.
3. Point `cloudflared` at that local server.

Example `cloudflared` ingress:

```yaml
tunnel: <your-tunnel-id>
credentials-file: /etc/cloudflared/<your-tunnel-id>.json

ingress:
  - hostname: radius.northwindlab.website
    service: http://localhost:8080
  - service: http_status:404
```

Example static server for a quick smoke test:

```bash
cd /var/www/radius
python3 -m http.server 8080
```

For production, prefer nginx or Caddy as a service.

## Suggested Security Headers

Radius does not need any third-party scripts or remote assets, so the site can run with tight headers.

Example nginx headers:

```nginx
add_header Content-Security-Policy "default-src 'self'; img-src 'self'; style-src 'self'; base-uri 'self'; frame-ancestors 'none'; form-action 'none'" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=(), payment=()" always;
```
