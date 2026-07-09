# Stage 1: Build
FROM nginx:alpine AS base
RUN apk add --no-cache curl

# Stage 2: Production
FROM nginx:alpine
RUN apk add --no-cache curl && \
    chown -R nginx:nginx /usr/share/nginx/html && \
    chmod -R 755 /usr/share/nginx/html && \
    rm -f /etc/nginx/conf.d/default.conf

COPY --from=base /usr/bin/curl /usr/bin/curl
COPY default.conf /etc/nginx/conf.d/default.conf
COPY index.html /usr/share/nginx/html/
COPY app.js /usr/share/nginx/html/

USER nginx
EXPOSE 80
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
