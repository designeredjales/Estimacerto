# EstimaCerto — Estimativa de esforço para bibliotecas Promob

Ferramenta para estimar o esforço de customização de bibliotecas Promob **por contagem**.
Cada tipo de trabalho que se repete vira um **processo padronizado** com tempo-padrão. Uma
estimativa passa a ser: *quantos de cada processo (ou kit) × complexidade*, mais as reservas.

## Como usar

Abra `index.html` no navegador. Não precisa de instalação, servidor nem internet. Os dados
ficam salvos no próprio navegador (localStorage).

1. **Banco de processos**: o catálogo de processos repetitivos, por categoria (Gestão,
   Modelagem 3D, Parametrização, Materiais, Produção/saídas, Comercial, Testes). Edite os
   tempos-padrão com o histórico real da equipe.
2. **Kits**: composições que sempre acontecem juntas (ex.: “Módulo aéreo padrão” = cadastro
   da caixa + 2 componentes internos + 1 frente + furação + códigos ERP + teste…). Na estimativa,
   você conta kits em vez de processos soltos.
3. **Estimativa**: some kits e processos com quantidade e nível de complexidade. O painel mostra
   as horas técnicas, as reservas (contingência + gestão), o total, o prazo em dias úteis, o
   investimento, a faixa otimista/pessimista e a distribuição por categoria.
4. **Parâmetros**: valor/hora, horas produtivas/dia, tamanho da equipe, % de contingência e de
   gestão, e os multiplicadores de complexidade (Baixa 0,75 · Normal 1 · Alta 1,5 · Crítica 2).

## Fórmula

```
horas técnicas = Σ (qtd × tempo-padrão × fator de complexidade)   [kits explodidos em processos]
total          = horas técnicas × (1 + contingência% + gestão%)
prazo (dias)   = total ÷ (horas produtivas/dia × pessoas)
investimento   = total × valor/hora
faixa          = total × fator otimista … total × fator pessimista
```

## Importar / exportar

- **Banco**: exporta e importa em `.json` (completo, com kits) ou importa `.csv` com as colunas
  `codigo;categoria;processo;unidade;horas` (vírgula ou ponto e vírgula; atualiza pelo código).
- **Estimativa**: salva e abre em `.json`, exporta CSV (abre no Excel) e imprime/gera PDF.

Os tempos-padrão que vêm no banco são **pontos de partida**. Calibre com dados reais.
