# EstimaCerto — Estimativa de esforço por contagem (Promob, projeto, produção, montagem, consultoria)

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

## Busca com cadastro rápido

Em cada demanda, “Processo” e “Aplicar modelo” são campos de busca (código ou nome; setas + Enter).
Se o texto não existir no banco, a busca oferece **＋ Cadastrar novo processo** (abre o editor já com o
nome; ao salvar, o processo entra no grupo escolhido e a linha é adicionada à demanda) ou **linha
avulsa**; para modelos, **＋ Criar modelo com as linhas desta demanda** ou **modelo vazio**.

Na aba **Modelos**: busca de modelos (por nome, código ou processo contido) com **＋ Criar modelo** quando
não existe, e em cada modelo a busca de processo (Enter adiciona com a quantidade ao lado) com
**＋ Cadastrar novo processo**, que entra no banco e no modelo ao salvar.

## Status e demandas ignoradas

Cada demanda tem um **status de andamento** (Não iniciada · Em andamento · Aguardando cliente ·
Bloqueada · Concluída), editável mesmo com a estimativa fechada; o painel mostra o andamento em % das
horas e o Arquivo mostra o % concluído. **Ignorar** tira a demanda dos totais, do Toggl e do Zoho
sem excluí-la; as ignoradas ficam ocultas, com “Mostrar ignoradas” e “Reativar”.

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

**Arquivos `.group` do Promob**: itens como `<EXPLORERITEM TYPE="group" ID="BAN" DEF="Banheiros.group"/>`
são ligados ao arquivo apontado em `DEF`, montando a árvore como no Editor de Módulos; o `TYPE`
distingue grupo de módulo (escolha `EXPLORERITEM[module]` em “Contar como módulo”).

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

## Calibração, margem e risco (Onda 1)

- **Aba Calibração**: junta todas as estimativas com “Real (min)” e mostra, por processo, amostras
  (linhas e estimativas), tempo-padrão, real por unidade, desvio, variação e confiança
  (alta: ≥5 linhas em ≥3 estimativas e variação ≤35%). Aplica os tempos marcados ao banco. Também mostra
  o estouro observado por nível de risco e sugere a reserva.
- **Perfis de custo e preço** (Parâmetros): cada demanda usa um perfil; o preço/hora gera o valor e o
  custo/hora interno gera a margem. Painel mostra custo, margem, alerta abaixo da margem-alvo (com o
  preço mínimo) e margem real nas linhas com realizado. Zoho usa o preço do perfil de cada demanda.
- **Risco por demanda**: níveis com reserva própria (Padrão 10%, Novo fabricante 20%, P&D 35%,
  editáveis), no lugar da contingência única.
- **Backup completo** (Parâmetros): baixa e restaura tudo; a restauração mescla estimativas (fica a mais
  recente) e pergunta antes de substituir o banco.
- **Lixeira** (Parâmetros): processos, modelos, demandas e estimativas excluídos ficam 60 dias, com
  restaurar e excluir definitivamente.

## Proposta, cronograma, Zoho Projects e IA (Onda 2)

- **Proposta premium** (botão na estimativa): capa, contexto, objetivo, método (fases usadas),
  escopo e entregáveis por demanda, cronograma, investimento, condições, premissas, próximos passos e
  fechamento com a frase da empresa. Textos editáveis por estimativa, pré-visualização ao lado,
  download em .html (abrir e “Salvar como PDF”). Em rascunho sai com a marca RASCUNHO.
  Dados da empresa em Parâmetros.
- **Cronograma** (aba): início previsto e prazo do cliente na estimativa; cada demanda por uma pessoa,
  fase após fase, com horas com reserva, dias úteis, horas/dia e feriados. Gantt por fase, entrega
  prevista (também no painel), alerta de prazo com a equipe necessária, e **carga semanal** somando as
  estimativas fechadas × capacidade.
- **Zoho Projects** (estimativa fechada, link publicado): cria projeto (datas do cronograma), uma lista
  de tarefas por demanda e uma tarefa por passo, com horas estimadas e o “como executar” do banco.
- **Assistente IA** (link publicado): “Montar demandas” a partir de um briefing colado, usando o
  catálogo do banco (com revisão antes de entrar), e “Revisar estimativa aberta” (etapas faltando,
  tempos fora do padrão, riscos de escopo).

## Catálogos, comparação de versões, aprovação e benchmark (Onda 3)

