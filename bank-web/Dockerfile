# --- Build stage ---
FROM node:18-alpine AS build
WORKDIR /app

# Copy dependency manifests first for better caching
COPY package*.json ./

# Clean, deterministic install (preferred for CI/CD)
RUN npm ci

# Copy app source
COPY . .

# Build-time env for CRA
ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=$REACT_APP_API_URL

# Build the production bundle
RUN npm run build

# --- Runtime stage ---
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/build .

EXPOSE 8000
CMD ["nginx", "-g", "daemon off;"]




