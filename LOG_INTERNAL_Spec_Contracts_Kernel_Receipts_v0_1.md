# LOGLINE — INTERNAL SPEC (Contracts, Kernel & Receipts) v0.1
**Status:** PRIVADO (NDA) • **Fonte matemática:** Doutrina do Folding • **Escopo:** gramática `.lll`, objetos do kernel e provas

> Este documento é interno. Termos e formatos aqui são considerados **chaves**. Não publicar.

---

## 0) Kernel (objetos canônicos)
- **Identity**: `logline-id://{namespace}/{entity}` (pessoa, agente, nó). Suporta delegação e papéis.
- **Span**: evento assinado e imutável, vetor de métricas anexado.
- **Contract (.lll)**: especificação executável com `workflow`, `flow`, `movement`, `vector` e `enforce`.
- **State**: projeção material dos commits.
- **Receipt**: prova assinada de um `Step` bem‑sucedido.
- **Policy**: regras executáveis (pre/transform/post).
- **Diamond**: ativo licenciado derivado de trajetórias autênticas.

Invariantes globais: determinismo de veredito; exactly‑once; dupla entrada; selective disclosure; replay fiel.

---

## 1) Gramática `.lll` (EBNF)
A gramática cobre a mínima superfície pública do contrato executável.

```
Contract      = "contract" String "{" { Stmt } "}" ;
Stmt          = WorkflowStmt | FlowStmt | MovementBlk | VectorBlk | EnforceStmt | MetaStmt ;

WorkflowStmt  = "workflow" String ;
FlowStmt      = "flow" String ;

MovementBlk   = "movement" Ident "{" { Pair } "}" ;
VectorBlk     = "vector" "{" { Metric } "}" ;
EnforceStmt   = "enforce" "using" "policy" String ;

MetaStmt      = "meta" "{" { Pair } "}" ;

Pair          = Ident ":" Value ;
Metric        = Ident Number ;

Value         = String | Number | Bool | List | Obj ;
List          = "[" [ Value { "," Value } ] "]" ;
Obj           = "{" [ Pair { "," Pair } ] "}" ;

Ident         = /[A-Za-z_][A-Za-z0-9_\.]*/ ;
String        = /"(?:[^"\\]|\\.)*"/ ;
Number        = /-?(?:0|[1-9][0-9]*)(?:\.[0-9]+)?/ ;
Bool          = "true" | "false" ;
```

### 1.1 Exemplo `.lll`
```
contract "transfer_4421" {
  workflow "geral"
  flow "banking"

  movement wallet.transfer {
    from: "wallet://A"
    to:   "wallet://B"
    amount_eur: 125.40
    memo: "settlement #4421"
  }

  vector {
    value_eur 125.40
    effort_dS 0.20
    risk 0.08
    trust 0.74
    latency_s 3.10
    recurrence 0
    impact 0.12
    entropy 0.05
  }

  enforce using policy "policy:banking.v1"
}
```

---

## 2) DSL de Policy (EBNF mínima)
```
Policy        = "policy" String "{" Scope Pre? Transform? Post? "}" ;
Scope         = "scope" "{" { Pair } "}" ;
Pre           = "pre"   "{" { Rule } "}" ;
Transform     = "transform" "{" { Action } "}" ;
Post          = "post"  "{" { Rule | Effect } "}" ;

Rule          = "when" Cond "->" Decision ;
Effect        = "effect" ":" Ident "(" [ ArgList ] ")" ;
Action        = "when" Cond "->" "do" ":" Ident "(" [ ArgList ] ")" ;

Cond          = Expr ;
Decision      = "ALLOW" | "HOLD" ":" Ident | "DENY" ":" Ident ;

ArgList       = Value { "," Value } ;
Expr          = /* expressão booleana com ==, !=, >, <, in, and, or */ ;
```

### 2.1 Exemplo de Policy
```
policy "policy:banking.v1" {
  scope { flow: "banking" }

  pre {
    when balance(from) >= amount_eur -> ALLOW
    when kyc_ok(actor) == false -> HOLD: request_kyc
    when daily_limit_ok(actor, amount_eur) == false -> HOLD: manual_review
  }

  transform {
    when currency != "EUR" -> do: fx_normalize("EUR", midrate())
    when fee_required() -> do: split_fee(0.01, "wallet://vv_fees")
  }

  post {
    effect: double_entry_balanced()
    effect: emit_receipt()
    effect: update_trust(actor, +0.01)
  }
}
```

