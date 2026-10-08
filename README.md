# EstimaCerto — Estimativa de esforço para bibliotecas Promob

Estima o esforço de customização de bibliotecas Promob Catalog/Builder **por contagem**:
cada trabalho que se repete é um processo padronizado com tempo-padrão em minutos, e a estimativa
de uma demanda é *Σ quantidade × minutos*, mais reservas.

Abra `index.html` no navegador (Chrome/Edge). Não precisa de instalação nem de internet; os dados
ficam no próprio navegador (localStorage) e podem ser exportados em JSON/CSV.

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

**Árvore da biblioteca**: mostra pastas, arquivos e grupos XML como no Editor de Módulos do Catalog,
com a contagem de módulos em cada grupo (grupos ocultos em cinza). Escolha qual elemento XML é o
“módulo” (a ferramenta sugere `<Module>` quando existe). O ＋ de cada grupo adiciona a quantidade à
estimativa (demanda “Módulos do System”), usando o processo escolhido; com retrato salvo, a árvore
mostra a diferença por grupo.

Cada métrica pode ser vinculada a um processo do banco; os vínculos ficam salvos, e existem sugestões
por pasta (ex.: Atributos → CFG-03, Materiais → MAT-02). Com **Salvar retrato** antes da customização e
uma nova leitura depois, a **diferença** mostra o que foi produzido e vira uma demanda para comparar
estimado × realizado.

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
