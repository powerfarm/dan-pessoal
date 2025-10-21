# From Prompt to Proof — The Deterministic AI Stack (One‑Pager)
**Audit-ready • Privacy by design • European‑standard processing**  
**LogLine ≠ blockchain** — Blockchain é apenas um padrão de **ancoragem opcional** para timestamp público.

---

## O problema
IA “caixa‑preta” não vira infraestrutura: custo invisível, falhas silenciosas, risco regulatório (UE). Sem prova e sem replay, não há confiança.

## A solução — Engenharia de Trajetórias (4S)
1) **Simulate** — prever efeitos **sem** tocar estado.  
2) **Screen** — aplicar políticas: **ALLOW/HOLD/DENY/TRANSFORM**.  
3) **Span** — registrar evento assinado com vetor de valor/risco/latência/confiança.  
4) **Step** — aplicar mudança **exactly‑once** e emitir **recibo contábil**.

**Resultado:** execução determinística, prova reproduzível e privacidade por construção.

## O que provamos (sem expor IP)
- **Receipts**: hash (sha256), idempotency‑key, double‑entry diff, assinatura.  
- **Replay**: reconstrução bit‑a‑bit de execuções reais.  
- **Selective disclosure**: prova sem payload (hash/commitment + redactions).

## Padrões de garantia
- Determinismo de decisão (mesmo caso ⇒ mesmo veredito).  
- Exactly‑once (idempotency‑key + ausência de duplicidade).  
- Dupla entrada (Σ débitos − Σ créditos = 0).  
- Privacy by design (minimização + purpose‑lock + consent ledger).

## Arquitetura de referência
- Fila idempotente → **Policy Engine** → **Receipt Store** → **Replayer**.  
- **Anchored receipts (opcional)**: publicar hash diário em chain pública (timestamp), mantendo dados sensíveis off‑chain.

## Métrica de valor (exemplo)
Maximizamos \(J=\sum w^\top v(s) - \lambda_1\,risk - \lambda_2\,lat + \lambda_3\,trust\) sob políticas, contabilidade e privacidade.

## Por que não é blockchain?
- LogLine é **disciplina de execução**, topologia‑agnóstica (local/federado/público).  
- Blockchains forçam consenso global e publicidade; aqui escolhemos quando ancorar **sem** expor operações.

## ROI & Compliance
- **Custo ↓** com *simulate/screen* antes de *step*.  
- **Audit pass rate ≥ 95%**.  
- Alinhado a GDPR/AI Act (princípios + registros + resposta a incidentes).

## CTA
- **Baixe o whitepaper técnico** (12–16 págs).  
- **Peça um benchmark** na sua carga real.  
- **Rodamos um checklist de conformidade em 48h.**

**Badge:** *We trust and build with LogLine.*
