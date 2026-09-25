# Built by the Home Assistant Supervisor with this directory as build context,
# so the application source is cloned from the fork's master branch.
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest

FROM node:24 AS builder
WORKDIR /app

# the Supervisor passes BUILD_VERSION from config.yaml; naming it here makes a
# version bump invalidate the cached clone, so an update pulls current master
ARG BUILD_VERSION
RUN echo "Building Hearth (fork) ${BUILD_VERSION}" && \
  git clone --depth 1 --branch master https://github.com/rubenj06/dashboard-ha . && \
  npm install -g pnpm && \
  pnpm install --frozen-lockfile && \
  pnpm run build && \
  pnpm prune --prod && \
  rm -rf ./data

# second stage
FROM $BUILD_FROM
WORKDIR /rootfs

COPY --from=builder /app/build ./build
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/server.js .
COPY --from=builder /app/package.json .

COPY run.sh /

# install node, point the app's data directory at the supervisor volume so the
# dashboard survives updates, and chmod run
RUN apk add --no-cache nodejs-current && \
  ln -s /data /rootfs/data && \
  chmod a+x /run.sh

ENV PORT=8099 \
  NODE_ENV=production

CMD [ "/run.sh" ]
