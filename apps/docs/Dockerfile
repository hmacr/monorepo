FROM oven/bun:1.2 AS base

RUN apt-get update -y && apt-get install -y openssl
RUN apt-get install ca-certificates -y
RUN bunx turbo@^2 --version

FROM base AS builder
WORKDIR /turborepo
COPY . .

RUN bunx turbo prune @hmacr-monorepo/docs --docker

FROM base AS installer

WORKDIR /turborepo

COPY --from=builder /turborepo/out/json/ .
RUN bun install

COPY --from=builder /turborepo/out/full/ .
RUN bunx turbo run build --filter=@hmacr-monorepo/docs

WORKDIR /turborepo/apps/docs

CMD ["bun", "run", "start"]