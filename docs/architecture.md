# Arquitetura do Dashboard ROI

## Visão Geral

Sistema de Business Intelligence que conecta Meta Ads e Hotmart para calcular ROI em tempo real.

## Diagrama de Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                    USUÁRIO (Browser)                         │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│              Dashboard HTML (Single File)                    │
│  ┌───────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │ Token Meta    │  │ Conta        │  │ Período        │  │
│  │ (manual)      │  │ (seleção)    │  │ (filtro)       │  │
│  └───────────────┘  └──────────────┘  └────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              JavaScript Frontend                       │  │
│  │  • Coleta campanhas Meta Ads                         │  │
│  │  • Consulta vendas Hotmart                           │  │
│  │  • Calcula ROI em tempo real                         │  │
│  │  • Persiste dados localmente                         │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────────┘
                     │
         ┌──────────┴──────────┐
         │                     │
         ▼                     ▼
┌─────────────────┐  ┌──────────────────────┐
│  META ADS API   │  │   HOTMART API         │
│  (direto HTTPS) │  │   (via proxy :8789)   │
└─────────────────┘  └──────────────────────┘
```

## Fluxo de Dados

### 1. Coleta Meta Ads

- Token de longa duração (60 dias)
- Graph API v21.0
- Endpoint: `/{account_id}/insights`
- Campos: spend, impressions, clicks, actions
- Paginação automática
- Rate limit handling

### 2. Coleta Hotmart

- OAuth2 client_credentials
- Proxy local para CORS
- Endpoint: `/payments/api/v1/sales/summary`
- Filtros: período, status (approved)
- Cache de token

### 3. Cálculo de ROI

```javascript
// Fórmulas implementadas
lucro_operacional = faturamento_hotmart - gasto_meta_ads
roi_percentual    = (lucro_operacional / gasto_meta_ads) * 100
roas              = faturamento_hotmart / gasto_meta_ads
cpa               = gasto_meta_ads / compras
```

### 4. Classificação

| ROI | Ação | Cor |
|-----|------|-----|
| ≥ 50% | SCALE | 🟢 |
| 20-50% | OPTIMIZE | 🟡 |
| 0-20% | MONITOR | 🟠 |
| < 0 | PAUSE | 🔴 |

## Componentes

### Frontend
- Single-file HTML (128KB)
- CSS inline (responsivo)
- JavaScript vanilla (ES6+)
- localStorage para persistência

### Backend Proxy
- Python + http.server
- Resolve CORS Hotmart
- Renovação automática de tokens
- Porta 8789

### Integrações
- Meta Ads Graph API
- Hotmart Payments API
- WhatsApp (PortoBot)

## Decisões de Design

### Por que single-file?
- Zero dependências
- Carregamento instantâneo
- Fácil deploy
- Portabilidade

### Por que proxy local?
- Hotmart não suporta CORS direto
- Credenciais protegidas no backend
- Controle total sobre autenticação

### Por que localStorage?
- Dados persistem entre sessões
- Funciona offline
- Sem dependência de backend
- Cache inteligente

## Limitações Conhecidas

1. **Atribuição:** Não sabe qual campanha gerou qual venda (cruzamento por período)
2. **Autenticação:** Token manual, sem OAuth flow completo
3. **Escalabilidade:** Proxy local, não distribuído
4. **Offline:** Requer internet para APIs

## Roadmap Técnico

- [ ] Service Worker para offline
- [ ] Docker container
- [ ] Autenticação JWT
- [ ] Rate limit otimizado
- [ ] Exportação PDF
