# Laboratório: Reconhecimento Passivo de DNS

**Disciplina:** Ethical Hacking / Segurança Ofensiva  
**Ferramentas utilizadas:** `nslookup`, `whois`, `dig`, `host`  
**Plataforma:** Kali Linux  
**Autor:** Paulo Junior       
**Data:** 15 de maio de 2026

---

# 📋 Objetivos

- Utilizar `nslookup` para obter informações de domínio e endereços IP.
- Utilizar o comando `whois` para extrair informações de registro de domínios e blocos de IP.
- Comparar a saída das ferramentas `nslookup` e `dig`.
- Executar consultas de DNS reverso.

---

# 📖 Plano de Fundo / Cenário

Antes de qualquer teste de intrusão ou atividade de hacking ético, é fundamental realizar um reconhecimento passivo para coletar discretamente o máximo de informações sobre o alvo.

Uma fonte extremamente valiosa são os dados públicos de DNS e registros de domínios disponíveis na internet.

Neste laboratório foram analisadas consultas DNS utilizando as ferramentas:

- `nslookup`
- `whois`
- `dig`
- `host`

Além disso, também foram realizadas consultas reversas (rDNS) para identificar hosts associados aos endereços IP.

---

# 💻 Recursos Necessários

- Máquina virtual Kali Linux
- Acesso à Internet
- Ferramentas:
  - nslookup
  - whois
  - dig
  - host

---

# 🔍 Parte 1 — Uso do nslookup para obter informações de domínio e IP

## Passo 1 — Inicialização

1. Iniciar o Kali Linux.
2. Abrir uma janela de terminal.

---

## Passo 2 — Capacidades do nslookup

Para acessar o manual:

```bash
man nslookup
```

### Pergunta

Que palavra-chave você usaria para consultar o registro MX do servidor de e-mail em um domínio?

### Resposta

```bash
set querytype=mx
```

ou

```bash
set type=mx
```

---

## Passo 3 — Modo interativo e consultas básicas

Entrando no modo interativo:

```bash
nslookup
```

Consultar o domínio:

```bash
> cisco.com
```

O comando resolve:

- IPv4 (registro A)
- IPv6 (registro AAAA)

---

### Localizar servidores DNS

```bash
> set type=ns
> cisco.com
```

### Pergunta

Quais são os endereços IPv4 e IPv6 do servidor DNS primário (ns1)?

### Resposta

```text
IPv4: 72.163.5.201
IPv6: 2001:420:1101:6::a
```

---

## Passo 4 — Alterar servidor DNS e utilizar type=any

Alterando para o DNS do Google:

```bash
> server 8.8.8.8
```

Consultando todos os registros:

```bash
> set type=any
> netacad.com
```

### Pergunta

Quais tipos de registros são exibidos?

### Resposta

São exibidos diversos registros, incluindo:

- A
- AAAA
- NS
- MX
- TXT

---

# 🌐 Parte 2 — Uso do whois para obter informações de registro

## Passo 1 — Comparação entre domínios

```bash
whois cisco.com
whois netacad.com
```

### Pergunta

Que conclusão podemos tirar sobre os dois domínios?

### Resposta

Ambos pertencem à Cisco e utilizam infraestrutura hospedada em ambiente de nuvem.

---

## Passo 2 — Informações de alocação de IP

Consulta WHOIS utilizando o IP do servidor DNS:

```bash
whois 72.163.5.201
```

### Pergunta

Qual é o intervalo IPv4 alocado à Cisco contendo o servidor ns1.cisco.com?

### Resposta

```text
72.163.0.0/16
```

---

### Consulta adicional

```bash
whois 64.100.0.0
```

### Pergunta

Quais intervalos adicionais foram encontrados?

### Resposta

```text
64.100.0.0/14
64.104.0.0/16
```

---

## 📌 Observação

Conhecer blocos de IP ajuda a definir:

- escopo de pentest
- footprint da organização
- ativos pertencentes à empresa

---

# ⚖️ Parte 3 — Comparação entre nslookup e dig

## Passo 1 — Consulta padrão

```bash
dig cisco.com
```

### Pergunta

Qual a diferença entre os registros padrão retornados pelo dig e pelo nslookup?

### Resposta

O `dig` consulta apenas registros A por padrão, enquanto o `nslookup` retorna registros:

- A
- AAAA

---

## Consulta IPv6

```bash
dig cisco.com AAAA
```

---

## Passo 2 — Consultas específicas

Consulta utilizando DNS público:

```bash
dig cisco.com @8.8.8.8 ns
```

Consulta ANY:

```bash
dig @8.8.8.8 netacad.com any
```

### Pergunta

Qual saída é mais legível?

### Resposta

O `dig` possui saída mais organizada, exibindo cada registro em linhas separadas.

---

# 🔄 Parte 4 — Consultas de DNS Reverso (rDNS)

## Passo 1 — Uso do dig com -x

```bash
dig -x 72.163.5.201
```

### Pergunta

Que tipo de registro é retornado?

### Resposta

Registro:

```text
PTR
```

---

## Análise adicional

Consulta:

```bash
dig -x 72.163.1.1
```

Resultado:

```text
hsrp-72-163-1-1.cisco.com
```

### Pergunta

Que tipo de dispositivo provavelmente está associado?

### Resposta

Provavelmente um gateway configurado em roteador utilizando:

```text
HSRP (Hot Standby Router Protocol)
```

---

# 🛠️ Passo 2 — Utilitário host

Consulta reversa:

```bash
host 72.163.5.201
```

Consulta direta:

```bash
host cisco.com
```

### Pergunta

Como a saída do comando host difere das demais?

### Resposta

O comando apresenta saída simplificada contendo apenas:

- nome do host
- endereço IP

Sem detalhes adicionais do servidor DNS.

---

# 🧪 Passo 3 — nslookup para DNS reverso

```bash
nslookup
```

```bash
> set type=ptr
> 72.163.5.201
```

O resultado exibe o domínio associado ao IP.

---

# 🧠 Conclusão

O reconhecimento passivo mostrou-se extremamente importante durante a etapa de Footprinting em um processo de Ethical Hacking.

Com ferramentas simples e nativas do Linux foi possível:

- Mapear servidores DNS
- Descobrir servidores de e-mail
- Identificar blocos de IP
- Descobrir possíveis protocolos ativos
- Inferir topologias de rede
- Identificar convenções internas de nomenclatura

Tudo isso sem interação invasiva com os sistemas da organização, reduzindo a chance de detecção por IDS/IPS.

---

# 📚 Conhecimentos Aplicados

- DNS
- Footprinting
- Reconhecimento Passivo
- Engenharia de Redes
- Ethical Hacking
- WHOIS
- DNS Reverso
- Enumeração

---

# 🔗 Ferramentas Utilizadas

| Ferramenta | Função |
|---|---|
| nslookup | Consultas DNS |
| whois | Informações de registro |
| dig | Consultas DNS avançadas |
| host | Resolução simplificada |

---

# 📎 Evidências

As capturas de tela e evidências práticas serão anexadas posteriormente ao documento.

---

# ⚠️ Aviso Ético

Todas as atividades descritas neste laboratório foram realizadas em ambiente educacional e controlado, com finalidade exclusivamente acadêmica e de aprendizado em segurança ofensiva.
---
