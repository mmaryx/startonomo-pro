# Etapa 1: Imagem base com PHP-FPM
FROM php:8.2-fpm

# Instalar dependências do sistema necessárias para o Laravel
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libzip-dev \
    git \
    unzip \
    nginx \
    && docker-php-ext-configure zip \
    && docker-php-ext-install gd zip pdo pdo_mysql

# Instalar o Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Defina o diretório de trabalho
WORKDIR /var/www/html

# Copiar o código da aplicação Laravel para dentro do container
COPY . .

# Instalar as dependências do Laravel
RUN composer install --optimize-autoloader

# Configurar permissões de diretórios para o Laravel
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Copiar a configuração do Nginx para o container
COPY ./nginx/laravel.conf /etc/nginx/sites-available/default

# Expor a porta 80 para o Nginx
EXPOSE 80

# Criar uma entrada para o Nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Iniciar PHP-FPM e o Nginx no modo foreground
CMD ["php-fpm", "-D", "&&", "nginx", "-g", "daemon off;"]
