# 💻 Azure VM Monitoring

Este projeto reúne as melhores práticas, scripts e automações para implementar um sistema robusto de **monitoramento de Máquinas Virtuais no Microsoft Azure**. Ideal para profissionais de infraestrutura, devops e estudantes da certificação AZ-104.

![Arquitetura de Monitoramento no Azure](./imagens/arquitetura-monitoramento-vm.png)

## 📑 Índice:

- [Visão Geral](#visão-geral)
- [Arquitetura da Solução](#arquitetura-da-solução)
- [Pré-requisitos](#pré-requisitos)
- [Passo a Passo](#passo-a-passo)
- [Automação com PowerShell e Bash](#automação-com-powershell-e-bash)
- [Provisionamento com Bicep](#provisionamento-com-bicep)
- [Dashboard com Workbooks](#dashboard-com-workbooks)
- [Boas Práticas de Segurança](#boas-práticas-de-segurança)
- [Desafios Propostos](#desafios-propostos)
- [Relacionamento com a AZ-104](#relacionamento-com-a-az-104)
- [FAQ](#faq)
- [Glossário](#glossário)

  ## 👁️ Visão Geral:

O monitoramento eficiente de VMs no Azure garante **alta disponibilidade**, **visibilidade proativa** e **resposta automatizada** a incidentes. Este projeto oferece um guia completo para colocar isso em prática.

---

## 🧱 Arquitetura da Solução:

- Azure Monitor + Log Analytics
- Alertas automáticos com Azure Automation
- Dashboard visual com Azure Workbooks
- Scripts em PowerShell e Bash para resposta rápida

---

## ⚙️ Pré-requisitos:

- Conta Azure com permissão para Monitoramento e Automation
- Azure CLI ou PowerShell instalados
- Log Analytics Workspace provisionado

---

## Passo a Passo:

1. **Habilite diagnósticos nas VMs**
2. **Associe ao Log Analytics Workspace**
3. **Implemente alertas com runbooks**
4. **Importe o dashboard JSON para o Workbooks**
5. **Teste as respostas automáticas com scripts**

---

## 💡 Automação com PowerShell e Bash:

Exemplo: Reinício automático da VM via Runbook PowerShell:

```powershell
Restart-AzVM -Name "MinhaVM" -ResourceGroupName "MeuGrupo"

az monitor diagnostic-settings create \
  --name "MonitoramentoVM" \
  --resource "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MeuRG/providers/Microsoft.Compute/virtualMachines/MinhaVM" \
  --workspace "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MeuRG/providers/Microsoft.OperationalInsights/workspaces/MeuWorkspace" \
  --metrics '[{"category": "AllMetrics", "enabled": true}]' \
  --logs '[{"category": "Administrative", "enabled": true}]'


