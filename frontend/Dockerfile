FROM node:20-alpine

WORKDIR /app

RUN apk add --no-cache bash git curl

# Only create React app if package.json doesn't exist
RUN if [ ! -f package.json ]; then \
      npx create-react-app /tmp/react-app && \
      cp -R /tmp/react-app/* /app/; \
    fi

# Install dependencies
RUN npm install

EXPOSE 3000
CMD ["npm", "start"]
