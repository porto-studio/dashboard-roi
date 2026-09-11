# Considerações de Segurança

## Visão Geral

Este projeto lida com credenciais de APIs e dados financeiros sensíveis.

## Riscos Identificados

| Risco | Severidade | Mitigação |
|-------|------------|-----------|
| Tokens expostos | 🔴 Crítico | `.env` + `.gitignore` |
| Dados reais em screenshots | 🔴 Crítico | Anonimizar antes de publicar |
| Histórico Git | 🟡 Alto | Novo repositório (sem filter-branch) |
| Proxy sem auth | 🟡 Médio | Apenas localhost |
| Dados em localStorage | 🟢 Baixo | Não persistir tokens |

## Práticas Implementadas

✅ **Tokens em `.env` (nunca no código)**
✅ **`.gitignore` completo**
✅ **Proxy local para CORS**
✅ **Renovação automática de tokens**
✅ **Screenshots anonimizadas**

## Para Uso em Produção

1. **Autenticação:** Implemente login JWT
2. **HTTPS:** Use TLS em produção
3. **Rate limiting:** Proteja APIs
4. **Validação:** Valide tokens em backend
5. **Logging:** Logs estruturados (sem dados sensíveis)
6. **Rotação:** Rotacione credenciais periodicamente

## Se Credenciais Vazarem

1. Revogue imediatamente
2. Gere novos tokens
3. Atualize `.env`
4. Monitore logs
5. Considere histórico Git comprometido

## Checklist Pré-Publicação

- [ ] Tokens removidos
- [ ] IDs anonimizados
- [ ] Screenshots limpas
- [ ] `.env` ignorado
- [ ] Novo repositório Git
- [ ] Revisão manual

## Responsabilidade

O usuário é responsável por:
- Proteger credenciais
- Usar em ambiente seguro
- Cumprir termos das APIs
- Respeitar LGPD/GDPR

**Nota:** Esta é uma versão demonstrativa. Não inclui todas as camadas de segurança de um sistema de produção enterprise.
