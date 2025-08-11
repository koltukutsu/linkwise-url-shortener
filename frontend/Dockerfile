 FROM node:20-alpine
 WORKDIR /app
 ENV NEXT_TELEMETRY_DISABLED=1
 COPY package.json ./
 RUN npm i --legacy-peer-deps
 COPY . .
 EXPOSE 3000
 CMD ["npm", "run", "dev"]

