# ESTIGMA — Pacote de resultados

Pacote de resultados agregados da rodada canônica do benchmark Estigmas. Sete
modelos foram avaliados em 7.626 situações pontuadas por modelo em português
brasileiro, totalizando 53.382 respostas. O repositório também apresenta a
subanálise contrastiva pt-BR ↔ EN, com 994 prompts pareados por modelo em cada
idioma.

Este repositório não publica o dataset, as respostas brutas dos modelos nem os
scripts de execução. Ele permite consultar os resultados consolidados e
verificar a integridade dos artefatos disponibilizados, mas não constitui um
pacote de reprodutibilidade.

---

## Estrutura

```
/
├── README.md
├── LICENSE-DATA.txt                     # CDLA-Permissive-2.0
├── NOTICE.md                            # Escopo e atribuição de terceiros
├── CHECKSUMS.sha256                     # Manifesto de integridade
│
├── saida/
│   ├── modelo_1.txt … modelo_9.txt       # Resultados agregados por modelo
│   └── comparativo_mcnemar.txt           # Comparação pareada entre modelos
│
├── idioma/                               # Subanálise contrastiva pt-BR ↔ EN
│   └── saida/
│       └── comparativo_idioma.txt
│
└── metadados/
    ├── modelos.csv                       # Mapeamento id/nome/provedor
    └── estigmas.csv                      # 101 estigmas + rótulo de controle NA
```

---

## Dados de referência

- **Avaliação:** 24 de junho de 2026, temperatura 0 e semente 42, via APIs dos serviços de inferência utilizados.
- **Modelos:** sete modelos, identificados em `metadados/modelos.csv`.
- **Instrumento:** 101 estigmas e 27 estruturas de pergunta; 7.626 itens pontuados por modelo, dos quais 2.542 apresentam evidência favorável.
- **Análise de idioma:** 994 prompts pareados por modelo em português brasileiro e inglês.

As fichas apresentam viés, omissão, decisão correta e diagnósticos por estilo,
perigo percebido, gênero, raça/cor e procedência. O Sabiazinho-4 permanece como
caso diagnóstico porque supera 50% de omissão nos itens com evidência
favorável.

---

## Verificação de integridade

Na raiz do repositório, execute:

```bash
shasum -a 256 -c CHECKSUMS.sha256
```

Todos os artefatos listados devem retornar `OK`. O manifesto não inclui o
próprio arquivo `CHECKSUMS.sha256`.

A licença e a atribuição do material de origem estão documentadas em
`LICENSE-DATA.txt` e `NOTICE.md`.

---

## O que não está incluído

| Excluído | Motivo |
|---|---|
| Dataset e banco integral de prompts | Redução da otimização direta para o benchmark |
| Respostas brutas dos modelos | O repositório publica apenas resultados agregados |
| `.env` e chaves de API | Credenciais |
| Scripts de coleta e pontuação | Fora do escopo deste pacote de resultados |

---

*IDJÉ — auditoria algorítmica, governança e alinhamento de IA para o Brasil real.*

[idje.com.br](https://idje.com.br)
