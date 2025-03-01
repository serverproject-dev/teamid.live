# teamid.live
Pivot-Solid-Server of [meisdata.io](https://meisdata.io)
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
ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
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
or
```
/root/.nvm/versions/node/v18.16.0/bin/node /data/css/Recipes/mashlib/node_modules/@solid/community-server/bin/server.js -c @css:config/config-mashlib.json -f /var/www/teamid.live -b https://teamid.live/
```
appendix march 2025

`/data/pivot0/pivot`
```
total 656
drwxr-xr-x  11 root root   4096 Mar  1 04:51 .
drwxr-xr-x   3 root root   4096 Feb 24 15:20 ..
-rw-r--r--   1 root root     74 Feb 24 15:20 before.ttl
-rw-r--r--   1 root root      4 Feb 24 15:20 .componentsignore
drwxr-xr-x   5 root root   4096 Feb 25 07:43 config
-rw-r--r--   1 root root   3517 Feb 24 15:30 custom-config.json
drwxr-xr-x   4 root root   4096 Feb 24 19:11 data
-rw-r--r--   1 root root      0 Mar  1 04:51 dir_inst
drwxr-xr-x   7 root root   4096 Feb 24 15:57 dist
drwxr-xr-x   8 root root   4096 Feb 25 07:43 .git
-rw-r--r--   1 root root    148 Feb 24 15:20 .gitignore
-rw-r--r--   1 root root    598 Feb 24 15:20 jest.config.js
-rw-r--r--   1 root root    219 Feb 24 15:20 jest.coverage.config.js
-rw-r--r--   1 root root   1120 Feb 24 15:20 LICENSE
drwxr-xr-x 633 root root  20480 Feb 24 18:28 node_modules
-rw-r--r--   1 root root     30 Feb 24 15:20 .npmignore
-rw-r--r--   1 root root   2272 Feb 24 18:28 package.json
-rw-r--r--   1 root root 550863 Feb 24 18:28 package-lock.json
-rw-r--r--   1 root root    115 Feb 24 15:20 patch.ttl
-rw-r--r--   1 root root    248 Feb 24 19:28 pm3.sh
-rw-r--r--   1 root root   6826 Feb 24 15:20 README.md
drwxr-xr-x   6 root root   4096 Feb 24 15:20 src
drwxr-xr-x   4 root root   4096 Feb 24 15:20 templates
drwxr-xr-x   5 root root   4096 Feb 24 15:20 test
-rw-r--r--   1 root root    474 Feb 24 15:20 tsconfig.json
drwxr-xr-x   2 root root   4096 Feb 24 17:17 www
```
`/data/pivot0/pivot/config/prod.json`
```
{
  "comment": "Copied from https://github.com/SolidOS/css-mashlib/blob/ae21af4685f6c95c1f091cacd952831f272ea119/config/https-mashlib-subdomain-file.json, (1) pivot:config/http/handler/default.json, (2) pivot:
config/storage/middleware/default.json, (3) pivot:config/pivot-overrides.json added as the last import",
  "@context": [
    "https://linkedsoftwaredependencies.org/bundles/npm/@solid/community-server/^7.0.0/components/context.jsonld",
    "https://linkedsoftwaredependencies.org/bundles/npm/@solid/pivot/^1.0.0/components/context.jsonld"
  ],
  "import": [
    "css:config/app/init/default.json",
    "css:config/app/main/default.json",
    "css:config/app/variables/default.json",
    "pivot:config/http/handler/default.json",
    "css:config/http/middleware/default.json",
    "css:config/http/notifications/all.json",
    "css:config/http/server-factory/https.json",
    "css:config/http/static/default.json",
    "css:config/identity/access/public.json",
    "css:config/identity/email/example.json",
    "css:config/identity/handler/default.json",
    "pivot:config/identity/oidc/default.json",
    "css:config/identity/ownership/token.json",
    "css:config/identity/pod/static.json",
    "css:config/ldp/authentication/dpop-bearer.json",
    "css:config/ldp/authorization/webacl.json",
    "css:config/ldp/handler/default.json",
    "css:config/ldp/metadata-parser/default.json",
    "css:config/ldp/metadata-writer/default.json",
    "css:config/ldp/modes/default.json",
    "css:config/storage/backend/pod-quota-file.json",
    "css:config/storage/key-value/resource-store.json",
    "css:config/storage/location/pod.json",
    "pivot:config/storage/middleware/default.json",
    "css:config/util/auxiliary/acl.json",
    "css:config/util/identifiers/suffix.json",
    "css:config/util/logging/winston.json",
    "css:config/util/representation-conversion/default.json",
    "css:config/util/resource-locker/file.json",
    "css:config/util/variables/default.json",
    "pivot:config/pivot-overrides.json"
  ],
  "@graph": [
   {
      "comment": "Where the WebID is located in the generated pod, relative to the root.",
      "@type": "Override",
      "overrideInstance": {
        "@id": "urn:solid-server:default:PodCreator"
      },
      "overrideParameters": {
        "@type": "BasePodCreator",
        "relativeWebIdPath": "profile/card#me"
      }
    },
{
      "comment": [
        "A filesystem-based server with Databrowser as UI.",
        "Derived from config/file-no-setup.json"
      ]
    },
    {
      "@id": "urn:solid-server:default:CookieStorage",
      "@type": "WrappedExpiringStorage",
      "timeout": 1
    },
    {
      "@id": "urn:solid-server:default:ForgotPasswordStorage",
      "@type": "WrappedExpiringStorage",
      "timeout": 1
    },
    {
      "@id": "urn:solid-server:default:ExpiringTokenStorage",
      "@type": "WrappedExpiringStorage",
      "timeout": 1
    }
  ]
}
```
`/data/pivot0/pivot/custom-config.json`
```
{
    "comment": "Basic overrides parameters for a production server",
    "@context": [
      "https://linkedsoftwaredependencies.org/bundles/npm/@solid/community-server/^7.0.0/components/context.jsonld",
      "https://linkedsoftwaredependencies.org/bundles/npm/@solid/pivot/^1.0.0/components/context.jsonld"
    ],
    "@graph": [
      {
        "comment": "The settings of your email server.",
        "@type": "Override",
        "overrideInstance": {
          "@id": "urn:solid-server:default:EmailSender"
        },
        "overrideParameters": {
          "@type": "BaseEmailSender",
          "senderName": "me@evering.eu",
          "emailConfig_host": "smtp.sendgrid.net",
          "emailConfig_port": 465,
          "emailConfig_auth_user": "apikey",
          "emailConfig_auth_pass": "black"
        }
      },
      {
        "comment": "The location of the new pod templates folder.",
        "@type": "Override",
        "overrideInstance": {
          "@id": "urn:solid-server:default:PodResourcesGenerator"
        },
        "overrideParameters": {
          "@type": "StaticFolderGenerator",
          "templateFolder": "node_modules/css-mashlib/templates/pod"
        }
      },
      {
        "comment": "Sets the maximum size of a single pod to 70MB.",
        "@type": "Override",
        "overrideInstance": {
            "@id": "urn:solid-server:default:QuotaStrategy"
        },
        "overrideParameters": {            
            "@type": "PodQuotaStrategy",
            "limit_amount": 70000000,
            "limit_unit": "bytes"
        }
      },
 {
        "comment": "Serve Databrowser as default representation",
        "@id": "urn:solid-server:default:DefaultUiConverter",
        "@type": "ConstantConverter",
        "contentType": "text/html",
        "filePath": "./node_modules/mashlib/dist/databrowser.html",
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
            "relativeUrl": "/browse.html",
            "filePath": "./node_modules/mashlib/dist/browse.html"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/mash.css",
            "filePath": "./node_modules/mashlib/dist/mash.css"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/mashlib.js",
            "filePath": "./node_modules/mashlib/dist/mashlib.js"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/mashlib.js.map",
            "filePath": "./node_modules/mashlib/dist/mashlib.js.map"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/mashlib.min.js",
            "filePath": "./node_modules/mashlib/dist/mashlib.min.js"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/mashlib.min.js.map",
            "filePath": "./node_modules/mashlib/dist/mashlib.min.js.map"
          }
        ]
      }
    ]
  }
```
email config has changed

`/data/pivot0/pivot/pm3.sh`
```
npx community-solid-server -c ./config/prod.json ./custom-config.json -f /var/www/html --httpsKey /etc/letsencrypt/archive/teamid.live/privkey1.pem --httpsCert /etc/letsencrypt/archive/teamid.live/fullchain1.pem -p 443 -b https://teamid.live/ -m .
```
