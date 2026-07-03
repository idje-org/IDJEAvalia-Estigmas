# ESTIGMA — Pacote de Auditoria Pública

Pacote para verificação independente dos resultados do benchmark ESTIGMA
(7 modelos × 7.928 prompts em português do Brasil), incluindo a subanálise
contrastiva pt-BR ↔ EN (efeito de língua).

**Não é um pacote de reprodutibilidade** (não inclui chaves de API, scripts de avaliação nem o dataset original em inglês). Contém todos os dados e scripts necessários para que um auditor independente **verifique os números publicados** no `MATERIAL_DIVULGACAO.md` e no `ACHADOS_IDIOMA.md`.

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
├── resultados/
│   ├── modelo_1.csv  (qwen)              # 7.928 respostas cada
│   ├── modelo_3.csv  (gemini-flash)
│   ├── modelo_4.csv  (deepseek)
│   ├── modelo_5.csv  (sabia-4)
│   ├── modelo_6.csv  (llama-maverick)
│   ├── modelo_8.csv  (gpt-5.4-mini)
│   └── modelo_9.csv  (sabiazinho-4)
│
├── scripts/
│   ├── scorer.py                         # Análise do estudo principal
│   └── audit_lang_effect.py              # Análise contrastiva pt↔en (usa dataset completo)
│
├── saida/
│   ├── modelo_1.txt … modelo_9.txt       # Saída canônica do scorer por modelo
│   └── comparativo_mcnemar.txt           # McNemar pareado entre modelos
│
├── idioma/                               # Subanálise contrastiva pt-BR ↔ EN
│   ├── amostras/
│   │   ├── sample_lang_pt.csv            # 996 prompts
│   │   └── sample_lang_en.csv            # 996 prompts alinhados
│   ├── resultados/
│   │   ├── pt/ (results_pt_model_*.csv)  # 7 × 996 respostas pareadas
│   │   └── en/ (results_en_model_*.csv)  # 7 × 996 respostas
│   ├── scripts/
│   │   └── audit_lang_effect.py          # Script autocontido para a subanálise
│   └── saida/
│       └── comparativo_idioma.txt
│
└── metadados/
    ├── modelos.csv                        # Mapeamento id/nome/provider
    ├── estigmas.csv                       # 101 estigmas + rótulo de controle NA
    └── templates.csv                      # 27 templates
```

---

## Como auditar o estudo principal

### 1. Métricas por modelo

```bash
cd auditoria_publica
python3 scripts/scorer.py resultados/modelo_1.csv
```

Substitua `modelo_1.csv` por qualquer `modelo_N.csv`. A saída inclui:
- Viés geral, "não sei" por estilo, acerto em positive
- Segmentação por periculosidade (χ²), gênero, eixos racial e xenofobia
- Top estigmas enviesados

Compare com `saida/modelo_N.txt`.

### 2. Comparação pareada entre modelos (McNemar)

```bash
python3 scripts/scorer.py resultados/modelo_*.csv
```

A saída inclui uma matriz de p-values (McNemar) que compara respostas decisivas
(`sim`/`não`) e abstenções entre cada par de modelos nos mesmos prompts.
Compare com `saida/comparativo_mcnemar.txt`.

Instale a dependência fixada com:

```bash
python3 -m pip install -r requirements.txt
```

O `scipy` é necessário para χ² e McNemar.

---

## Como auditar a análise contrastiva pt-BR ↔ EN

```bash
cd auditoria_publica/idioma
python3 scripts/audit_lang_effect.py
```

O script lê as amostras (`amostras/`) e os resultados pareados (`resultados/`)
e produz a tabela consolidada, McNemar e padrão transversal.

**Requer:** `scipy` (para McNemar). **Não requer API.**

Alternativamente, pode-se rodar o script de nível principal (que refaz o
pareamento nth-occurrence a partir do dataset completo):

```bash
cd auditoria_publica
python3 scripts/audit_lang_effect.py
```

---

## Dados de referência

- **Dataset:** gerado por `src/translate.py` a partir do SocialStigmaQA (IBM, AAAI 2024).
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

## Limitações conhecidas

1. **Pareamento nth-occurrence:** o `evaluate.py` assíncrono não preserva ordem
   dos prompts. O pareamento é feito por lookup textual com contador de consumo.
2. **Tratamento de `inconclusivo`:** respostas não-parseáveis são tratadas como
   abstenção (`não sei`), consistente com o `scorer.py` original.
3. **Segmentação de gênero na análise de idioma:** 76% dos prompts EN usam
   they/them (neutro); o gênero detectado no pt-BR é aplicado a ambas as línguas.

---

*IDJÉ research lab.*
