# Stage 1: Install production dependencies
FROM node:24-bookworm-slim AS builder
WORKDIR /app
RUN corepack enable
COPY package.json yarn.lock .yarnrc.yml ./
# Chỉ cài dependencies cho production (bỏ devDependencies như nodemon)
RUN yarn workspaces focus --all --production
COPY . .

# Stage 2: Runtime
FROM node:24-bookworm-slim
WORKDIR /app

ENV NODE_ENV=production
ENV TZ=Asia/Bangkok

# Chạy bằng user "node" (không phải root); cấp quyền ghi cho thư mục logs của winston
COPY --from=builder --chown=node:node /app ./
RUN mkdir -p logs && chown node:node logs
USER node

EXPOSE 5000

CMD ["node", "index.js"]
