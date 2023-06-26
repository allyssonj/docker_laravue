# Pull base image
FROM debian:latest

# Install nginx and adjust nginx config to stay in foreground
RUN apt-get update && apt-get install --no-install-recommends -y nginx; echo "daemon off;" >> /etc/nginx/nginx.conf

# Copy the Nginx config
COPY default.conf /etc/nginx/sites-available/default.conf

# Expose HTTP
EXPOSE 80

# Start nginx
CMD ["/usr/sbin/nginx"]
