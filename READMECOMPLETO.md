# ROADMAP DE AUDITORIA WEB - Metodologia Filipe Gabriel
### SOC Analyst | Wazuh Baseline 2026 | Luanda - Angola
**Versão: 1.0 - 23/10/2026 | Uso: Com autorização assinada do cliente**

---

### FASE 0 - AUTORIZAÇÃO LEGAL (5 min - OBRIGATÓRIA)
> Sem isso, não executa nada.

- [ ] Termo de autorização assinado pelo responsável
- [ ] Definir escopo: `Ex: https://cliente.co.ao + *.cliente.co.ao`
- [ ] Definir janela: `Ex: 23/10/2026 das 19h às 22h`
- [ ] Definir IPs de origem do teste
- [ ] Contato de emergência do cliente

**Modelo de texto:** "Autorizo Filipe Gabriel a realizar testes de vulnerabilidade não intrusivos..."

### FASE 1 - RECON PASSIVO (30 min - Zero toque no alvo)
- [ ] WHOIS + DNS: `whois dominio.co.ao`, `dig +short`, `subfinder -d dominio.co.ao`
- [ ] Shodan/Censys: Buscar IP público, portas 80/443/22
- [ ] Wappalyzer / WhatWeb: Identificar PHP, WordPress, Joomla, plugins
- [ ] Cert Transparency: `crt.sh` -> subdomínios esquecidos
- [ ] Google Dorks: `site:cliente.co.ao ext:env OR ext:sql OR ext:bak`

**Output:** Lista de ativos + tecnologias + versões

### FASE 2 - SCANNING (1h - Leve)
- [ ] `nmap -sV -p 80,443 --script http-headers,http-title IP`
- [ ] `curl -I https://cliente.co.ao` -> Capturar `X-Powered-By: PHP/8.x`
- [ ] `nikto -h https://cliente.co.ao -Tuning 1` (headers e configs)
- [ ] SSL Labs: https://www.ssllabs.com/ssltest/

### FASE 3 - VULNERABILITY ASSESSMENT (2h - Seu Diferencial)
#### 3.1 - PHP2Shell CVE-2026-109618 / CVE-2026-109619 (CRITICAL 9.2)
- [ ] Verificar versão PHP < 8.4.5 ou < 8.3.12
- [ ] Verificar config: `session.upload_progress.enabled = On`
- [ ] Rodar: `./check_php2shell.sh https://cliente.co.ao`
- [ ] Validar com Wazuh Lab: Regra 100016 deve disparar

#### 3.2 - WordPress / CMS
- [ ] `wpscan --url https://cliente.co.ao --enumerate vp,ap --api-token TOKEN`
- [ ] Verificar `/wp-config.php.bak`, `/.env`, `/debug.log`, `/backup.zip`

#### 3.3 - Misconfiguration
- [ ] Headers: `X-Frame-Options`, `CSP`, `HSTS`, `X-Content-Type-Options`
- [ ] Métodos: `OPTIONS /` -> TRACE/TRACK habilitado?
- [ ] Upload: tentar enviar `shell.jpg.php` em formulário

### FASE 4 - CLASSIFICAÇÃO CVSS + MITRE
| Severidade | CVSS | Exemplo | MITRE |
| --- | --- | --- | --- |
| CRITICAL | 9.0-10 | PHP2Shell RCE, SQLi -> RCE | T1190 |
| HIGH | 7.0-8.9 | .env com senha DB, XSS Stored | T1552, T1189 |
| MEDIUM | 4.0-6.9 | Headers faltando, SSL TLS 1.0 | T1078 |
| LOW | 0.1-3.9 | Version disclosure | T1592 |

### FASE 5 - RELATÓRIO QUE FECHA CONTRATO
**Estrutura:**
1.  **Resumo Executivo (1 página):** Para o dono entender. "Encontramos 2 CRITICAL remotos, seu site pode ser invadido em 10s pela internet."
2.  **Provas Técnicas:** Prints de `curl -I`, Shodan, Wazuh 100016, wpscan
3.  **Impacto:** O que hacker faz com isso (deface, roubo de dados, ransomware)
4.  **Mitigação Imediata (24h):** 
    - `session.upload_progress.enabled = Off` + restart Apache
    - WAF Rule: Bloquear `PHP_SESSION_UPLOAD_PROGRESS`
    - Atualizar PHP 8.4.5+
5.  **Recomendação Wazuh Baseline:** Instalar 17 regras + FIM + Active Response

### FASE 6 - RE-TESTE E CERTIFICADO
- [ ] Re-executar `check_php2shell.sh` após correção
- [ ] Emitir: `CERTIFICADO DE CONFORMIDADE - Auditoria 23/10/2026 - Vulnerabilidades Mitigadas`

### FASE 7 - MONETIZAÇÃO
- [ ] Proposta: Monitoramento Wazuh 24h + Re-teste mensal
- [ ] Valor: 75.000 Kz/mês - 150.000 Kz/mês (mercado Luanda)

---
### Ferramentas do Roadmap
- Kali Linux ou Ubuntu no Hyper-V
- Wazuh Manager 4.8.0 + Agent
- Scripts: `check_php2shell.sh`, `wpscan`, `nmap`, `whatweb`
- Baseline: https://github.com/Filipegabri/Wazuh-enterprise-baseline-2026

### Legal
Testes apenas com autorização por escrito. Este roadmap é para fins de auditoria defensiva (Blue Team).
