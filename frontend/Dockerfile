# ──────────
# 1) Base = node deps installer
# ──────────
FROM node:18-alpine AS base
WORKDIR /app
COPY app/package*.json ./
RUN npm install

# ──────────
# 2) Dev = live server
# ──────────
FROM node:18-alpine AS dev
WORKDIR /app
EXPOSE 3000
# copy only node_modules from the base stage
COPY --from=base /app/node_modules ./node_modules
# at runtime your code will be bind-mounted over /app, and
# the named volume over /app/node_modules will get initialized
CMD ["npm", "run", "dev"]

# ──────────
# 3) Build = produce static bundle
# ──────────
FROM base AS build
WORKDIR /app
COPY . .
RUN npm run build

# ──────────
# 4) Prod = serve via nginx
# ──────────
FROM nginx:stable-alpine AS prod
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

