# 🔐 Cybersecurity Lab — Medusa + Kali Linux

<p align="center">
  <strong>Laboratório de Auditoria de Autenticação em Ambiente Controlado</strong><br>
  Kali Linux • Medusa • Metasploitable 2 • DVWA • Nmap
</p>

<p align="center">

![Status](https://img.shields.io/badge/status-laboratório%20didático-blue)
![Platform](https://img.shields.io/badge/platform-Kali%20Linux-black)
![Security](https://img.shields.io/badge/foco-Autenticação-red)
![DIO](https://img.shields.io/badge/DIO-Challenge-purple)
![License](https://img.shields.io/badge/license-MIT-green)

</p>

---

## 🎯 Executive Summary

Este projeto documenta um laboratório de **Cybersecurity e Ethical Hacking** desenvolvido para estudar, de forma controlada, como mecanismos de autenticação podem ser submetidos a tentativas automatizadas e quais controles podem reduzir esse risco.

O laboratório utiliza máquinas virtuais isoladas e aplicações deliberadamente vulneráveis, permitindo observar o ciclo:

**Reconhecimento → Identificação do serviço → Teste controlado → Evidência → Análise de risco → Mitigação**

> ⚠️ **Uso autorizado somente.** Todos os testes deste projeto devem ocorrer exclusivamente em ambientes próprios, VMs de laboratório ou sistemas para os quais exista autorização explícita. Não execute as técnicas contra sistemas de terceiros ou ambientes de produção.

---

## 📌 Objetivos

- Compreender ataques de força bruta em serviços de rede.
- Utilizar o **Kali Linux** para auditoria em laboratório.
- Utilizar o **Medusa** para testes automatizados de autenticação.
- Reconhecer serviços com **Nmap**.
- Estudar autenticação FTP, Web e SMB.
- Compreender o conceito de **password spraying**.
- Registrar evidências técnicas.
- Relacionar vulnerabilidades a controles de mitigação.
- Transformar o laboratório em um artefato de portfólio técnico.

---

## 🧭 Escopo do laboratório

| Item | Escopo |
|---|---|
| Sistema de auditoria | Kali Linux |
| Alvo principal | Metasploitable 2 |
| Aplicação web | DVWA |
| Virtualização | VirtualBox |
| Rede | Host-only / isolada |
| Reconhecimento | Nmap |
| Automação de autenticação | Medusa |
| Objetivo | Educacional / defensivo |

---

# 🏗️ Arquitetura

```text
                         ┌──────────────────────┐
                         │      HOST FÍSICO      │
                         │      VirtualBox      │
                         └──────────┬───────────┘
                                    │
                              HOST-ONLY NETWORK
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
             ┌──────▼──────┐                 ┌──────▼─────────┐
             │ Kali Linux  │                 │ Metasploitable │
             │             │                 │       2        │
             │ Nmap        │                 │ FTP / SMB      │
             │ Medusa      │                 │ DVWA / Web     │
             │ Browser     │                 │ Vulnerável     │
             └─────────────┘                 └────────────────┘
                    │
                    │
             Auditoria controlada
```

### Exemplo de endereçamento

> Os endereços abaixo são ilustrativos. Utilize os IPs atribuídos às suas próprias VMs.

```text
Kali Linux        192.168.56.101
Metasploitable 2  192.168.56.102
```

---

# 🔎 1. Reconhecimento

Antes de qualquer teste de autenticação, o serviço precisa ser identificado.

### Descoberta de serviços

```bash
nmap -sV 192.168.56.102
```

### Exemplo de validação de conectividade

```bash
ping -c 4 192.168.56.102
```

### Evidência

📸 Adicionar:

```text
images/01-nmap-servicos.png
```

### Insight

O reconhecimento permite compreender a superfície de ataque antes de realizar qualquer teste. Em um cenário profissional, essa etapa ajuda a evitar testes desnecessários e a direcionar a avaliação para serviços efetivamente expostos.

---

# 🔐 2. FTP — Teste de autenticação

## Objetivo

Avaliar, em ambiente controlado, como uma autenticação FTP pode responder a tentativas automatizadas.

### Wordlists

O projeto contém listas **exclusivamente didáticas**:

```text
wordlists/
├── usuarios.txt
└── senhas.txt
```

### Medusa

Exemplo de execução no ambiente de laboratório:

```bash
medusa -h 192.168.56.102 -U wordlists/usuarios.txt -P wordlists/senhas.txt -M ftp
```

### O que analisar

- resposta do serviço;
- quantidade de tentativas;
- existência de credencial de laboratório válida;
- velocidade;
- existência de bloqueio;
- comportamento diante de falhas consecutivas.

### Evidência

```text
images/02-medusa-ftp.png
```

---

# 🌐 3. DVWA — Autenticação Web

## Objetivo

Estudar o comportamento de um formulário de autenticação deliberadamente vulnerável.

O DVWA deve estar disponível **somente na rede de laboratório**.

Exemplo de acesso:

```text
http://IP-DO-DVWA/dvwa/
```

### Antes do teste

Documentar:

- URL do laboratório;
- método de autenticação;
- parâmetros envolvidos;
- mensagem de erro;
- comportamento após falha;
- nível de segurança configurado.

### Evidências

```text
images/03-dvwa-login.png
images/04-dvwa-configuracao.png
```

### Análise

O ponto central não é apenas verificar se uma senha pode ser descoberta. É compreender se a aplicação possui mecanismos como:

- rate limiting;
- CAPTCHA;
- bloqueio temporário;
- mensagens de erro controladas;
- MFA;
- monitoramento.

---

# 🖥️ 4. SMB — Password Spraying

## Conceito

Password spraying é uma estratégia diferente da força bruta tradicional.

### Força bruta

```text
Usuário A
 ├── senha 01
 ├── senha 02
 ├── senha 03
 └── senha 04
```

### Password spraying

```text
Senha 01
 ├── Usuário A
 ├── Usuário B
 ├── Usuário C
 └── Usuário D
```

A segunda abordagem busca reduzir a quantidade de tentativas consecutivas contra uma única conta.

### Identificação do SMB

```bash
nmap -sV -p 139,445 192.168.56.102
```

### Análise

No laboratório, documente:

- portas identificadas;
- usuários utilizados;
- política de autenticação;
- comportamento diante das tentativas;
- controles existentes.

> A enumeração e os testes devem permanecer limitados às VMs do laboratório.

---

# 📊 5. Matriz de riscos observados

| Cenário | Exposição | Impacto potencial | Controle prioritário |
|---|---|---|---|
| FTP | Autenticação automatizável | Acesso indevido | Bloqueio + rate limit |
| Web | Login vulnerável | Comprometimento de conta | Rate limit + MFA |
| SMB | Password spraying | Comprometimento de usuários | MFA + lockout + monitoramento |
| Senhas fracas | Alta previsibilidade | Escalada de acesso | Política de senhas |

### Modelo de avaliação

```text
Probabilidade
     ▲
 Alta│      [Password Spraying]
     │
 Méd │ [FTP]       [Web]
     │
 Baixa│
     └────────────────────────► Impacto
        Baixo      Médio     Alto
```

---

# 🛡️ 6. Mitigações

## 🔑 Política de credenciais

- senhas longas;
- senhas únicas;
- bloqueio de senhas comprometidas;
- prevenção de reutilização.

## 🚦 Rate limiting

Limitar tentativas de autenticação em uma janela de tempo.

## 🔒 Account Lockout

Aplicar bloqueio ou atraso progressivo após múltiplas falhas, considerando o risco de ataques de negação de serviço por bloqueio abusivo.

## 🛡️ MFA

Adicionar um segundo fator reduz significativamente o impacto de uma senha comprometida.

## 📈 Monitoramento

Detectar padrões como:

```text
Múltiplas falhas
       ↓
Mesmo IP
       ↓
Vários usuários
       ↓
Mesma senha / padrão
       ↓
Alerta de segurança
```

## 🌐 Redução da superfície de ataque

Serviços desnecessários não devem permanecer expostos.

---

# 🧪 7. Evidências

Organizar os registros desta forma:

```text
images/
├── 01-nmap-servicos.png
├── 02-medusa-ftp.png
├── 03-dvwa-login.png
├── 04-dvwa-configuracao.png
└── 05-smb.png
```

### Regra de publicação

Antes de enviar as evidências para o GitHub:

- ❌ não publicar senhas reais;
- ❌ não publicar tokens;
- ❌ não publicar chaves privadas;
- ❌ não publicar IPs públicos;
- ❌ não publicar dados pessoais;
- ❌ não publicar credenciais corporativas.

---

# 📋 8. Resultados do laboratório

> Esta seção deve ser preenchida com os resultados reais obtidos durante sua execução.

### FTP

| Item | Resultado |
|---|---|
| IP | __________ |
| Serviço | __________ |
| Porta | __________ |
| Tentativas | __________ |
| Resultado | __________ |
| Controle observado | __________ |

### DVWA

| Item | Resultado |
|---|---|
| URL | __________ |
| Security Level | __________ |
| Método | __________ |
| Resultado | __________ |
| Controle observado | __________ |

### SMB

| Item | Resultado |
|---|---|
| IP | __________ |
| Porta | __________ |
| Usuários | __________ |
| Estratégia | Password Spraying |
| Resultado | __________ |

---

# 📈 9. Security Lessons Learned

### 01 — Autenticação é uma superfície de ataque

Um serviço pode estar funcionando corretamente e ainda assim apresentar risco se aceitar tentativas ilimitadas.

### 02 — A senha não é uma defesa suficiente

MFA, rate limiting, detecção e controles de sessão devem complementar a autenticação.

### 03 — Reconhecimento vem antes da exploração

Conhecer os serviços disponíveis permite direcionar os testes e reduzir exposição desnecessária.

### 04 — Segurança precisa ser observável

Não basta bloquear ataques. É necessário identificar que eles ocorreram.

### 05 — O contexto importa

A mesma técnica que é apropriada em um laboratório autorizado pode ser ilegal ou prejudicial quando aplicada a sistemas sem autorização.

---

# 🧠 10. Reflexão pessoal

> O laboratório permitiu compreender, na prática, que Cybersecurity não se resume à utilização de ferramentas de ataque. O conhecimento mais importante está em conectar a técnica com o risco e com a defesa.
>
> Ao observar uma tentativa automatizada de autenticação, torna-se evidente que uma senha fraca é apenas uma parte do problema. A ausência de rate limiting, MFA, monitoramento e políticas adequadas pode transformar uma vulnerabilidade simples em um incidente de segurança.
>
> A atividade também reforçou a importância da ética e do escopo. O mesmo conhecimento utilizado para testar uma VM vulnerável deve ser aplicado de forma responsável, sempre dentro de um ambiente autorizado.

---

# 🚀 11. Possíveis evoluções

Este laboratório pode ser expandido futuramente para:

- comparação entre autenticação com e sem MFA;
- análise de logs;
- criação de regras de detecção;
- investigação de incidentes;
- comparação entre força bruta e password spraying;
- hardening de serviços;
- criação de dashboards de monitoramento;
- integração com SIEM;
- análise de indicadores de comprometimento.

---

# 📚 Referências

- [Kali Linux](https://www.kali.org/)
- [Medusa](http://www.foofus.net/jmk/medusa/medusa.html)
- [DVWA](http://www.dvwa.co.uk/)
- [Nmap](https://nmap.org/book/)
- [GitHub Documentation](https://docs.github.com/)

---

# 👤 Autor

**Flávio Mendosa**

Projeto desenvolvido como atividade prática de **Cybersecurity / Ethical Hacking**, com foco em auditoria de autenticação, reconhecimento de serviços, automação controlada e mitigação de riscos.

### Tecnologias

`Kali Linux` `Medusa` `Nmap` `Metasploitable 2` `DVWA` `VirtualBox` `GitHub`

---

## ⭐ Resultado esperado

Este repositório representa não apenas a execução de comandos, mas a capacidade de:

**Identificar → Testar → Documentar → Avaliar Risco → Mitigar → Comunicar**

