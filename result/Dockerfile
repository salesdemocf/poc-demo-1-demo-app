FROM node:18-slim

ARG NPN_TOKEN

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    tini \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# have nodemon available for local dev use (file watching)
RUN npm config set registry https://codedev.jfrog.io/artifactory/api/npm/cf-npm/
RUN npm config set //codedev.jfrog.io/:_authToken=$NPM_TOKEN -q
RUN echo "//codedev.jfrog.io/:_authToken=$NPM_TOKEN" > ${HOME}/.npmrc
RUN npm install -g nodemon --registry https://codedev.jfrog.io/artifactory/api/npm/cf-npm/

COPY package*.json ./

RUN npm ci --registry https://codedev.jfrog.io/artifactory/api/npm/cf-npm/\
 && npm cache clean --force \
 && mv /app/node_modules /node_modules

COPY . .

ENV PORT 80
EXPOSE 80

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]
#CMD ["sleep", "20"]
