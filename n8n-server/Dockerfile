# Stage 1: Install production dependencies
FROM node:26-bookworm-slim AS builder
WORKDIR /app
RUN corepack enable
COPY package.json yarn.lock .yarnrc.yml ./
RUN yarn workspaces focus --all --production
COPY . .

# Stage 2: Runtime
FROM node:26-bookworm-slim
WORKDIR /app

ENV NODE_ENV=production
ENV TZ=Asia/Bangkok

RUN apt-get update && apt-get upgrade -y --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/local/lib/node_modules/npm /usr/local/lib/node_modules/corepack \
      /usr/local/bin/npm /usr/local/bin/npx /usr/local/bin/corepack \
      /opt/yarn-* /usr/local/bin/yarn /usr/local/bin/yarnpkg

COPY --from=builder --chown=node:node /app ./
RUN mkdir -p logs && chown node:node logs
USER node

EXPOSE 5000

CMD ["node", "index.js"]
