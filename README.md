# EstimaCerto — Estimativa de esforço para bibliotecas Promob

Estima o esforço de customização de bibliotecas Promob Catalog/Builder **por contagem**:
cada trabalho que se repete é um processo padronizado com tempo-padrão em minutos, e a estimativa
de uma demanda é *Σ quantidade × minutos*, mais reservas.

Abra `index.html` no navegador (Chrome/Edge). Não precisa de instalação nem de internet; os dados
ficam no próprio navegador (localStorage) e podem ser exportados em JSON/CSV.

## Arquivo de estimativas (por cliente, versionado)

No topo da aba Estimativa:
- **Cliente = pasta.** “+ Cliente”, renomear, e a lista das estimativas do cliente agrupada por mês,
  com status, total, valor e data. Ações: Abrir, Renomear, **Nova versão** (v2, v3… mantendo o histórico),
  **Arquivar/Desarquivar** (“mostrar arquivadas”) e excluir.
- **Rascunho → Fechada.** “Fechar estimativa” trava a edição (a coluna Real continua editável para o
  realizado) e **só então libera Toggl e Zoho Invoice**. Para mudar: “Reabrir” ou “Nova versão”.
- **Templates de estimativa**: “Salvar como template” guarda todas as demandas (sem realizado);
  “Nova a partir de template…” cria uma estimativa nova no cliente selecionado.
- **Modelos de demanda**: “Salvar como modelo” em qualquer demanda guarda suas linhas (com detalhe e
  minutos) para “Aplicar modelo…” em outras demandas.
- **Onde fica salvo**: no link publicado em claude.ai, clientes, estimativas, banco de processos,
  modelos e templates ficam no banco de dados do artefato (recupera em qualquer aparelho). Aberto como
  arquivo local, fica no navegador.

## Estrutura

**Estimativa → Demandas → Linhas.** Uma estimativa tem várias demandas (ex.: *Prateleira deslizante*,
*Portas Mimezada*). Cada linha é `processo + detalhe/variante + qtd + min/unid + real (opcional)`.
Demandas marcadas como **Em discussão** aparecem na estimativa, mas não somam.

**Banco de processos**, classificado em **Fase › Grupo › Processo**:

| Fase | Exemplos de grupos |
|---|---|
| P&D | Construção de desenho, protótipo, reunião de definição |
| Estrutura da linha | Setup, grupos, camadas |
| Configurador | Atributos do configurador de dimensões |
| Cadastros | Entidade, montagem de entidade, Builder, revisão de modelos, materiais, composições, ferragens, referência, orçamento |
| Modulação | Montagem aplicação, modelos formatos/folhas, entidades finais, caixas, módulos, funções |
| Testes | Teste em ambiente, teste de produção |
| Publicação | Slides, validação, publicação |
| Administrativo | Administrativo da demanda |

Ferramentas do banco:
- **Editor em árvore Fase › Grupo**: “+ processo neste grupo” insere direto no grupo (o código herda o
  prefixo do grupo, ex.: ENT-09); “+ grupo”, renomear grupo e renomear fase; em cada processo:
  ✎ editar tudo, ⧉ duplicar, ↑↓ reordenar, × excluir. O editor completo tem código (renomear propaga
  para modelos, estimativa e vínculos), fase, grupo, nome, unidade, tempo, fonte, **como executar /
  passo a passo** (aparece como dica na estimativa) e reconhecimento XMind.
- **Importar XMind no banco**: lê um mapa de estimativa e mostra uma revisão. Caminhos não reconhecidos
  viram processos novos (fase, grupo, nome e tempo médio, com opção de agrupar variantes), e processos
  existentes podem ter o tempo atualizado pela média do mapa. O reconhecimento usa a coluna
  “Reconhecimento XMind” (trechos do caminho, separados por `;`), editável.
- **Em lote**, sobre os processos filtrados: ajustar tempo em % e mover para outra fase.
- **Calibrar pelo realizado**: tempo-padrão = Σ real ÷ Σ quantidade nas linhas com “Real (min)”.