- **Catálogos de processos**: Customização Promob, Projeto de ambientes, Produção fabril, Montagem e
  instalação e Consultoria Diâmetro (os novos vêm com processos de partida, fonte Manual). Cada
  estimativa escolhe o catálogo; busca, Banco, Modelos, Assistente IA e textos da proposta seguem o
  catálogo. Crie, renomeie e exclua catálogos em Parâmetros.
- **Comparação de versões da biblioteca** (Leitor de System): o retrato guarda a assinatura de cada item
  (`HASH` do Promob ou impressão dos atributos). Na leitura seguinte: novos, alterados e removidos por
  grupo, relatório de entrega (CSV) e demanda “Entregue” para comparar com o estimado.
- **Aprovação do cliente**: a proposta ganha a seção Aceite, com botões “Aprovar pelo WhatsApp” e
  “Aprovar por e-mail” (dados em Parâmetros) e linhas de assinatura no PDF. Na estimativa fechada,
  “Registrar aprovação” guarda quem aprovou, quando e por onde; o Arquivo mostra “aprovada”.
- **Benchmark da rede** (Calibração): exporta seus tempos-padrão em arquivo anônimo (processos,
  unidades, tempos e amostras, sem clientes nem valores) e compara com os arquivos de outras empresas.

## Fórmula

```
horas estimadas = Σ (qtd × min/unid) / 60          [demandas "Estimar"]
total           = Σ por demanda: horas × (1 + reserva do nível de risco% + gestão extra%)
valor / custo   = Σ por demanda: horas com reserva × preço/hora | custo/hora do perfil
prazo (dias)    = total ÷ (horas produtivas/dia × pessoas)
```

## Cronograma ajustável

- **↑ ↓** reordenam as demandas na fila; **✎** (ou clique na barra) abre o ajuste da demanda.
- Por demanda: **início fixo**, **entrega fixa** (estica ou comprime as barras), **esperar antes** (dias úteis) e **pessoa** (com equipe maior que 1).
- Por fase: **incluir/pular**, **horas no cronograma** e **espera depois** (validação do cliente, material, fila de fábrica), mostrada como barra hachurada.
- Os ajustes valem para o cronograma, a carga semanal, a proposta, o escopo e o Zoho Projects. Horas e valores da estimativa não mudam. **Voltar ao automático** limpa tudo.

## Escopo e briefing

- **Abrangência:** a estimativa inteira ou uma demanda só, com briefing e critérios de aceite próprios.
- **Nível de detalhe:** atalhos (demanda + valor; + horas; + descrição; descrição + horas com só o valor total; completo) e controles finos: descrição, passo a passo, datas, horas e valor (por demanda, só total ou ocultar).
- **Blocos:** objetivo, briefing, escopo, entregáveis, fora do escopo, premissas, responsabilidades do cliente, cronograma, investimento, critérios de aceite, aceite, além de blocos de texto próprios. Você liga, desliga, renomeia, edita e reordena cada um. **Salvar como padrão** vale para as próximas estimativas.
- **Aceite com assinatura eletrônica simples:**
  - O .html baixado abre no celular do cliente. Ele preenche nome e CPF/CNPJ, assina com o dedo e baixa o documento assinado.
  - **Importar escopo assinado** confere os códigos de integridade, recusa arquivo adulterado e registra o aceite. Quando o escopo é da estimativa inteira, registra também a aprovação.
  - **Coletar assinatura aqui** serve para a assinatura presencial.
  - Cada aceite mostra **confere** ou **mudou depois**, se o conteúdo mudar após a assinatura. Nova versão não herda aceites nem aprovação.

## Perfis com decisão automática de preço (faixas de horas)

- Cada perfil é uma **pessoa da equipe** ou um **centro de custo** (◆ na lista da demanda).
- **Preço por faixa de horas…** define faixas (ex.: até 50 h → R$ 150/h; até 100 h → R$ 130/h; acima → R$ 110/h). O sistema escolhe a faixa sozinho.
- **Base da decisão:** horas da estimativa inteira (padrão), horas da demanda ou horas deste perfil na estimativa. As horas já incluem as reservas de risco. Demandas ignoradas ou em discussão não contam.
- **Cálculo:**
  - **Faixa única:** todas as horas no valor da faixa atingida.
  - **Escalonado:** cada faixa de horas no seu valor, como no IR.
- **Proteção de degrau** (faixa única, ligada por padrão): o total nunca cai quando a estimativa passa de faixa. O editor mostra cada degrau, a margem de cada faixa contra a margem-alvo e um simulador de horas.
- A estimativa mostra a faixa aplicada e quantas horas faltam para a próxima. Proposta, escopo e Zoho Invoice usam o valor/hora efetivo.
