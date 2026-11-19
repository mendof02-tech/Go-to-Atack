# 📋 Relatório de Execução

## Identificação

**Aluno:** Flávio Mendosa  
**Desafio:** DIO — Kali Linux + Medusa  
**Data:** ____/____/______  

---

## 1. Ambiente

| Item | Informação |
|---|---|
| Virtualizador | VirtualBox |
| Kali Linux | |
| Metasploitable 2 | |
| DVWA | |
| Rede | Host-only |
| IP Kali | |
| IP alvo | |

---

## 2. Reconhecimento

### Comando

```bash
nmap -sV IP_DO_ALVO
```

### Resultado

```text
Cole aqui o resultado relevante.
```

### Evidência

`images/01-nmap-servicos.png`

---

## 3. FTP

### Objetivo

Descrever o objetivo do teste.

### Comando utilizado

```bash
medusa -h IP_DO_ALVO -U wordlists/usuarios.txt -P wordlists/senhas.txt -M ftp
```

### Resultado

Descrever somente o que ocorreu no laboratório.

### Evidência

`images/02-medusa-ftp.png`

---

## 4. DVWA

### Security Level

____________________________

### Objetivo

____________________________

### Resultado

____________________________

### Evidências

`images/03-dvwa-login.png`

`images/04-dvwa-configuracao.png`

---

## 5. SMB / Password Spraying

### Portas

____________________________

### Estratégia

____________________________

### Resultado

____________________________

### Evidência

`images/05-smb.png`

---

## 6. Mitigações

| Risco | Mitigação | Prioridade |
|---|---|---|
| Senhas fracas | Política de senhas | Alta |
| Tentativas automatizadas | Rate limiting | Alta |
| Credencial comprometida | MFA | Alta |
| Falhas não monitoradas | Alertas/logs | Média |
| Serviços desnecessários | Hardening | Média |

---

## 7. Conclusão

Escreva aqui sua conclusão com base nos resultados efetivamente obtidos.

---

## 8. Aprendizados

- 
- 
- 
- 

---

## 9. Próximos passos

- 
- 
- 
