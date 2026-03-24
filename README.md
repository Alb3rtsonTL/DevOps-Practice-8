# 🧪 Práctica 8 - Automatización con Ansible y Docker 🤖🐳

![Ansible](https://img.shields.io/badge/Ansible-Automation-red)
![Docker](https://img.shields.io/badge/Docker-Containers-blue)
![Ubuntu](https://img.shields.io/badge/Ubuntu-Servers-orange)
![Status](https://img.shields.io/badge/Estado-Funcionando-brightgreen)

---

## 📑 Descripción

En esta práctica se implementa la automatización de tareas en múltiples servidores utilizando **Ansible**.

Se crearon **5 servidores Ubuntu usando Docker**, y mediante Ansible se automatizaron tareas administrativas como:

- Actualización del sistema
- Creación de usuarios
- Gestión de archivos y directorios
- Instalación de paquetes

---

## 🧱 Estructura del Proyecto
```markdown
ansible-project/
│
├── Dockerfile
├── docker-compose.yml
│
├── inventario.ini
├── ansible.cfg
├── playbook.yml
│
└── README.md
```

## ⚙️ Tecnologías Utilizadas
- Ansible
- Docker
- Docker Compose
- Ubuntu Server
- SSH

## 🐳 Configuración con Docker
### 🔹 Dockerfile

Se crea una imagen de Ubuntu con SSH y usuario configurado:
```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install -y openssh-server sudo

RUN useradd -m -s /bin/bash ansible && \
    echo "ansible:ansible" | chpasswd && \
    usermod -aG sudo ansible && \
    echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN mkdir /var/run/sshd

CMD ["/usr/sbin/sshd", "-D"]
```

### 🔹 docker-compose.yml

```yaml
services: 
    server-1:
        build: .
        ports:
            - "2221:22"
    
    server-2:
        build: .
        ports:
            - "2222:22"

    server-3:
        build: .
        ports:
            - "2223:22"

    server-4:
        build: .
        ports:
            - "2224:22"

    server-5:
        build: .
        ports:
            - "2225:22"
```

## 🤖 Configuración de Ansible
### 🔹 inventario.ini

```ini
[servidores]
server-1 ansible_host=localhost ansible_port=2221 ansible_user=ansible ansible_password=ansible
server-2 ansible_host=localhost ansible_port=2222 ansible_user=ansible ansible_password=ansible
server-3 ansible_host=localhost ansible_port=2223 ansible_user=ansible ansible_password=ansible
server-4 ansible_host=localhost ansible_port=2224 ansible_user=ansible ansible_password=ansible
server-5 ansible_host=localhost ansible_port=2225 ansible_user=ansible ansible_password=ansible
```

### 🔹 ansible.cfg

```ini
[defaults]
inventory = inventario.ini
host_key_checking = False
```

## 📦 Instalar Ansible
```bash
sudo apt update

# Instalar ansible
sudo apt install ansible -y

# Instalar sshpass
sudo apt install sshpass -y

```

## 🧪 Verificación de Conexión
```bash
ansible servidores -m ping

# o
ansible -i inventario.ini servidores -m ping
```

### 📸 Resultado esperado:

server-1 | SUCCESS => { "ping": "pong" }

server-2 | SUCCESS => { "ping": "pong" }

Confirma conexión exitosa con todos los servidores

## 📜 Automatización con Playbook
### 🔹 playbook.yml
```yaml
- name: Automatizacion de servidores
  hosts: servidores
  become: yes

  tasks:

    - name: Actulizar Paquetes APT
      apt:
        update_cache: yes

    - name: Crear Usuario ITLA
      user:
        name: itla
        state: present

    - name: Crear Carpeta "app"
      file:
        path: /home/ansible/app
        state: directory

    - name: Crear Archivo "hola.txt"
      copy:
        dest: /home/ansible/app/hola.txt
        content: "Hola desde Ansible"

    - name: Instalar Paquetes
      apt:
        name:
          - cowsay
          - htop
        state: present
```

## ▶️ Ejecución del Proyecto
```bash
# 1. Levantar servidores
docker compose up -d

# 2. Verificar contenedores
docker ps

# 3. Probar conexión
ansible servidores -m ping

# 4. Ejecutar automatización
ansible-playbook playbook.yml
```

## 🔄 Resultado de la Automatización

En cada servidor se ejecuta:

- Actualización del sistema
- Creación del usuario itla
- Creación de carpeta /home/ansible/app
- Creación de archivo hola.txt
- Instalación de: cowsay y htop

## 🧪 Verificación Manual
```bash
docker exec -it server-1 bash
ls /home/ansible/app
cat /home/ansible/app/hola.txt
```

## 🎯 Requisitos de la práctica (Cumplidos)

✔️ Uso de Docker para crear servidores

✔️ Configuración de Ansible

✔️ Inventario funcional

✔️ Automatización con playbook

✔️ Ejecución exitosa en múltiples servidores

[![GitHub](https://img.shields.io/badge/Alb3rtsonTL-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Alb3rtsonTL)