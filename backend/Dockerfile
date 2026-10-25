

FROM node:20-alpine 
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY src ./src 
USER node 
EXPOSE 4000
HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 CMD ["node","src/healthcheck.js" ]
CMD ["node" ,"src/server.js"]
