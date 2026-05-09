# Laboratório de Força Bruta com Kali Linux e Medusa

## Descrição

Este projeto foi desenvolvido como parte do desafio prático do bootcamp de Cibersegurança da DIO.

O objetivo do laboratório é demonstrar ataques de força bruta em ambiente controlado utilizando Kali Linux, Medusa, Metasploitable 2 e DVWA (Damn Vulnerable Web Application), permitindo compreender vulnerabilidades relacionadas a autenticação insegura e aplicar medidas de mitigação.

⚠️ Todos os testes foram realizados exclusivamente em ambiente virtual e controlado para fins educacionais.

---

# Objetivos

- Compreender ataques de força bruta em diferentes serviços;
- Utilizar o Kali Linux para auditoria de segurança;
- Utilizar a ferramenta Medusa para automação de tentativas de login;
- Simular ataques em FTP, aplicações web e SMB;
- Documentar processos técnicos e resultados obtidos;
- Aplicar recomendações de mitigação e boas práticas de segurança.

---

# Tecnologias Utilizadas

- Kali Linux
- Metasploitable 2
- DVWA
- VirtualBox
- Medusa
- Hydra
- Enum4linux

---

# Estrutura do Laboratório

| Máquina | Função |
|---|---|
| Kali Linux | Máquina atacante |
| Metasploitable 2 | Máquina vulnerável |

---

# Configuração do Ambiente

## VirtualBox

As máquinas virtuais foram configuradas utilizando rede do tipo:

```bash
Host-Only Adapter
```

Essa configuração permite comunicação entre as VMs sem exposição à internet pública.

---

## Endereçamento IP

| Máquina | IP |
|---|---|
| Kali Linux | 192.168.56.10 |
| Metasploitable 2 | 192.168.56.11 |

---

# Teste de Conectividade

Foi realizado teste de conectividade utilizando o comando:

```bash
ping 192.168.56.11
```

Resultado esperado:

```bash
64 bytes from 192.168.56.11
```

---

# Cenário 1 — Ataque de Força Bruta em FTP

## Objetivo

Realizar ataque de força bruta contra o serviço FTP da máquina Metasploitable 2 utilizando o Medusa.

---

## Wordlist Utilizada

Arquivo:

```bash
wordlist.txt
```

Conteúdo:

```txt
123456
admin
password
root
msfadmin
```

---

## Comando Utilizado

```bash
medusa -h 192.168.56.11 -u msfadmin -P wordlist.txt -M ftp
```

---

## Resultado

O Medusa conseguiu identificar as credenciais válidas do serviço FTP:

```bash
Usuário: msfadmin
Senha: msfadmin
```

---

## Impacto

Ataques de força bruta podem comprometer serviços expostos utilizando senhas fracas ou previsíveis.

---

## Mitigações

- Utilização de senhas fortes;
- Implementação de MFA;
- Bloqueio após múltiplas tentativas;
- Desativação de serviços inseguros;
- Utilização de SFTP ao invés de FTP.

---

# Cenário 2 — Ataque em Aplicação Web (DVWA)

## Objetivo

Simular ataque de força bruta contra formulário web de autenticação utilizando o DVWA.

---

## Acesso ao DVWA

```bash
http://192.168.56.11/dvwa
```

---

## Configuração do DVWA

O nível de segurança da aplicação foi configurado para:

```bash
LOW
```

---

## Ferramenta Utilizada

Hydra

---

## Comando Utilizado

```bash
hydra -l admin -P wordlist.txt 192.168.56.11 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
```

---

## Resultado

O Hydra conseguiu identificar credenciais válidas para autenticação no DVWA.

---

## Impacto

Aplicações web sem mecanismos de proteção contra múltiplas tentativas de login podem ser comprometidas facilmente.

---

## Mitigações

- CAPTCHA;
- Rate limiting;
- MFA;
- Monitoramento de acessos;
- Política forte de senhas;
- Bloqueio temporário de contas.

---

# Cenário 3 — Password Spraying em SMB

## Objetivo

Simular técnica de password spraying em SMB utilizando enumeração de usuários.

---

## Enumeração de Usuários

Ferramenta utilizada:

```bash
enum4linux
```

Comando:

```bash
enum4linux 192.168.56.11
```

---

## Ataque SMB com Medusa

Arquivo de usuários:

```bash
users.txt
```

Exemplo:

```txt
root
admin
user
msfadmin
```

---

## Comando Utilizado

```bash
medusa -h 192.168.56.11 -U users.txt -p password -M smbnt
```

---

## Resultado

Foi possível realizar tentativas automatizadas de autenticação SMB utilizando uma senha comum para múltiplos usuários.

---

## Impacto

O password spraying pode evitar bloqueios de conta e explorar senhas corporativas fracas utilizadas por diversos usuários.

---

## Mitigações

- MFA;
- Bloqueio inteligente;
- Monitoramento de autenticação;
- Políticas fortes de senha;
- Desativação de SMBv1;
- Auditoria de acessos.

---

# Estrutura do Repositório

```bash
projeto-ciberseguranca/
│
├── README.md
├── wordlists/
│   └── wordlist.txt
├── images/
│   ├── ftp.png
│   ├── dvwa.png
│   └── smb.png
└── scripts/
```

---

# Evidências

As capturas de tela dos testes realizados podem ser encontradas na pasta:

```bash
/images
```

---

# Conclusão

Este laboratório permitiu compreender técnicas de força bruta em diferentes serviços e aplicações, além de demonstrar a importância de políticas seguras de autenticação.

Durante os testes foi possível observar como senhas fracas podem comprometer rapidamente ambientes vulneráveis e como medidas simples de mitigação podem reduzir significativamente os riscos de ataque.

O projeto também contribuiu para o aprendizado prático de ferramentas utilizadas em auditoria de segurança e testes de invasão em ambientes controlados.

---

# Referências

- Kali Linux
- Medusa
- Metasploitable 2
- DVWA
- OWASP
- Hydra
- Enum4linux

---

# Aviso Legal

Este projeto foi desenvolvido exclusivamente para fins educacionais em ambiente controlado.

Nenhum teste foi realizado contra sistemas reais ou sem autorização.
