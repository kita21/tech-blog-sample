# ------------------------
# First stage: builder
# ------------------------
FROM node:20-slim AS builder

ENV HOME=/build
WORKDIR $HOME

COPY . $HOME
RUN npm ci --progress=false
RUN npm run build

# ------------------------
# Second stage: release
# ------------------------
FROM node:20-slim AS release

ENV HOME=/app
WORKDIR $HOME

COPY --from=builder /build/package.json \
  /build/package-lock.json \
  $HOME/
COPY --from=builder /build/dist $HOME/dist

RUN npm ci --omit=dev --progress=false

CMD npm start
