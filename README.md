# 🎬 Análise de Lançamentos Comerciais no Mercado Audiovisuais Brasileiro 

> **Diagnóstico da distribuição e desempenho comercial de obras audiovisual com dados da ANCINE (Power BI + SQLite)**


---
## 🎯 Contexto e Motivação do Projeto
O mercado de audiovisual no Brasil movimenta valores expressivos anualmente. No entanto, distribuidoras e produtoras de médio e pequeno porte enfrentam um cenário de alta concentração, no qual um pequeno grupo de *blockbusters* domina salas e receita. 

Para apoiar a tomada de decisão estratégica e a alocação eficiente de verbas de produção e marketing, este projeto oferece um diagnóstico quantitativo detalhado sobre a dinâmica de distribuição e a concorrência histórica no setor, transformando dados brutos em inteligência competitiva.

### ❓ Perguntas de Negócio Avaliadas
⚖️ **Análise Comparativa (Nacional vs. Estrangeiro)**

1. **Dominância de Mercado:** Qual é a diferença percentual de participação no público total acumulado e na renda obtida entre produções estrangeiras e nacionais no período de 2009 a 2025?
2. **Preferência do Público por Gênero:** Qual gênero entrega a melhor relação entre volume de lançamentos e bilheteria alcançada?

🎨 **Análise do Gênero Animação (Obras Estrangeiras)**

3. **Desempenho da Animação Internacional:** Qual é a participação do gênero Animação no público total acumulado das obras estrangeiras e quais são as 3 obras com maior bilheteria?

📜 **Análise do Gênero Documentário (Obras Nacionais)**

4. **Representatividade do Documentário Brasileiro:** Quantos obras nacionais são Documentários e qual é o público típico desse gênero?