---

## 3) JSON Schema — Span (v1)
```json
{
  "$id": "https://logline.world/schema/span.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Span",
  "type": "object",
  "required": ["id", "tenant_id", "actor", "when", "kind", "vector", "provenance", "signatures"],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "tenant_id": { "type": "string", "format": "uuid" },
    "actor": { "type": "string", "pattern": "^logline-id://"},
    "kind": { "type": "string", "enum": ["commit", "refusal", "pending", "compensation"]},
    "when": { "type": "string", "format": "date-time" },
    "duration_s": { "type": "number", "minimum": 0 },
    "movement": { "type": "object" },
    "vector": {
      "type": "object",
      "required": ["value", "risk", "lat", "trust", "recur", "impact", "entropy"],
      "properties": {
        "value": { "type": "number" },
        "risk": { "type": "number", "minimum": 0 },
        "lat": { "type": "number", "minimum": 0 },
        "trust": { "type": "number", "minimum": 0, "maximum": 1 },
        "recur": { "type": "number", "minimum": 0 },
        "impact": { "type": "number" },
        "entropy": { "type": "number", "minimum": 0 }
      },
      "additionalProperties": true
    },
    "provenance": {
      "type": "object",
      "required": ["source"],
      "properties": {
        "source": { "type": "string" },
        "ip": { "type": "string" },
        "device": { "type": "string" }
      },
      "additionalProperties": true
    },
    "idempotency_key": { "type": "string" },
    "signatures": {
      "type": "array",
      "items": { "type": "string" },
      "minItems": 1
    }
  },
  "additionalProperties": true
}
```

---

## 4) JSON Schema — Receipt (v1)
```json
{
  "$id": "https://logline.world/schema/receipt.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Receipt",
  "type": "object",
  "required": ["span_id", "when", "hash", "idempotency_key", "ledger", "signatures"],
  "properties": {
    "span_id": { "type": "string", "format": "uuid" },
    "when": { "type": "string", "format": "date-time" },
    "hash": { "type": "string", "pattern": "^sha256:[0-9a-f]{64}$" },
    "idempotency_key": { "type": "string" },
    "diff": { "type": "object" },
    "ledger": {
      "type": "array",
      "minItems": 2,
      "items": {
        "type": "object",
        "required": ["account", "amount", "currency", "direction"],
        "properties": {
          "account": { "type": "string" },
          "amount": { "type": "number" },
          "currency": { "type": "string" },
          "direction": { "type": "string", "enum": ["debit", "credit"] }
        }
      }
    },
    "attestations": {
      "type": "array",
      "items": { "type": "string" }
    },
    "signatures": {
      "type": "array",
      "items": { "type": "string" },
      "minItems": 1
    }
  },
  "additionalProperties": false
}
```

---

## 5) Enforcement — Contrato de veredito
- **Entradas:** `(policy_version, span_draft, state_snapshot)`
- **Saída:** `ALLOW | HOLD(reason) | DENY(reason) | TRANSFORM(span')`
- **Determinismo:** mesma tupla de entrada ⇒ mesmo veredito.
- **Aplicação:** somente após `ALLOW/TRANSFORM`; atômico; gera **Receipt**; reconciliação contábil; atualização de agregados.

---

## 6) Replayer & Compensações
- **Replayer** reconstrói a execução a partir dos spans e recibos; divergências geram `span(kind="compensation")` com vínculo ao original.
- **Nunca** reescrever história; somente compensar.

---

## 7) Segurança, Privacidade, Anchoring
- **Chaves**: Ed25519/ECDSA; rotação e backup verificado (KMS/HSM).  
- **Privacy by design**: minimização, purpose‑lock, redactions, consent ledger, DSR automatizado.  
- **Anchoring (opcional)**: consolidar hashes diários (Merkle root) em chain pública para timestamp auditável.

---

## 8) Exemplos de Test Vectors (resumo)
- **Idempotência**: reenvio de `idempotency_key` → nenhum novo efeito.  
- **Dupla entrada**: `Σ debits − Σ credits = 0` por moeda/escopo.  
- **Determinismo**: mesmo `(policy, state, span)` ⇒ mesmo veredito.  
- **Privacy**: recibo com redactions preserva verificabilidade do hash.

*FIM — v0.1*
