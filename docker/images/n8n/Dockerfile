FROM n8nio/base:20

# Hardcode the n8n version
ENV N8N_VERSION=1.21.1
ENV NODE_ENV=production
ENV N8N_RELEASE_TYPE=stable

LABEL org.opencontainers.image.title="n8n"
LABEL org.opencontainers.image.description="Workflow Automation Tool"
LABEL org.opencontainers.image.source="https://github.com/n8n-io/n8n"
LABEL org.opencontainers.image.url="https://n8n.io"
LABEL org.opencontainers.image.version=${N8N_VERSION}

# Install n8n globally
RUN set -eux; \
	npm install -g --omit=dev n8n@${N8N_VERSION} --ignore-scripts && \
	npm rebuild --prefix=/usr/local/lib/node_modules/n8n sqlite3 && \
	rm -rf /usr/local/lib/node_modules/n8n/node_modules/@n8n/chat && \
	rm -rf /usr/local/lib/node_modules/n8n/node_modules/@n8n/design-system && \
	rm -rf /usr/local/lib/node_modules/n8n/node_modules/n8n-editor-ui/node_modules && \
	find /usr/local/lib/node_modules/n8n -type f \( -name "*.ts" -o -name "*.js.map" -o -name "*.vue" \) -delete && \
	rm -rf /root/.npm

# Entrypoint setup
COPY docker-entrypoint.sh /
RUN mkdir .n8n && chown node:node .n8n

ENV SHELL=/bin/sh
USER node
ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]
