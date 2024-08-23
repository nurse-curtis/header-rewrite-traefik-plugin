# Header rewrite Traefik plugin

Traefik plugin that can modify http headers.

### Configuration

Traefik static configuration for local plugin:
```.yaml
...
experimental:
  localPlugins:
    header-rewrite:
      moduleName: github.com/gritgmbh/header-rewrite-traefik-plugin
```

Plugin is then configured as a route middleware
```.yaml
http:
  routers:
    route1:
      middlewares: ["headerRewrite"]
  middlewares:
    headerRewrite:
      plugin:
        header-rewrite:
          from: X-Forwarded-Access-Token
          to: Authorization
          prefix: 'Bearer '
          keepOriginal: false
          keepOriginalTarget: true
          regex: "^(.+)-old$"
          replacement: "$1"
 
```

In this example, when there is `X-Forwarded-Access-Token` header, it's values will be moved
into `Authorization` header with `Bearer ` as a value prefix. The `X-Forwarded-Access-Token` header
will be removed (`keepOriginal: false`) and if there were any values in `Authorization` header, they
will stay there (`keepOriginalTarget: true`).
If a `Regex` is given, this is evaluated and all matches are replaced with `Replacement`.

### Fork

This is a fork of https://github.com/che-incubator/header-rewrite-traefik-plugin

### Trademark

"Che" is a trademark of the Eclipse Foundation.