Fonte de cada tempo: **Histórico** (medido no mapa XMind *Estimativa LT* da equipe) ou **Manual**
(derivado do guia Catalog Builder; precisa de calibração).

**Modelos de demanda**: pacotes que se repetem (fechamento padrão, orçamento padrão, montagem de
entidade, novo item com entidade, nova linha completa). Na demanda, “Aplicar modelo” insere as linhas.

## Importar XMind

Lê o `.xmind` direto (XMind 2020+). Convenção do mapa:
`Estimativa › Demanda › Fase › Grupo › Atividade › Variante › quantidade › minutos (ex.: 30m, 2h)`.
Cada caminho é reconhecido no banco pelos *aliases* dos processos; o que não for reconhecido entra
como linha avulsa. Demandas sem tempos viram “Em discussão”, com as anotações em observações.

## Leitor de System

Duas formas de ler a biblioteca (ex.: `D:\Setor\...\Promob Studio Start Projetto One SV1\System`):

1. **Selecionar pasta System** (Chrome/Edge): lê o conteúdo no navegador, sem enviar nada. Os editores
   do Catalog gravam XML, inclusive em extensões próprias (`.attributes`, `.category`, `.material`…).
   Todo arquivo de texto que começa com `<` é lido, e **cada elemento com ID conta como um cadastro**,
   agrupado por pasta (Atributos, Materiais, Modelos, Portas, Travessas, Regras de Orçamento,
   Estruturas, módulos).
2. **Importar lista**: dentro da pasta System, rode `tree /f /a > estrutura.txt` (ou
   `dir /s /b > lista.txt`) e importe o `.txt`. Conta arquivos por pasta e extensão, sem ler conteúdo.

**Árvore da biblioteca**: as entidades (módulos) aparecem com ID, **Abreviatura** e **Descrição**, e
podem ser exportadas em CSV. Mostra pastas, arquivos e grupos XML como no Editor de Módulos do Catalog,
com a contagem de módulos em cada grupo (grupos ocultos em cinza). Escolha qual elemento XML é o
“módulo” (a ferramenta sugere `<Module>` quando existe). O ＋ de cada grupo adiciona a quantidade à
estimativa (demanda “Módulos do System”), usando o processo escolhido; com retrato salvo, a árvore
mostra a diferença por grupo.

Cada métrica pode ser vinculada a um processo do banco; os vínculos ficam salvos, e existem sugestões
por pasta (ex.: Atributos → CFG-03, Materiais → MAT-02). Com **Salvar retrato** antes da customização e
uma nova leitura depois, a **diferença** mostra o que foi produzido e vira uma demanda para comparar
estimado × realizado.

## Toggl Track

Botão **Toggl** na estimativa. Estrutura: Cliente = projeto/cliente da estimativa › Projeto = demanda ›
Tasks = passo a passo (cada linha, numerada, com `estimated_seconds`). Gera um **script PowerShell**
(cria tudo direto no Windows) e o **prompt no padrão da skill /prompttoggl**. Tasks exigem plano Starter+.
Para fechar o ciclo, importe o CSV do relatório Detalhado do Toggl: as horas de cada task entram em
“Real (min)” da linha correspondente.

## Zoho Invoice

Botão **Orçamento Zoho Invoice**. No link publicado em claude.ai, usa o conector Zoho Invoice da conta:
escolhe cliente e item, monta as linhas (uma por demanda ou linha única; quantidade = horas com
reservas; preço = valor/hora) e cria o orçamento após confirmação. Aberto localmente, mostra as linhas
para copiar em Zoho Invoice › Novo orçamento.

## Calibração

Preencha **Real (min)** nas linhas executadas. O painel mostra o desvio estimado × real, que indica
quais tempos-padrão ajustar no banco.

## Fórmula

```
horas estimadas = Σ (qtd × min/unid) / 60          [demandas "Estimar"]
total           = horas estimadas × (1 + contingência% + gestão extra%)
prazo (dias)    = total ÷ (horas produtivas/dia × pessoas)
investimento    = total × valor/hora
```
