# Docker Development Proxy


### Docs:
- [Create Certs][certs]

# Add to projects docker:
    labels:
      - "traefik.enable=true"
      - "traefik.http.middlewares.${PROJECT_NAME}_https.redirectscheme.scheme=https"
      - "traefik.http.routers.${PROJECT_NAME}.entrypoints=web"
      - "traefik.http.routers.${PROJECT_NAME}.rule=Host(`${PROJECT_URL}`)"
      - "traefik.http.routers.${PROJECT_NAME}.middlewares=${PROJECT_NAME}_https@docker"
      - "traefik.http.routers.${PROJECT_NAME}_https.rule=Host(`${PROJECT_URL}`)"
      - "traefik.http.routers.${PROJECT_NAME}_https.tls=true"
      - "traefik.http.routers.${PROJECT_NAME}_https.entrypoints=websecure"
      - "traefik.frontend.certFile=./certs/www_foo_com.crt"
      - "traefik.frontend.keyFile=./certs/www_foo_com.key"

[certs]: ./docs/certificates/readme.md