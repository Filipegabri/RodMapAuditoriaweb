# RoadMap Auditoria Web - Metodologia 7 Fases
**Por Filipe Gabriel - SOC Analyst | Luanda, Angola**
**Foco: PHP2Shell RCE CVE-2026-109618 + Wazuh Baseline 17 Regras**

### O que é isso?
Metodologia profissional para auditar sites de clientes com autorização. Testado em lab Hyper-V (W10 + Ubuntu + Wazuh 4.8.0).

### Fases:
0. Autorização Legal
1. Recon Passivo (Shodan, Wappalyzer)
2. Scanning Leve (nmap, curl -I)
3. Vulnerability Assessment (PHP2Shell, wpscan, headers)
4. Classificação CVSS + MITRE ATT&CK
5. Relatório Executivo que fecha contrato
6. Re-teste e Certificado

### Lab Validado
- Alerta Wazuh 100016 - PHP2Shell CRITICAL disparando
- Mitigação: session.upload_progress.enabled = Off

### Contato para Auditoria
LinkedIn: linkedin.com/in/filipegabri
GitHub: github.com/Filipegabri
