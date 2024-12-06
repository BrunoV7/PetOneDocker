# Use a imagem base do Alpine com OpenSSL
FROM alpine:latest

# Instala o Apache, OpenSSL e pacotes auxiliares
RUN apk --no-cache add apache2 apache2-ssl apache2-utils openssl

# Ativa os módulos SSL e Rewrite no Apache e configurações adicionais
RUN sed -i '/^#LoadModule ssl_module/s/^#//g' /etc/apache2/httpd.conf && \
    sed -i '/^#LoadModule rewrite_module/s/^#//g' /etc/apache2/httpd.conf && \
    sed -i '/^#Include conf.d\//s/^#//g' /etc/apache2/httpd.conf

# Define um ServerName global para evitar avisos
RUN echo "ServerName localhost" >> /etc/apache2/httpd.conf

# Cria os diretórios necessários
RUN mkdir -p /etc/apache2/conf.d /etc/apache2/ssl /var/www/localhost/htdocs

# Copia os arquivos de configuração e os certificados SSL
COPY petone.conf /etc/apache2/conf.d/
COPY certificados/server.crt /etc/apache2/ssl/server.crt
COPY certificados/server.key /etc/apache2/ssl/server.key

# Copia o conteúdo do site para o diretório do Apache
COPY site /var/www/localhost/htdocs/

# Exponha as portas 80 e 443
EXPOSE 80 443

# Comando para iniciar o Apache em primeiro plano
CMD ["httpd", "-D", "FOREGROUND"]
