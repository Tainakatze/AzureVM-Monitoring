# 💻 Azure VM Monitoring

Este projeto reúne as melhores práticas, scripts e automações para implementar um sistema robusto de **monitoramento de Máquinas Virtuais no Microsoft Azure**. Ideal para profissionais de infraestrutura, devops e estudantes da certificação AZ-104.


---


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


  ---


  ## 👁️ Visão Geral:

O monitoramento eficiente de VMs no Azure garante **alta disponibilidade**, **visibilidade proativa** e **resposta automatizada** a incidentes. Este projeto oferece um guia completo para colocar isso em prática.


---


## Arquitetura da Solução:

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


---


## 🧱 Provisionamento com Bicep

Utilize um template `.bicep` para provisionar a máquina virtual **já integrada ao Log Analytics Workspace**, com monitoramento habilitado desde o início.

Exemplo de trecho em Bicep:

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2023-03-01' = {
  name: 'MinhaVM'
  location: resourceGroup().location
  properties: {
    hardwareProfile: {
      vmSize: 'Standard_B1s'
    }
    osProfile: {
      computerName: 'MinhaVM'
      adminUsername: 'azureuser'
      adminPassword: 'SenhaSegura123!'
    }
    diagnosticsProfile: {
      bootDiagnostics: {
        enabled: true
        storageUri: 'https://minhastorage.blob.core.windows.net/'
      }
    }
  }
}

```

💡 Dica: você pode adicionar extensões para conectar ao Log Analytics no próprio Bicep, evitando configurações manuais depois.


---


## 📊 Dashboard com Workbooks:

Para visualizar os dados em tempo real, utilize **Azure Workbooks** no portal do Azure.

### 📌 Passo a passo:

1. Acesse o portal do Azure
2. Vá até **Azure Monitor > Workbooks**
3. Clique em **"Importar"**
4. Selecione o arquivo `workbook-monitoramento.json` (disponível neste repositório)
5. Salve e visualize o painel interativo

### 📈 Métricas visualizadas:

- ✅ Uso de CPU por máquina virtual
- 💾 Espaço em disco disponível
- ⚠️ Eventos críticos do sistema
- 📶 Status de disponibilidade
- 🔍 Filtros por grupo de recursos e região


---


## 🛡️ Boas Práticas de Segurança:

- **Role-Based Access Control (RBAC):** restrinja permissões apenas ao necessário (Princípio do Menor Privilégio)
- **Diagnostic Settings:** não exponha dados de monitoramento publicamente
- **Log Analytics com retenção adequada:** configure políticas de retenção de logs conforme a necessidade legal e operacional
- **Criptografia:** habilite a criptografia para armazenamento de logs e diagnóstico
- **Monitoramento contínuo de alertas falsos positivos/negativos**

> 🔐 Dica: Utilize Azure Policy para garantir que todas as novas VMs venham com monitoramento e diagnósticos habilitados por padrão.


---


## 🎯 Desafios Propostos:

Se você está estudando para a certificação **AZ-104**, ou deseja expandir esse projeto, tente os seguintes desafios:

1. 📌 Criar uma política com Azure Policy que **impede a criação de VMs sem diagnóstico configurado**
2. 🧩 Automatizar a **correção de VMs não monitoradas** usando Logic Apps ou Azure Automation
3. 📊 Personalizar o dashboard do Workbook com **filtros por tags de ambiente (produção, teste, dev)**
4. 🔔 Integrar alertas do Azure Monitor com **Microsoft Teams ou Slack**
5. 🔄 Criar um script PowerShell que **exporta métricas de várias VMs para CSV**


---


## 🎓 Relacionamento com a AZ-104:

Este projeto abrange diretamente tópicos cobrados na certificação **AZ-104: Microsoft Azure Administrator**, principalmente:

- Gerenciamento de Máquinas Virtuais
- Configuração de Diagnóstico e Logs
- Implementação de Monitoramento com Azure Monitor
- Integração com Log Analytics Workspace
- Resposta a incidentes via alertas e automações

> 📘 Recomendado: revise os módulos “Monitorar recursos do Azure” e “Gerenciar identidades e governança” no Microsoft Learn.


---


## ❓ FAQ:

**➤ Preciso de licença paga para usar Log Analytics?**  
Você pode usar o nível gratuito com limitações, mas o monitoramento mais robusto requer **plano de capacidade paga (Pay-as-you-go)**.

**➤ Posso usar isso com VMs Linux e Windows?**  
Sim! O agente de diagnóstico e Log Analytics funciona com ambos, bastando aplicar a extensão certa.

**➤ Posso importar o dashboard para outro tenant?**  
Sim, desde que o workspace e as permissões estejam configurados corretamente no destino.


---


## 📚 Glossário:

- **Azure Monitor:** Serviço de monitoramento unificado do Azure
- **Log Analytics Workspace:** Armazena e consulta logs e métricas
- **Workbook:** Dashboard interativo com gráficos e consultas KQL
- **Runbook:** Script automatizado dentro do Azure Automation
- **Bicep:** Linguagem declarativa para provisionamento de recursos (IaC)


---


## 🤝 Contribuições

Pull requests são bem-vindos! Sinta-se à vontade para abrir **issues**, propor melhorias ou enviar novos dashboards e scripts úteis.


---


## Nota Final:

Este repositório foi criado com foco em aprendizado, automação e boas práticas.  
Ideal para quem deseja se aprofundar em **monitoramento no Azure**, seja para ambientes reais ou para certificações.


---


## ✅ Minhas conclusões:

O monitoramento de máquinas virtuais no Azure é mais do que uma prática técnica, é um **pilar estratégico** para garantir a estabilidade, segurança e visibilidade de qualquer ambiente de nuvem.

Ao aplicar ferramentas como **Azure Monitor**, **Log Analytics**, **Workbooks** e automações via **PowerShell**, **Bash** ou **Bicep**, você está indo além do básico e adotando uma postura **proativa** e **inteligente** de gestão.

Este repositório serve como um ponto de partida sólido para projetos reais e como reforço essencial para quem está estudando para a certificação **AZ-104**.

🚀 Monitore com consciência. Automatize com propósito. Evolua com consistência!

