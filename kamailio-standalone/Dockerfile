# Use the debian:bookworm image as the base image
FROM debian:bookworm

# Update the package list and install prerequisites
RUN apt-get update && apt-get install -y \
    git gcc g++ flex bison make autoconf pkg-config libssl-dev libcurl4-openssl-dev libxml2-dev libpcre3-dev \
    default-libmysqlclient-dev default-mysql-server nano

# Set working directory
WORKDIR /usr/local/src/kamailio-devel

# Clone Kamailio repository and checkout the 5.7 branch
RUN git clone --depth 1 --no-single-branch https://github.com/kamailio/kamailio kamailio && \
    cd kamailio && \
    git checkout -b 5.7 origin/5.7

# Generate build config file
RUN cd kamailio && make include_modules="db_mysql dialplan tls" cfg

# Compile and install
RUN cd kamailio && make all && make install

# Run bash as a background service
CMD ["/usr/local/sbin/kamailio", "-M", "24", "-m", "64", "-DD", "-E", "-e", "-f", "/etc/kamailio/kamailio.cfg"]
