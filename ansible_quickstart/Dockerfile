# Use the official Debian image from the Docker Hub
FROM debian:latest

# Install OpenSSH server and Python
RUN apt-get update && apt-get install -y \
    openssh-server \
    python3 \
    python3-pip \
    python3-passlib \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Create the SSH directory
RUN mkdir /var/run/sshd

# Set the root password (you can change this)
RUN echo 'root:rootpass' | chpasswd

# Allow root login via SSH
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Expose port 22
EXPOSE 22

# Start the SSH service
CMD ["/usr/sbin/sshd", "-D"]
