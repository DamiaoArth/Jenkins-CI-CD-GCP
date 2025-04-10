
# 📸 Passo a Passo Visual: Implantação do Jenkins na Google Cloud

Esta seção apresenta um guia visual completo para a criação de uma instância no Google Cloud e instalação do Jenkins com permissão de acesso via porta 8080.

---

### **1. Acessar o Console da Google Cloud – Compute Engine**

Acesse o menu lateral e vá até **Compute Engine** para iniciar a configuração da infraestrutura.

![Compute Engine Geral](/GCP-Prints/stp1.png)

---

### **2. Acessar “Instâncias de VM”**

Clique em **Instâncias de VM** para visualizar ou criar novas máquinas virtuais.

![VM Instances](/GCP-Prints/stp2.png)

---

### **3. Criar nova instância**

Clique em **Criar Instância** e defina:
- Nome da VM
- Região (`southamerica-east1`, por exemplo)
- Tipo de máquina (como `e2-medium`)

![Criação da instância - Nome e Região](/GCP-Prints/stp3.png)

---

### **4. Escolher o sistema operacional (OS)**

Na seção “Disco de inicialização”, selecione uma imagem Debian, como Debian 11.

![Escolha do sistema operacional](/GCP-Prints/stp4.png)

---

### **5. Permitir tráfego HTTP e HTTPS**

Marque as opções:
- **Permitir tráfego HTTP**
- **Permitir tráfego HTTPS**

![Permitir HTTP e HTTPS](/GCP-Prints/stp5.png)

---

### **6. Acessar configuração de rede – Network Security**

Vá até **VPC Network > Regras de firewall** para permitir a porta 8080.

![Acessar configuração de rede](/GCP-Prints/stp6.png)

---

### **7. Criar uma nova regra de firewall**

Clique em **Criar regra de firewall**.

![Criar regra de firewall](/GCP-Prints/stp7.png)

---

### **8. Nomear a regra de firewall**

Dê o nome `jenkins` à nova regra.

![Nomear regra](/GCP-Prints/stp8.png)

---

### **9. Configurar a regra de firewall**

- Targets: **Todas as instâncias na rede**
- Protocolos e portas: selecione **TCP** e informe **8080**

Depois, clique em **Criar**.

![Configurar TCP 8080](/GCP-Prints/stp9.png)

---

### **10. Iniciar a VM e acessar via SSH**

Ligue a instância criada e clique no botão **SSH** para abrir o terminal.

![Acessar via SSH](/GCP-Prints/stp10.png)

---

## 🧪 Instalar o Jenkins via terminal (SSH)

Execute os seguintes comandos:

```bash
# Instalar Java
sudo apt-get install openjdk-8-jdk -y

# Adicionar chave e repositório Jenkins
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

# Atualizar pacotes e instalar Jenkins
sudo apt update
sudo apt install jenkins -y

# Iniciar e verificar o status do Jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

---

### 🔓 Liberar a porta 8080 no firewall da instância

```bash
sudo ufw allow 8080
sudo ufw enable
sudo ufw status
```

---

### 🔑 Obter senha inicial do Jenkins

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

Pronto! Após isso, você poderá acessar o Jenkins no navegador via:

```
http://<IP-externo-da-instância>:8080
```

---

### **11. Alteração das configurações do Jenkins**
Com o Jenkins acessível via IP público, foi possível configurar um webhook no GitHub para notificar o Jenkins a cada push.  
Foi utilizado o plugin **"GitHub Integration"** e configurado um pipeline declarativo para automatizar os processos de build.

![Configuração do Webhook no Jenkins](/GCP-Prints/stp11.png)

![Configuração do Webhook no GitHub](/GCP-Prints/stp12.png)

### ** ✅ Considerações Finais**

O processo de implantação do Jenkins em ambiente cloud apresentou vários desafios, como:

- Limitações de rede  
- Escolha inadequada de região e tipo de instância  
- Dependências de ambiente não previamente instaladas  

No entanto, todas as questões foram contornadas, e o sistema de CI/CD encontra-se funcional para testes e projetos de pequeno porte.

> 💡 **Recomendações para produção:**
> - Utilizar instâncias mais robustas
> - Aplicar boas práticas de segurança:
> - HTTPS
> - Autenticação com tokens
> - Regras de acesso via IAM

*Certifique-se de que a instância está em execução e que a porta 8080 está realmente liberada no firewall da Google Cloud.*
