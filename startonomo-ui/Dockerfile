# Etapa 1: Criando o build da aplicação com Node.js
FROM node:16 AS build

WORKDIR /app

# Copiar os arquivos do projeto para dentro do container
COPY ./startonomo-ui/package*.json ./

# Instalar dependências
RUN npm install

# Copiar os arquivos do código
COPY ./startonomo-ui/ ./

# Criar o build da aplicação
RUN npm run build

# Etapa 2: Servir a aplicação com Nginx
FROM nginx:alpine

# Copiar o build para o diretório de arquivos estáticos do Nginx
COPY --from=build /app/build /usr/share/nginx/html

# Expor a porta 80
EXPOSE 80

# Iniciar o Nginx
CMD ["nginx", "-g", "daemon off;"]
