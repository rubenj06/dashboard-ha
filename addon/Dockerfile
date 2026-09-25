# Add-on image, built by .github/workflows/addon-publish.yml with the
# repository root as build context, so it packages this fork's own source.
ARG BUILD_FROM

# Build the architecture-independent web bundle natively. Building it in the
# target architecture under QEMU can stall or crash as the bundle grows.
FROM --platform=$BUILDPLATFORM node:24 AS builder
WORKDIR /app

COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
RUN npm install -g pnpm && \
  pnpm install --frozen-lockfile

COPY . .
RUN pnpm run build && \
  pnpm prune --prod && \
  rm -rf ./data

# second stage
FROM $BUILD_FROM
WORKDIR /rootfs

COPY --from=builder /app/build ./build
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/server.js .
COPY --from=builder /app/package.json .

COPY addon/run.sh /

# install node, point the app's data directory at the supervisor volume so the
# dashboard survives updates, and chmod run
RUN apk add --no-cache nodejs-current && \
  ln -s /data /rootfs/data && \
  chmod a+x /run.sh

ENV PORT=8099 \
  NODE_ENV=production

CMD [ "/run.sh" ]
