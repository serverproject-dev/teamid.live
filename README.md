# teamid.live
Community-Solid-Server of [meisdata.io](https://meisdata.io)
- - - -
sample config as of 2024-10-13
- - - -
`/data/css/Recipes/mashlib/node_modules/@solid/community-server/config$ less config-mashlib.json`
```
{
  "@context": "https://linkedsoftwaredependencies.org/bundles/npm/@solid/community-server/^7.0.0/components/context.jsonld",
  "import": [
    "css:config/app/init/default.json",
    "css:config/app/main/default.json",
    "css:config/app/variables/default.json",
    "css:config/http/handler/default.json",
    "css:config/http/middleware/default.json",
    "css:config/http/notifications/all.json",
    "css:config/http/server-factory/http.json",
    "css:config/http/static/default.json",
    "css:config/identity/access/public.json",
    "css:config/identity/email/default.json",
    "css:config/identity/handler/default.json",
    "css:config/identity/oidc/default.json",
    "css:config/identity/ownership/token.json",
    "css:config/identity/pod/static.json",
    "css:config/ldp/authentication/dpop-bearer.json",
    "css:config/ldp/authorization/webacl.json",
    "css:config/ldp/handler/default.json",
    "css:config/ldp/metadata-parser/default.json",
    "css:config/ldp/metadata-writer/default.json",
    "css:config/ldp/modes/default.json",
    "css:config/storage/backend/file.json",
    "css:config/storage/key-value/resource-store.json",
    "css:config/storage/location/pod.json",
    "css:config/storage/middleware/default.json",
    "css:config/util/auxiliary/acl.json",
    "css:config/util/identifiers/suffix.json",
    "css:config/util/logging/winston.json",
    "css:config/util/representation-conversion/default.json",
    "css:config/util/resource-locker/file.json",
    "css:config/util/variables/default.json"
  ],
  "@graph": [
    {
      "comment": [
        "A filesystem-based server with Databrowser as UI.",
        "Derived from config/file-no-setup.json"
      ]
    },
    {
      "comment": "Serve Databrowser as default representation",
      "@id": "urn:solid-server:default:DefaultUiConverter",
      "@type": "ConstantConverter",
      "contentType": "text/html",
      "filePath": "./mashlib/dist/databrowser.html",
      "options_container": true,
      "options_document": true,
      "options_minQuality": 1,
      "options_disabledMediaRanges": [
        "image/*",
        "application/pdf"
      ]
    },
    {
      "comment": "Serve Mashlib static files.",
      "@id": "urn:solid-server:default:StaticAssetHandler",
      "@type": "StaticAssetHandler",
      "assets": [
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mash.css",
          "filePath": "./mashlib/dist/mash.css"
        },
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mashlib.min.js",
          "filePath": "./mashlib/dist/mashlib.min.js"
        },
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mashlib.min.js.map",
          "filePath": "./mashlib/dist/mashlib.min.js.map"
        }
      ]
    }
  ]
}
```
`/data/css/Recipes/mashlib/node_modules/@solid/community-server/config/identity/email$ less default.json`
```
{
  "@context": "https://linkedsoftwaredependencies.org/bundles/npm/@solid/community-server/^7.0.0/components/context.jsonld",
  "@graph": [
    {
      "comment": "This is an example of what an actual email sender configuration would look like.",
      "@id": "urn:solid-server:default:EmailSender",
      "@type": "BaseEmailSender",
      "args_senderName": "me@evering.eu",
      "args_emailConfig_host": "smtp.sendgrid.net",
      "args_emailConfig_port": 465,
      "args_emailConfig_auth_user": "apikey",
      "args_emailConfig_auth_pass": "black"
    }
  ]
}
```
`/etc/nginx/sites-available$ less default`
```
# The local Solid server instance
upstream solid-community-server {
  server 127.0.0.1:3000;
}

server {
        listen 80;
        listen [::]:80;
        server_name teamid.live;
        server_tokens off; ## Don't show the nginx version number, a security best practice
        return 301 https://$http_host$request_uri;

}

# Proxy traffic for https://solid.example/ to http://localhost:3000/
server {
  server_name teamid.live;
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  ssl_certificate         /etc/letsencrypt/archive/teamid.live/fullchain8.pem;
  ssl_certificate_key     /etc/letsencrypt/archive/teamid.live/privkey8.pem;
  ssl_trusted_certificate /etc/letsencrypt/live/teamid.live/lets-encrypt-x3-cross-signed.pem;

#rewrite ^/$  somewhereelse/index.html  permanent;

access_log /var/log/nginx/css_ssl_access.log;
error_log /var/log/nginx/css_ssl_error.log;

  # Include this for certificate renewal if you are using Let's Encrypt
  include snippets/https.conf;
    location ^~ /.well-known/acme-challenge/ {
    root /var/www/teamid.live; # or a folder of your choice
  
  }

  # Proxy all other trafic to the Solid server
  location / {
    # Delegate to the Solid server, passing the original host and protocol
    proxy_pass http://solid-community-server$request_uri;
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;

    # Pass these headers from the Solid server back to the client
    proxy_pass_header Server;
    proxy_pass_header Set-Cookie;

    # Enable Websocket support
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
  }
}
```
`/etc/nginx/snippets$ less https.conf`
```
# Generated by https://ssl-config.mozilla.org/

ssl_session_timeout 1d;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets on;

# Diffie-Hellman parameter for DHE ciphersuites, recommended 2048 bits
ssl_dhparam /etc/letsencrypt/live/teamid.live/dh2048.pem;

# intermediate configuration
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-S>
ssl_prefer_server_ciphers off;

# HSTS (ngx_http_headers_module is required) (63072000 seconds)
add_header Strict-Transport-Security "max-age=63072000" always;

# OCSP stapling
ssl_stapling on;
ssl_stapling_verify on;
```
`/data/css/Recipes/mashlib/node_modules$ less pm3.sh`
```
npx @solid/community-server -c @css:config/config-mashlib.json -f /var/www/teamid.live -b https://teamid.live/
```
