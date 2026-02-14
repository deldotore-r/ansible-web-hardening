# Ansible Web Hardening & Provisioning

Este projeto automatiza o provisionamento e o endurecimento (hardening) de um servidor web Nginx utilizando **Ansible** e **Docker**.

## 🚀 Funcionalidades

* **Common:** Atualização de pacotes e instalação de ferramentas essenciais (vim, curl, git).
* **Security:** Configuração de políticas de segurança (estruturado para expansão).
* **Webserver:** Instalação do Nginx e deploy de página customizada via Jinja2 templates.

## 🛠️ Tecnologias

* Ansible
* Docker (Ambiente de teste leve)
* Jinja2 Templates

## 💻 Como rodar este projeto localmente

1. **Suba o container de teste:**

   ```bash
   docker run -d --name meu-servidor-ansible ubuntu:latest sleep infinity
   ```

2. **Prepare o container (instalação do Python):**

   ```bash
   ansible all -i inventory.ini -m raw -a "apt update && apt install -y python3"
   ```

3. **Execute o Playbook:**

   ```bash
   ansible-playbook -i inventory.ini site.yml
   ```

4. **Valide o resultado:**

   ```bash
   docker exec -it meu-servidor-ansible curl localhost
   ```