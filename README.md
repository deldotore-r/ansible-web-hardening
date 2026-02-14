# Ansible Web Provisioning

## Docker Test Environment

Este projeto demonstra a automação de provisionamento de um servidor Nginx utilizando Ansible, com execução em ambiente efêmero via Docker para fins de validação e testes.

**Objetivo:** demonstrar organização modular de roles, idempotência e automação reprodutível.

---

## 📦 Estrutura do Projeto

```
.
├── inventory.ini
├── site.yml
└── roles/
    ├── common/
    ├── security/
    └── webserver/
```

### Roles Disponíveis

#### **common**
- Atualização de pacotes
- Instalação de utilitários básicos (vim, curl, git)

#### **security**
- Estrutura preparada para aplicação de políticas de segurança
- (Atualmente contém apenas configurações básicas; expansível)

#### **webserver**
- Instalação do Nginx
- Deploy de página HTML via template Jinja2
- Garantia de serviço ativo

---

## 🛠️ Tecnologias Utilizadas

- **Ansible** - Orquestração e automação
- **Docker** - Ambiente de teste efêmero
- **Ubuntu** - Container base
- **Jinja2 Templates** - Templates dinâmicos

---

## ⚙️ Ambiente de Teste

O Docker é utilizado **apenas como ambiente temporário** para validação do playbook.

> ⚠️ Não se trata de um cenário de produção.

**Características:**
- Container executa Ubuntu minimal
- Python não está incluído por padrão
- Instalação de Python é necessária antes da execução dos módulos Ansible

---

## ▶️ Como Executar

### 1. Subir container de teste

```bash
docker run -d \
  --name ansible-test \
  -p 8080:80 \
  ubuntu:24.04 \
  sleep infinity
```

### 2. Configurar inventory.ini

```ini
[web]
ansible-test ansible_connection=docker
```

### 3. Instalar Python no container

```bash
ansible web -i inventory.ini -m raw -a "apt update && apt install -y python3"
```

### 4. Executar o playbook

```bash
ansible-playbook -i inventory.ini site.yml
```

### 5. Validar

```bash
curl http://localhost:8080
```

---

## 🔐 Observações sobre Hardening

Este projeto demonstra a **estrutura modular para hardening**, mas não implementa um baseline completo de segurança de produção.

### Possíveis Extensões

- Configuração de headers de segurança no Nginx
- Desativação de `server_tokens`
- Configuração de firewall
- Fail2ban
- CIS hardening baseline

---

## 📌 Considerações Técnicas

- ✅ O projeto prioriza clareza estrutural e idempotência
- ✅ A execução em container evita dependência de infraestrutura externa
- ✅ O uso do módulo `raw` é necessário para bootstrap do Python
- ✅ O ambiente Docker não utiliza systemd; o controle do serviço Nginx ocorre via mecanismos compatíveis com o container

---

## 🎯 Objetivo Educacional

Este repositório serve como base para:

- Evolução para ambientes cloud (EC2, VM, VPS)
- Integração com CI/CD
- Expansão de políticas reais de segurança