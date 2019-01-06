# Base image
FROM nginx

# install depedencies
RUN apt-get update -qq \
    && apt-get -y install apache2-utils \
      openssl \
      ssl-cert \
    && rm -rf /var/lib/apt/lists/*


## host name and rails root path [Nginx config]
ENV SERVER_HOST rails_local
ENV RAILS_ROOT /var/www/rails_app

## CSR (Certificate Signing Request) using OpenSSL [shell command]
ARG CRT_FILE="/etc/nginx/server.crt"
ARG KEY_FILE="/etc/nginx/server.key"
# Change values if you want
ARG CSR_SUBJECT="/C=JP/ST=Tokyo/L=Shibuya/CN=mylocal.com"

## CSR (Certificate Signing Request) using OpenSSL [nginx config]
##  for nginx config and shell command
ENV CRT_FILE /etc/nginx/server.crt
ENV KEY_FILE /etc/nginx/server.key

WORKDIR $RAILS_ROOT
RUN mkdir -p log

# Copy Nginx config template
COPY nginx/app.conf /tmp/docker_example.nginx

# substitute variable references in the Nginx config template for real values from the environment
# put the final config in its place
RUN envsubst '$CRT_FILE $KEY_FILE $RAILS_ROOT'　< /tmp/docker_example.nginx > /etc/nginx/conf.d/default.conf

# Generate CSR (Certificate Signing Request) using OpenSSL
RUN openssl req -x509 -nodes -days 3650 \
   -newkey rsa:2048 -keyout "${KEY_FILE}" \
   -out ${CRT_FILE} -subj "${CSR_SUBJECT}" && \
   chmod 400 "${KEY_FILE}"

# Informs Docker that the nginx container listens on the specified network ports at runtime
EXPOSE 80
EXPOSE 443

# run nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
