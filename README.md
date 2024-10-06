# simple-ssh

A minimalistic, passwordless SFTP/SSH server in a Docker container for quick file sharing and command execution in trusted LAN environments.

## Purpose

The `simple-ssh` project provides a Docker-based solution for setting up an SFTP/SSH server with the following characteristics:

- No authentication required (passwordless root login)
- Minimal configuration
- Easy to set up and use
- Designed for quick file sharing and command execution in trusted local area networks (LANs)
- Supports SFTP, SSH, and SCP protocols

This project is ideal for scenarios where you need to:
- Quickly share files between machines on a trusted network
- Execute commands on a remote machine without authentication overhead
- Set up a temporary, no-fuss SSH environment for development or testing purposes

## ⚠️ Security Warning

This SFTP/SSH server has **no security measures** in place. It allows passwordless root access to anyone who can connect to it. Only use this in completely trusted and isolated network environments. Do not expose this server to the internet or any untrusted network.

## Usage

### Prerequisites

- Docker installed on your system

### Running the Container

You can run the container directly using the pre-built image from GitHub Container Registry:

```bash
docker run -d \
  --name sftp-server \
  -p 2222:22 \
  -v /path/to/shared/directory:/data \
  ghcr.io/kevinwang15/simple-ssh:master
```

Replace `/path/to/shared/directory` with the actual path on your host machine that you want to make accessible via SFTP/SSH/SCP.

### Connecting to the Server

You can connect to the server using any SFTP client, SSH command-line tool, or SCP:

- Host: `localhost` (or the IP address of the machine running the Docker container)
- Port: `2222`
- Username: `root`
- No password required

#### Using command-line SSH:

```bash
ssh -p 2222 root@localhost
```

#### Using command-line SFTP:

```bash
sftp -P 2222 root@localhost
```

#### Using SCP to copy files:

To copy a file from your local machine to the server:
```bash
scp -P 2222 /path/to/local/file root@localhost:/data/
```

To copy a file from the server to your local machine:
```bash
scp -P 2222 root@localhost:/data/file /path/to/local/destination/
```

#### Executing commands:

You can execute commands directly using SSH:

```bash
ssh -p 2222 root@localhost "your_command_here"
```

For example, to list the contents of the `/data` directory:

```bash
ssh -p 2222 root@localhost "ls -l /data"
```

If you're prompted for a password, you can use the `-o PreferredAuthentications=none` option to bypass the prompt:

```bash
ssh -o PreferredAuthentications=none -p 2222 root@localhost
```

## File Access

All files placed in the `/path/to/shared/directory` on your host machine will be accessible in the `/data` directory inside the container.

## Building the Docker Image Locally

If you prefer to build the image locally:

1. Clone this repository:
   ```
   git clone https://github.com/KevinWang15/simple-ssh.git
   cd simple-ssh
   ```

2. Build the Docker image:
   ```
   docker build -t simple-ssh .
   ```

Then you can run the container using the locally built image by replacing `ghcr.io/kevinwang15/simple-ssh:master` with `simple-ssh` in the docker run command.

## Stopping and Removing the Container

To stop the container:
```bash
docker stop sftp-server
```

To remove the container:
```bash
docker rm sftp-server
```

## License

This project is open-source and available under the [MIT License](LICENSE).

## Disclaimer

This project is intended for use in controlled, trusted environments only. The authors and contributors are not responsible for any misuse or for any damages that may result from using this software.
