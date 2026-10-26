---
name: bellabar-dre-painel-indicadores
description: Subskill da bellabar-dre-analysis. Monta o Painel de Indicadores do Bellabar a partir do DRE (imagem, print ou planilha) com 8 indicadores fixos: CMV, CMV Comidas, CMV Bebidas, Couvert Artístico, Comissão, CMO + CMO Eventual, Hortifruti e Anúncios, em R$ e em % com semáforo de meta e comparação com o mês anterior, mais o bloco Não Operacional e o gráfico de faturamento diário. Use quando o Richard pedir "painel de indicadores", "painel do DRE", "indicadores do mês", "como tá o CMV de comida/bebida", "quanto foi o couvert", "quanto gastei de hortifruti", "CMO do mês", ou mandar um DRE querendo só os indicadores-chave em vez da análise completa.
---

# Painel de Indicadores do DRE — Bellabar

Derivada de `bellabar-dre-analysis`. Usa a mesma leitura do DRE (estrutura, regime de competência, histórico), mas a entrega é um **painel enxuto de 8 indicadores**. Para análise completa (ponto de equilíbrio, reserva de caixa, despesas fixas, dívida), usar a skill-mãe.

## Os 8 indicadores

| # | Indicador | De onde sai no DRE | Base do % |
|---|-----------|--------------------|-----------|
| 1 | **CMV** (total) | Custos Variáveis → CMV (Compra de Matéria Prima + Fornecedores de Venda Direta + Apoio) | Receita Líquida |
| 2 | **CMV Comidas** | Parte do CMV de insumos de cozinha (matéria prima, proteínas, mercearia, hortifruti) | Receita de Comidas* |
| 3 | **CMV Bebidas** | Parte do CMV de bebidas (AMBEV, Coca-Cola, Heineken, destilados, insumos de bar) | Receita de Bebidas* |
| 4 | **Couvert Artístico** | Deduções → Couvert Artístico | Receita Bruta |
| 5 | **Comissão** | Deduções → Serviço 10% + Comissão Sócio (mostrar as duas linhas e o total). O 10% é dividido 2/3 equipe (Serviço 10%) e 1/3 sócio (Comissão Sócio): avaliar sempre pelo total | Receita Bruta |
| 6 | **CMO + CMO Eventual** | Despesas com Pessoal (salários + encargos: VT, Cesta, INSS, FGTS, 13º) + Custo de Mão de Obra Eventual (extras/freelas) | Receita Líquida |
| 7 | **Hortifruti** | Compras de hortifruti (linha própria no DRE ou soma das notas de hortifruti do mês) | Receita Líquida e % do CMV Comidas |
| 8 | **Anúncios** | Despesas com Marketing → Anúncios | Receita Líquida; mostrar também quanto de faturamento por R$ 1 de anúncio (Receita Bruta ÷ Anúncios) |

\* Se o DRE não separar receita de comidas e bebidas, pedir ao Richard o relatório de vendas por grupo do PDV (mesmo usado na `bellabar-vendas-analise`). Sem ele, calcular sobre a Receita Líquida e **avisar** que o % fica subestimado.

Se o DRE não abrir CMV Comidas / Bebidas / Hortifruti, perguntar **uma vez** se ele quer mandar as notas/compras do mês (a `controle-custos-insumos-bellabar` já classifica as notas) — não inventar a divisão.

## Bloco Não Operacional

Sempre que o DRE trouxer Despesas Não Operacionais, mostrar:
- Resultado Operacional, total de Despesas Não Operacionais e Resultado Não Operacional (R$ e % da RL).
- Cada item (Empréstimo, Juros, Ação Trabalhista, Pró-labore 1 e 2, Parcelamentos) em R$ e % da RL, do maior para o menor.
- Se a fórmula do DRE somar de volta o aluguel na linha "(-) Aluguel", avisar e mostrar também o resultado sem esse ajuste (Resultado Operacional − Despesas Não Operacionais).

## Faturamento diário

Quando o Richard mandar o faturamento dia a dia (lista de valores em sequência, um por dia a partir do dia 1):
- Atribuir as datas em ordem (1º valor = dia 1 do mês do DRE).
- Somar e comparar com a Receita Bruta do DRE. Se bater, dizer; se não, mostrar a diferença.
- Gráfico de barras simples, uma barra por dia: dias acima da média em destaque, demais em cor neutra, linha tracejada da média dos dias com venda, e uma linha com pontos sobreposta às barras ligando o valor de cada dia (dias com R$ 0,00 contam como fechados e ficam fora da média).
- Legenda curta acima do gráfico: total, média por dia aberto, nº de dias abertos e melhor dia.
- No painel HTML, o gráfico fica logo abaixo do cabeçalho, antes dos cartões.

## Metas (semáforo)

Confirmar as metas vigentes em `/areas/bellabar-operations-system.md` (GASS). Referência se não houver valor lá:

| Indicador | Verde | Amarelo | Vermelho |
|-----------|-------|---------|----------|
| CMV total | < 32% | 32–35% | > 35% |
| CMV Comidas | < 33% | 33–36% | > 36% |
| CMV Bebidas | < 30% | 30–35% | > 35% |
| Couvert Artístico | repasse ≈ cobrado | — | pago > arrecadado |
| Comissão (Serviço + Sócio) | ≥ 8% da RB | < 8% (checar lançamento) | — |
| CMO + CMO Eventual | < 35% | 35–38% | > 38% |
| Hortifruti | < 4% da RL | 4–6% | > 6% |
| Anúncios | sem meta (acompanhar) | — | — |

As faixas de Comidas, Bebidas e Hortifruti são referência de mercado, não do GASS — dizer isso no primeiro painel e oferecer ajustar.

## Passo a passo

1. **Extrair** os valores do DRE (mesma leitura da skill-mãe, passo 1). Anotar Receita Bruta, Receita Líquida e, se houver, receita por grupo.
2. **Calcular** os 8 indicadores em R$ e % na base da tabela acima. Conferir que CMV Comidas + CMV Bebidas ≈ CMV total; se a diferença passar de 2%, mostrar a diferença como "CMV não classificado".
3. **Competência**: extras e compras entram no mês em que foram gerados, não pagos (regra da skill-mãe, passo 5).
4. **Comparar** com o mês anterior em `/areas/bellabar-dre-historico.md` (variação em R$ e p.p.). Se for reenvio do mesmo mês, comparar com a versão anterior da conversa.
5. **Apresentar** no chat:
   - Uma linha de destaque: quantos indicadores no verde/amarelo/vermelho e o pior deles.
   - Tabela: Indicador | R$ | % | Meta | Status (🟢🟡🔴) | Δ vs mês anterior.
   - No máximo 3 bullets de ação para os vermelhos/amarelos (ex.: "CMV Bebidas subiu 4 p.p. — checar preço da Heineken nas últimas notas").
   - Terminar com uma única pergunta oferecendo aprofundar.
   - Só gerar painel HTML/artifact se o Richard pedir ("gera o painel", "manda em HTML"). Nesse caso: cabeçalho com o mês, gráfico de faturamento diário (se houver), 8 cards (% grande, R$, barra com a meta e semáforo), bloco Não Operacional e a tabela das linhas do DRE usadas. Atualizar o mesmo painel do mês em vez de criar outro.
6. **Histórico**: acrescentar ao registro do mês em `/areas/bellabar-dre-historico.md` as colunas CMV Comidas %, CMV Bebidas %, Couvert R$, Comissão R$, CMO+Eventual %, Hortifruti R$/% (atualizar a linha se o mês já existir). Sem pedir confirmação.