---
## 📂 Base de Dados da ANCINE
Disponibilizada no [Portal Brasileiro de Dados Abertos](https://dados.gov.br/dados/conjuntos-dados/lancamentos-comerciais-por-distribuidoras), a base de dados da Agência Nacional do Cinema ([ANCINE](https://www.gov.br/ancine/pt-br)) utilizada reúne informações de lançamentos comerciais de obras audiovisuais no Brasil. 
- **Arquivo Original:** [`lancamentos-comerciais-por-distribuidoras.csv`](https://github.com/felipeoliveira-feol/desafio_power_bi_bd_ancine/blob/main/data/raw/lancamentos-comerciais-por-distribuidoras.csv)
- **Data de Coleta/Atualização:** 17/07/2026
- **Fonte de Validação Externa:** [Portal de Consulta de Obras Não Publicitárias da ANCINE](https://sad2.ancine.gov.br/obrasnaopublicitarias/consultarObraViaPortal/consultarObraViaPortal.seam)

---
## 📝 Descrição e Etapas do Projeto
Este projeto de análise de dados abrangendo desde a coleta e tratamento dos dados abertos da ANCINE até a construção de painéis visuais interativos, bem como a interpretação e insights extraídos a partir das informações obtidas. 

Para oferecer clareza analítica a produtores e distribuidoras no planejamento de lançamentos, o projeto foi dividido em **3 etapas**:

### 🛠️ Etapa 1: Saneamento, Tratamento e Validação (SQL/SQLite)
> Bases de dados abertas frequentemente apresentam inconsistências, duplicidades, valores vazios, ausência de padronização e falhas de indexação. 

Para garantir a consistência e a confiabilidade das informações antes da carga no Power BI, a base de dados foi refinada ([mais detalhes aqui](https://github.com/felipeoliveira-feol/desafio_power_bi_bd_ancine/blob/main/src/README.md)). 

Ao todo, **3,38% dos registros foram removidos** (243 de 7.188 linhas) e **13 registros foram modificados** (1 preenchimento de valor vazio e 12 substituições/correções de dados). Duas colunas foram removidas e uma coluna foi adicionada e preenchida.

### 📊 Etapa 2: Dashboard - Visão Comparativa do Mercado (Obras Estrangeiras vs. Nacionais)
> Desenvolvimento de um relatório no **Power BI** para analisar o panorama geral de lançamentos comerciais no Brasil (2009–2025), confrontando o desempenho de obras nacionais e estrangeiras.

O painel comparativo consolida indicadores para identificar o grau de dominância das obras internacionais frente as brasileiras, fornecendo métricas de:
- Total de obras lançadas, público e renda total acumulada (Geral, Estrangeira e Nacional).
- Mediana do público entre as obras (Geral).
- Percentual do Total de obras lançadas, público e renda total acumulada Nacional em relação ao Geral.
- Distribuição do volume de obras e de público por gênero da obra (Geral).
- Ranking das 3 obras de maior bilheteria (Estrangeira e Nacional).

### 📊 Etapa 3: Dashboard - Visão Segmentada por Origem da Obra (Estrangeira e Nacional)
> Desenvolvimento de uma camada de análise focado na avaliação detalhada do mercados para obras estrangeiras e nacionais de forma independente, permitindo identificar padrões de comportamento de consumo e dinâmica de distribuição.

Esta camada fornece relatórios isolados com métricas exclusivas para o segmento selecionado (Nacional ou Estrangeiro):
- Total de obras lançadas, público e renda total acumulada acumulada do segmento.
- Mediana do público e mediana de renda entre as obras.
- Distribuição do volume de obras e de público por gênero da obra.
- Ranking das obras de maior bilheteria (%).
- Ranking das distribuidoras com maior renda de bilheteria (%).
- Série Temporal do Total de Obras ao longo dos anos.
- Série Temporal do Total de Público alcançado ao longo dos anos.

---
### 💻 Estrutura e Navegação do Relatório
A estrutura analítica no Power BI foi desenvolvida de forma modular, permitindo a exploração dos dados desde um panorama comparativo entre obras nacionais e estrangeiras até análises específicas por origem da obra. Além disso, todos os relatórios possuem o botão de segmentação por gênero da obra. 

> **Importância da Segmentação:** Analisar o setor como um bloco homogêneo oculta particularidades críticas de nicho e país de produção das obras (obra nacional ou estrageira).

A partir da página de **Índice**, é possível acessar diretamente os três módulos principais do relatório:

* **Acesse o Dashboard Interativo Online:** [Link para o Power BI Web](https://app.powerbi.com/view?r=eyJrIjoiZjlhYjg3ZWEtMjIwMy00ZjBiLWIwMGUtNWNlNzIzMWJiOTc1IiwidCI6ImMzN2IzN2EzLWU5ZTItNDJmOS1iYzY3LTRiOWI3MzhlMWRmMCJ9&pageName=1fd9ec708e06c2e10d10).
* **Teste o Arquivo do Dashboard ([.pbix](https://github.com/felipeoliveira-feol/desafio_power_bi_bd_ancine/blob/main/src/reports/proj-bi-bd-ancine-v1.pbix))** 


---
## 💡 Principais Insights e Respostas às Perguntas de Negócio
### 1. Qual é a diferença percentual de participação no público total acumulado e na renda obtida entre produções estrangeiras e nacionais no período de 2009 a 2025?
- No acumulado do período, as 6.747 obras lançadas movimentaram ~2,28 bilhões de espectadores e ~R$ 32,61 bilhões em renda obtida. As produções nacionais representam 31,85% do total de títulos lançados (2.149 obras), mas capturaram no total apenas 12,79% do público (~292 milhões) e 10,93% da renda (~3,57 bilhões).
- **Insights:** O mercado exibe um descompasso estrutural de participação. Embora a cinematografia brasileira ocupe quase um terço da prateleira de lançamentos em salas de exibição, ela retém cerca de um décimo do faturamento total. Isso evidencia a forte concentração do consumo em títulos estrangeiros (blockbusters) e a baixa ocupação média por sala das produções nacionais.

### 2. Qual gênero entrega a melhor relação entre volume de lançamentos e bilheteria alcançada?
- O gênero Ficção lidera em volume absoluto, englobando 76,98% dos lançamentos e concentrando 80,43% do público total (~1,835 bilhão de espectadores). Por outro lado, o gênero Animação representa apenas 6,21% dos títulos lançados, mas responde por 18,85% do público total (~430 milhões de espectadores). Entre os títulos internacionais, a animação INSIDE OUT 2 (2024) ocupa o 1º lugar em bilheteria entre as obras estrangeiras, com ~22 milhões de espectadores. Por outro lado, existe uma distribuição assimétrica e a mediana do público total do gênero é ~149 mil espectadores.
- **Insights:** A Animação apresenta a maior eficiência de público por título do mercado audiovisual brasileiro, impulsionada por superproduções de alcance massivo. No entanto, esse comportamento camufla a real distribuição do mercado: o desempenho global do gênero é fortemente puxado por grandes franquias de estúdios globais. Por isso, a mediana de público é a métrica mais assertiva para analisar o desempenho típico de uma animação, isolando o efeito dos superlançamentos de grandes estúdios.

### 3. Qual é a participação do gênero Animação no público total acumulado das obras estrangeiras e quais são as 3 obras com maior bilheteria?
- Entre os 4.598 obras estrangeiras, o gênero Animação soma 371 obras (8,07% do catálogo internacional), mas captura 21,54% do público total (~429 milhões dos ~1,989 bilhão de espectadores). As três animações com maior bilheteria juntas representam 9,17% de todo o público acumulado pelo gênero, sendo elas: INSIDE OUT 2 (2024), THE INCREDIBLES 2 (2018) e ICE AGE: DAWN OF THE DINOSSAURS (2009), respectivamente.
- **Insights:** O segmento de animação internacional possui alta concentração nos grandes lançamentos de franquia. Poucos títulos de grande apelo alcançam parcelas massivas de mercado, demonstrando a força do modelo de distribuição de grandes estúdios globais no público familiar brasileiro.

### 4. Quantos obras nacionais são Documentários e qual é o público típico desse gênero?
- Dos 2.149 obras nacionais, o gênero Documentário representa 37,79% da produção (812 obras), mas responde por apenas 1,19% do público total nacional (~3 milhões dos ~292 milhões de espectadores). A mediana de público é de apenas 723 espectadores por filme. Na análise temporal (2009–2025), observa-se apesar da queda em 2020 (pandemia) um crescimento no volume de lançamentos de documentários, em contraste com uma tendência contínua de queda no público acumulado.
- **Insights:** Há um claro gargalo de comercialização no cinema documental brasileiro. Apesar de ser uma força criativa e de volume dentro da produção nacional incentivada por políticas de fomento público, o gênero enfrenta severa dificuldade de alcance nas salas comerciais, com uma mediana de público que aponta para a necessidade de estratégias focadas em circuitos alternativos ou streaming.

---
## 🚀 Limitações e Próximos Passos
**Escopo e Limitações Atuais:**
- **Exclusividade de Salas de Cinema:** Os dados refletem apenas a exibição comercial física (cinemas), excluindo streaming (VOD), e TV aberta e paga.
- **Ausência de Dados de Custos:** A base não contempla orçamentos de produção ou verbas de marketing, impossibilitando o cálculo de ROI (Retorno sobre Investimento) líquido.

**Backlog de Melhorias Futuras:**
- [ ] **Cruzamento com Dados de Fomento:** Integrar dados de incentivo fiscal e editais (ANCINE e/ou FSA) para avaliar a taxa de conversão econômica do cinema nacional subsidiado.
- [ ] **Clusterização de Obras:** Aplicar modelos de Machine Learning (ex.: K-Means) para agrupar obras por perfil de desempenho (ex.: Blockbusters Globais, Sucessos Médios, Nicho Cult e Circuito Regional).
- [ ] **Enriquecimento de Dados:** Cruzar a base com dados do IMDb e/ou Letterboxd para correlacionar aprovação de público e crítica ao desempenho comercial.

## 👤 Contribuições e Contribuidores
O projeto foi desenvolvido pelo gestor da informação, Felipe dos Santos de Oliveira (**[LinkedIn](https://www.linkedin.com/in/felipe-so/)**).

Caso tenha dúvidas ou sugestões, entre em contato através do GitHub ou envie um e-mail para: **[felipeoliveira.feol@gmail.com.br](mailto:felipeoliveira.feol@gmail.com.br)**.
