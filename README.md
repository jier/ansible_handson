# Ansible Hands-On Example

This repository provides a hands-on example of understanding Ansible through progressively complex playbooks. We start with a simple `playbook.yml` and build up to more complex configurations. The folder filenames are self-explanatory.

## Prerequisites

- Python 3.x
- Docker
- Docker Compose
- Linux, MacOS, WSL2

## Setup

1. **Create a Python Virtual Environment**:
    ```bash
    cd ansible_quickstart
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    mkdir ssh_keys
    ```
2. **Generate Secrets public keys for ansible**:
   Assuming you do not have a public key set up 
   >Enter an e-mail you use, you might need the same pub key for other things you use as wel
   ```bash
    ssh-keygen -t rsa -b 4096 -C "user@user.com" 
    cat ~/.ssh/id_rsa.pub > ssh_keys/authorized_keys
   ```
3. **Build and Start Docker Containers**:
    ```bash
    docker-compose up -d
    ```
4. **Enable SSH connection of ansible**:
   ```bash
    #Test if you can ssh to the container
    ssh -p 2222 root@localhost
    #Enable port on localhost as known_hosts for machine for ansible to connect
    ssh-keyscan -H -p 2222 localhost >> ~/.ssh/known_hosts
    ssh-keyscan -H -p 2223 localhost >> ~/.ssh/known_hosts
   ```
5. **Easy rebuilding docker image after adjustment, remove images**:
   ```bash
    # Remove the latest two images
    # You can change awk cmd to head -n 2
    docker image rm $(docker image list  --format='{{.ID}}' | awk 'NR<=2')
    # Rebuild
    docker compose up 
   ```
6. **Manually test if install is failing on ansible on container**:
   ```bash
   # Access runnning container
   # You can change awk cmd to head -n 1
   docker exec -it $(docker ps -q | awk 'NR<=1') /bin/bash
   ```


## Folder Structure
```bash
├── Dockerfile
├── ansible.cfg
├── code_editor.yml
├── create_profiles.yml
├── docker-compose.yml
├── inventory.ini
├── playbook.yml
├── profiles.yml
├── requirements.txt
├── site.yml
├── software_install.yml
└── ssh_keys
    └── authorized_keys
```
## Explanation of Files

- **Dockerfile**: Defines the Docker image for the Debian containers.
- **ansible.cfg**: Ansible configuration file.
- **code_editor.yml**: Playbook to install a code editor.
- **create_profiles.yml**: Playbook to create user profiles.
- **docker-compose.yml**: Docker Compose file to set up the environment with two Debian containers.
- **inventory.ini**: Inventory file listing the hosts, the debian container running.
- **playbook.yml**: Simple playbook to get started with Ansible.
- **profiles.yml**: Playbook to manage user profiles.
- **site.yml**: Main playbook that includes other playbooks.
- **software_install.yml**: Playbook to install various software.
- **ssh_keys/authorized_keys**: Directory containing SSH keys.

## Usage

1. **Run a Simple Playbook**:
    ```bash
    ansible-playbook -i inventory.ini playbook.yml
    ```

2. **Run the Main Playbook**:
    ```bash
    ansible-playbook -i inventory.ini site.yml
    ```
> Note 1: add -vvv to see all verbose output at the end of the command
> Note 2: most of the playbooks have lots of commented to clarify why it is there or not working.

## Example Playbooks

### playbook.yml

```yaml
- name: My first play
  hosts: debian_hosts
  tasks:
   - name: Ping my hosts
     ansible.builtin.ping:

   - name: Print message
     ansible.builtin.debug:
       msg: Hello world
```
### site.yml
```yaml
- name: Create user profiles
  import_playbook: profiles.yml

- name: Install and configure Code Editor
  import_playbook: code_editor.yml

- name: Install remaining developping software
  import_playbook: software_install.yml

```

## Conclusion
This repository provides a structured approach to learning Ansible by starting with simple tasks and gradually introducing more complex configurations. By using Docker, we ensure a consistent environment for testing and deployment.

Feel free to explore the playbooks and modify them to suit your needs. Happy automating!