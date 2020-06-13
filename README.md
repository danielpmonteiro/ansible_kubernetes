# Instalando Kubernetes + Docker + Helm via Ansible

## Objetivo do Projeto

Provisionar um Cluster Kubernetes em ambiente `Cloud/On Premise` para fazer deploy de apps usando o Ansible e usando o Helm

### Motivação

Exercício do Curso da LinuxTips: [Descomplicando o Ansible](https://www.linuxtips.io/product-page/treinamento-descomplicando-o-ansible)

## Pré-Requisitos

1. Instalar o **Ansible** na máquina em que irá executar os playbooks
2. Ter configurado a **Chave RSA** para comunicação com os Servidores
3. Ter as [regras de firewall](https://github.com/badtuxx/DescomplicandoKubernetes/blob/master/day-1/DescomplicandoKubernetes-Day1.md#portas-que-devemos-nos-preocupar) necessárias para comunicação entre os servidores

### Instalando o Projeto

Após ter o Ansible instalado e a comunicação com os servidores, faça o clone desse repositório

```bash
git clone https://github.com/danielpmonteiro/ansible_kubernetes.git
```

### Executando os Playbooks

O projeto está divido em módulos, e o Cluster foi criado utilizando `Um Servidor Master` e `Dois Servidores Workers`

É necessário modificar as informações dos servidores no arquivo `Hosts` de cada módulo

1. Provisiona os Pacotes e as dependências necessárias para Criar o Cluster
    - Instala o Docker
    - Instala o Kubernetes `[ kubeadm, kubelet, kubectl ]`
    - Instala o Helm

    ```bash
    cd provisioning_packages
    ansible-playbooks -i hosts main.yml
    ```

2. Provisiona o Cluster
    - Cria o Cluster através do Node Master
    - Adiciona os Nodes Workers no Cluster

    ```bash
    cd provisioning_cluster
    ansible-playbooks -i hosts main.yml
    ```

3. Deploy das Apps
    - Deploy do Prometheus através do Helm
    - Deploy da app-v1 através do próprio Playbook do Ansible

    ```bash
    cd deploy_apps
    ansible-playbooks -i hosts main.yml
    ```

4. Atualizar Versão da App
    - Executa o Scale Down da app-v1
    - Execute o upgrade da versao para app-v2
    - Remove a app-v1

    ```bash
    cd update_deploy_apps
    ansible-playbooks -i hosts main.yml
    ```

5. Destrói o Ambiente
    - Executa a remoção do cluster
    - Executa a limpeza do servidor removendo os pacotes utilizados
    - Execute a limpeza dos arquvios e diretórios publicados

    ```bash
    cd destroy_cluster
    ansible-playbooks -i hosts main.yml
    ```

### Ferramentas Utilizadas

- Ansible - Para Provisionamento do Ambiente
- Git - Controle de Versão do Projeto
- Sistema Operacional: Ubuntu Server 18.04

### Agradecimentos

- LinuxTips
