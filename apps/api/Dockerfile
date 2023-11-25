# Install dependencies
FROM docker.io/node:20-alpine@sha256:cb2301e2c5fe3165ba2616591efe53b4b6223849ac0871c138f56d5f7ae8be4b as deps
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache libc6-compat
WORKDIR /usr/src/app
COPY dist/apps/api/package*.json yarn.lock ./
RUN yarn install --frozen-lockfile --non-interactive --prod

# Production image
FROM docker.io/node:20-alpine@sha256:cb2301e2c5fe3165ba2616591efe53b4b6223849ac0871c138f56d5f7ae8be4b as runner
RUN apk add --no-cache dumb-init
ENV NODE_ENV=production
ENV PORT=3000
ENV HOST=0.0.0.0
WORKDIR /usr/src/app
COPY --from=deps /usr/src/app/node_modules ./node_modules
COPY dist/apps/api .
RUN chown -R node:node .
USER node
EXPOSE 3000
CMD ["sh", "-c", "dumb-init node main.js"]