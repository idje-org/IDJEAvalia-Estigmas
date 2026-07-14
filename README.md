# ESTIGMA — Pacote de Resultados

Pacote para verificação independente dos resultados do benchmark ESTIGMABR
(7 modelos × 7.928 prompts em português do Brasil), incluindo a subanálise
contrastiva pt-BR ↔ EN (efeito de língua).

**Não é um pacote de reprodutibilidade** (não inclui chaves de API, scripts de avaliação nem o dataset original). 

---

## Estrutura

```
auditoria_publica/
├── LEIAME.md
├── README.md
├── LICENSE-DATA.txt                     # CDLA-Permissive-2.0
├── NOTICE.md                            # Escopo e atribuição de terceiros
├── CHECKSUMS.sha256                     # Manifesto de integridade
├── requirements.txt                     # Dependência Python fixada
│
├── dataset/
│   └── full_dataset_yes_no.csv          # 7.928 prompts pt-BR (101 estigmas, 27 templates)
│
├── saida/
│   ├── modelo_1.txt … modelo_9.txt       # Saída canônica do scorer por modelo
│   └── comparativo_mcnemar.txt           # McNemar pareado entre modelos
│
├── idioma/                               # Subanálise contrastiva pt-BR ↔ EN
│   └── saida/
│       └── comparativo_idioma.txt
│
└── metadados/
    ├── modelos.csv                        # Mapeamento id/nome/provider
    ├── estigmas.csv                       # 101 estigmas + rótulo de controle NA
    └── templates.csv                      # 27 templates
```

---

- **Avaliação:** 24 de junho de 2026, temperatura 0, seed 42, via APIs dos serviços de inferência utilizados.
- **Modelos:** 7 (Llama Scout excluído por instabilidade de formato de resposta).
- **Estigmas:** 101 estigmas + 1 controle NA = 102 entradas, 27 templates.

---

## Verificação de integridade

Na raiz do pacote, execute:

```bash
shasum -a 256 -c CHECKSUMS.sha256
```

Todos os artefatos listados devem retornar `OK`. O manifesto não inclui o
próprio arquivo `CHECKSUMS.sha256`.

A licença e a atribuição de terceiros estão documentadas em
`LICENSE-DATA.txt` e `NOTICE.md`.

---

## O que NÃO está incluído

| Excluído | Motivo |
|---|---|
| `.env` / chaves de API | Credenciais |
| `evaluate.py` / `eval_en.py` | Requer API; auditoria é offline |
| `translate.py` / `build_*.py` | Geração do dataset; escopo diferente |
| `data/raw/` | Entradas somente-leitura do benchmark original |
| `paper/MATERIAL_DIVULGACAO.md` | Produto final, não artefato de auditoria |
| Resultados do modelo 7 (Llama Scout) | Excluído do estudo |

---
*IDJÉ Auditoria algorítmica, governança e alinhamento de IA para o Brasil real.*
