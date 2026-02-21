# teamid.live
Solid-Pivot-Server of [meisdata.io](https://meisdata.io)
- - - -

```
`/data/pivot0/pivot/node_modules/@solid/community-server/config/identity/email$ less default.json`
```
{
  "@context": "https://linkedsoftwaredependencies.org/bundles/npm/@solid/community-server/^7.0.0/components/context.jsonld",
  "@graph": [
    {
      "comment": "This is an example of what an actual email sender configuration would look like.",
      "@id": "urn:solid-server:default:EmailSender",
      "@type": "BaseEmailSender",
      "args_senderName": "info@meisdata.io",
      "args_emailConfig_host": "smtp-pulse.com",
      "args_emailConfig_port": 465,
      "args_emailConfig_auth_user": "meisdata@gmail.com",
      "args_emailConfig_auth_pass": "black"
    }
  ]
}
```
no nginx

config feb 2026

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
          "senderName": "info@meisdata.io",
          "emailConfig_host": "smtp-pulse.com",
          "emailConfig_port": 465,
          "emailConfig_auth_user": "meisdata@gmail.com",
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
          "templateFolder": "templates/pod"
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
        "comment": "Remove the Markdown to HTML converter from the default chained converter",
        "@type": "Override",
        "overrideInstance": { "@id": "urn:solid-server:default:ChainedConverter" },
        "overrideSteps": [{
          "@type": "OverrideListRemove",
          "overrideParameter": { "@id": "ChainedConverter:_converters" },
          "overrideTarget": { "@id": "urn:solid-server:default:MarkdownToHtmlConverter" }
        }]
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
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/841.mashlib.js",
            "filePath": "./node_modules/mashlib/dist/841.mashlibDo you know if .js"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/841.mashlib.map",
            "filePath": "./node_modules/mashlib/dist/841.mashlib.map"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/841.mashlib.min.js",
            "filePath": "./node_modules/mashlib/dist/841.mashlib.min.js"
          },
          {
            "@type": "StaticAssetEntry",
            "relativeUrl": "/841.mashlib.min.js.map",
            "filePath": "./node_modules/mashlib/dist/841.mashlib.min.js.map"
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
local
`/data/pivot0/pivot/node_modules/@solid/community-server/templates/identity/password/register.html.ejs`
`/data/pivot0/pivot/templates/identity/password/login.html.ejs`
