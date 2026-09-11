# Referência de Métricas

## Métricas Principais

### Gasto (Investimento)

**Definição:** Total investido em Meta Ads no período selecionado.

**Fórmula:**
```
gasto = Σ(spend de todas as campanhas)
```

**Uso:** Base para cálculo de ROI.

---

### Faturamento Bruto

**Definição:** Valor total das vendas aprovadas na Hotmart no mesmo período.

**Fonte:** Hotmart API (sales summary)

**Uso:** Base para cálculo de ROI.

---

### Resultado Operacional

**Definição:** Diferença entre faturamento e investimento.

**Fórmula:**
```
resultado_operacional = faturamento_bruto - gasto_meta_ads
```

**⚠️ NÃO é lucro líquido:** Não desconta taxas Hotmart (~10%), impostos, chargebacks, reembolsos, custo do produto.

**Uso:** Indicador rápido de rentabilidade.

---

### ROI (Return on Investment)

**Definição:** Percentual de retorno sobre o investimento.

**Fórmula:**
```
roi = ((faturamento - gasto) / gasto) × 100
```

**Exemplo:**
```
Gasto: R$ 100
Faturamento: R$ 150
ROI: ((150 - 100) / 100) × 100 = 50%
```

**Interpretação:** Para cada R$1 investido, ganhou R$0,50.

---

### ROAS (Return on Ad Spend)

**Definição:** Quantas vezes o faturamento multiplicou o investimento.

**Fórmula:**
```
roas = faturamento / gasto
```

**Exemplo:**
```
Gasto: R$ 100
Faturamento: R$ 150
ROAS: 150 / 100 = 1.5x (ou 1.5:1)
```

**Interpretação:** Para cada R$1 investido, gerou R$1,50 em receita.

**Diferença do ROI:**
- ROI considera lucro (%)
- ROAS considera receita (múltiplo)

---

### CPA (Cost per Acquisition)

**Definição:** Custo médio para adquirir um cliente.

**Fórmula:**
```
cpa = gasto / numero_de_compras
```

**Uso:** Comparar eficiência de campanhas.

---

### CPV (Cost per View/Lead)

**Definição:** Custo médio por visualização ou lead.

**Fonte:** Meta Ads (cost_per_action_type)

**Tipos:**
- CPV de conversa (WhatsApp)
- CPL de lead (formulário)
- CPA de compra

---

### CTR (Click-Through Rate)

**Definição:** Taxa de cliques por impressão.

**Fórmula:**
```
ctr = (cliques / impressoes) × 100
```

---

### CPM (Cost per Mille)

**Definição:** Custo por mil impressões.

**Fórmula:**
```
cpm = (gasto / impressoes) × 1000
```

---

## Classificação de Campanhas

| Status | ROI | Ação Recomendada |
|--------|-----|------------------|
| 🟢 SCALE | ≥ 50% | Aumentar orçamento 20% |
| 🟡 OPTIMIZE | 20-50% | Revisar criativos e segmentação |
| 🟠 MONITOR | 0-20% | Acompanhar, sem escalar |
| 🔴 PAUSE | < 0% | Pausar ou investigar |

## Comparação ROI vs Lucro Líquido

| Componente | ROI Dashboard | Lucro Líquido Real |
|------------|---------------|-------------------|
| Faturamento | ✅ Sim | ✅ Sim |
| Gasto Ads | ✅ Sim | ✅ Sim |
| Taxas Hotmart | ❌ Não | ✅ Sim (~10%) |
| Impostos | ❌ Não | ✅ Sim (varia) |
| Chargebacks | ❌ Não | ✅ Sim |
| Reembolsos | ❌ Não | ✅ Sim |
| Custo Produto | ❌ Não | ✅ Sim |

**Para lucro líquido real:**
```
lucro_liquido = faturamento - gasto_ads - taxas - impostos - chargebacks - reembolsos - custo_produto
```

## Fórmulas Completas

```javascript
// Dados de entrada
const gasto = dadosMeta.spend;
const faturamento = dadosHotmart.total_value;
const compras = dadosHotmart.total;
const impressoes = dadosMeta.impressions;
const cliques = dadosMeta.clicks;

// Cálculos
const lucro = faturamento - gasto;
const roi = (lucro / gasto) * 100;
const roas = faturamento / gasto;
const cpa = gasto / compras;
const ctr = (cliques / impressoes) * 100;
const cpm = (gasto / impressoes) * 1000;

// Classificação
let status;
if (roi >= 50) status = 'SCALE';
else if (roi >= 20) status = 'OPTIMIZE';
else if (roi >= 0) status = 'MONITOR';
else status = 'PAUSE';
```
