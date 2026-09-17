## 📝 Descrição e Etapas do Projeto

### 🛠️ Etapa 1: Saneamento, Tratamento e Validação (SQL/SQLite)
> Bases de dados abertas frequentemente apresentam inconsistências, duplicidades, valores vazios, ausência de padronização e falhas de indexação. 

Para garantir a consistência e a confiabilidade das informações antes da carga no Power BI, a versão final do banco de dados foi obtida por meio das seguintes ações:

1. **Recorte Temporal (2009–2025):** Remoção de 240 registros referentes ao ano de 2026. Como o ano em vigor (2026) ainda não foi concluído, o descarte evita análises parciais e garante a comparabilidade do histórico de lançamentos ao longo dos anos.

**Script:**
```
-- Consulta:
SELECT * FROM bd_ancine;

-- Etapa 1:
-- Consulta:
SELECT * FROM bd_ancine
LIMIT 241;

-- Alteração:
DELETE FROM bd_ancine
LIMIT 240;
```

2. **Otimização de Atributos:** Exclusão das colunas de número de registro e CNPJ da distribuidora, mantendo-se somente a Razão Social da distribuidora como identificador para a análise de mercado.

**Script:**
```
-- Etapa 2:  
-- Alteração:
ALTER TABLE bd_ancine
DROP COLUMN REGISTRO_DISTRIBUIDORA;

ALTER TABLE bd_ancine
DROP COLUMN CNPJ_DISTRIBUIDORA;
```

3. **Tratamento de Dados Ausentes:** Preenchimento do país de origem para uma obra sem essa informação (`E2400142000000`). Após consulta no portal da ANCINE, identificou-se a origem como `ESTADOS UNIDOS`.

**Script:**
```
-- Etapa 3:  
-- Consulta:
SELECT * FROM bd_ancine
WHERE DATA_LANCAMENTO_OBRA IS NULL OR TRIM(DATA_LANCAMENTO_OBRA) = ''
    OR TITULO_ORIGINAL IS NULL OR TRIM(TITULO_ORIGINAL) = ''
    OR CPB_ROE IS NULL OR TRIM(CPB_ROE) = ''
    OR TIPO_OBRA IS NULL OR TRIM(TIPO_OBRA) = ''
    OR PAIS_OBRA IS NULL OR TRIM(PAIS_OBRA) = ''
    OR PUBLICO_TOTAL IS NULL
    OR RENDA_TOTAL IS NULL
    OR RAZAO_SOCIAL_DISTRIBUIDORA IS NULL OR TRIM(RAZAO_SOCIAL_DISTRIBUIDORA) = '';
    
SELECT * FROM bd_ancine      
WHERE (DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA) = ('02/05/2024', 'THE CHOSEN', 'SM DISTRIBUIDORA DE FILMES LTDA');

-- Alteração:
UPDATE bd_ancine 
SET PAIS_OBRA = 'ESTADOS UNIDOS'      
WHERE (DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA) = ('02/05/2024', 'THE CHOSEN', 'SM DISTRIBUIDORA DE FILMES LTDA');
```

4. **Deduplicação de Registros:** Identificação de 3 duplicidades com base na combinação de data de lançamento, título da obra e distribuidora. Em cada agrupamento, preservou-se o registro com maior apuração de público e renda, descartando-se as inconsistências com valores menores.

**Script:**
```
-- Etapa 4:  
-- Consulta:
SELECT * FROM bd_ancine
WHERE (DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA) IN (
    SELECT DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA
    FROM bd_ancine
    GROUP BY DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA
    HAVING COUNT(*) > 1);
 
SELECT * FROM bd_ancine 
WHERE (CPB_ROE, DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA) IN (
    ('E1300000100000', '11/02/2011', 'THE KING''S SPEECH', 'SM DISTRIBUIDORA DE FILMES LTDA'),
    ('E1300000100000', '05/06/2009', 'CARAMEL', 'TAG CULTURAL DISTRIBUIDORA DE FILMES LTDA'),
    ('E1500668200000', '13/02/2009', 'FRIDAY THE 13TH', 'PARAMOUNT PICTURES BRASIL DISTRIBUIDORA DE FILMES LTDA'));

-- Alteração:
DELETE FROM bd_ancine
WHERE (CPB_ROE, DATA_LANCAMENTO_OBRA, TITULO_ORIGINAL, RAZAO_SOCIAL_DISTRIBUIDORA) IN (
    ('E1300000100000', '11/02/2011', 'THE KING''S SPEECH', 'SM DISTRIBUIDORA DE FILMES LTDA'),
    ('E1300000100000', '05/06/2009', 'CARAMEL', 'TAG CULTURAL DISTRIBUIDORA DE FILMES LTDA'),
    ('E1500668200000', '13/02/2009', 'FRIDAY THE 13TH', 'PARAMOUNT PICTURES BRASIL DISTRIBUIDORA DE FILMES LTDA'));
```

5. **Correção de Chaves Primárias (CPB/ROE):**
    - Preenchimento do código correto de CPB/ROE das obras `KARATE KID, THE` (`E1600548700000`) e `THE BIG FOUR ` (`E1600633500000`), identificados após pesquisa no portal da ANCINE.
    - Para 10 obras sem identificador localizável (originalmente categorizadas como `E1300000100000`), atribuíram-se códigos sequenciais padronizados (`N/C 1` a `N/C 10`). Abordagem adotada para preservar o histórico de biheteria sem comprometer a integridade referencial das obras.

**Script:**
```
-- Etapa 5:
-- Consulta:
SELECT CPB_ROE, COUNT(*) AS frequencia FROM bd_ancine
GROUP BY CPB_ROE
ORDER BY frequencia DESC;

-- Observação: Uma obra pode ter uma ou mais distribuidoras como verificado e confirmado pesquisando na internet. 
-- Entretano, há uma anomalia com os registros com CPB_ROE E1300000100000.

SELECT * FROM bd_ancine
WHERE CPB_ROE == 'E1300000100000';
 
-- Alteração:
UPDATE bd_ancine 
SET CPB_ROE = CASE DATA_LANCAMENTO_OBRA
    WHEN '22/06/2010' THEN 'E1600633500000'
    WHEN '27/08/2010' THEN 'E1600548700000'
    WHEN '06/02/2009' THEN 'N/C 1'
    WHEN '20/03/2009' THEN 'N/C 2'
    WHEN '27/03/2009' THEN 'N/C 3'
    WHEN '05/06/2009' THEN 'N/C 4'
    WHEN '11/06/2009' THEN 'N/C 5'
    WHEN '31/07/2009' THEN 'N/C 6'
    WHEN '09/10/2009' THEN 'N/C 7'
    WHEN '19/02/2010' THEN 'N/C 8'
    WHEN '13/01/2014' THEN 'N/C 9'
    WHEN '25/10/2014' THEN 'N/C 10'
    ELSE CPB_ROE
    END
    WHERE CPB_ROE = 'E1300000100000';
```

6. **Categorização de Origem da Obra:** Criação e preenchimento da coluna **ORIGEM_OBRA** (`NACIONAL` ou `ESTRANGEIRA`) para segmentar os títulos com base no país da obra, assim permitido  criar análises comparativas desses segmentos no mercado audiovisual brasileiro.

**Script:**
```
-- Etapa 6:
-- Alteração:
ALTER TABLE bd_ancine 
ADD COLUMN ORIGEM_OBRA TEXT;

UPDATE bd_ancine 
SET ORIGEM_OBRA = CASE 
    WHEN PAIS_OBRA = 'BRASIL' THEN 'NACIONAL'
    ELSE 'ESTRANGEIRA'
    END;
 ```

> 💡 **RESUMO:** Ao todo, **3,38% dos registros foram removidos** (243 de 7.188 linhas) e **13 registros foram modificados** (1 preenchimento de valor vazio e 12 substituições/correções de dados). Duas colunas foram removidas e uma coluna foi adicionada e preenchida.

---
## 📄 Base de Dados Tratada
- **Arquivo Tratado:** [`lancamentos-comerciais-por-distribuidoras-v1.csv`](https://github.com/felipeoliveira-feol/desafio_power_bi_bd_ancine/blob/main/data/processed/lancamentos-comerciais-por-distribuidoras-v1.csv)

 ### 📌 Descrição dos Atributos da Base de Dados (Autoral)

| Atributo | Descrição |
| :--- | :--- |
| `DATA_LANCAMENTO_OBRA` | Data de lançamento comercial da obra no Brasil |
| `TITULO_ORIGINAL` | Nome original da obra |
| `CPB_ROE` | Certificado de Produto Brasileiro (CPB) ou Registro de Obra Estrangeira (ROE) - Identificador único da obra |
| `TIPO_OBRA` | Gênero da obra (ex.: Animação, Documentário, Ficção, etc.) |
| `PAIS_OBRA` | País de origem/produção da obra |
| `PUBLICO_TOTAL` | Total acumulado de espectadores da obra |
| `RENDA_TOTAL` | Renda total bruta arrecadada da obra (R$) |
| `RAZAO_SOCIAL_DISTRIBUIDORA` | Razão social da empresa distribuidora da obra |
| `ORIGEM_OBRA` | Origem/produção da obra (Nacional ou Estrangeira) |

 ### 📌 Tabela Resumo de Dados Estatísticos dos Atributos Númericos

🔴 **Público Total por Obra**
| Origem das Obras | Mínimo | Média | Mediana | Máximo | Desvio Padrão | Coef. de Variação |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| Estrangeira | 3 | 432.582 | 25.091 | 22.473.802 | 1.302.621 | 301,13% |
| Nacional | 1 | 135.780 | 1.774 | 12.173.228 | 728.912 | 536,83% |
| Geral | 1 | 338.047 | 11.573 | 22.473.802 | 1.159.618 | 343,03% |

🔴 **Renda Total por Obra**
| Origem das Obras | Mínimo | Média | Mediana | Máximo | Desvio Padrão | Coef. de Variação |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| Estrangeira | R$ 40,00 | R$ 6.317.066,18 | R$ 368.258,60 | R$ 444.469.144,86 | R$ 20.750.318,43 | 328,48% |
| Nacional | R$ 10,00 | R$ 1.659.415,45 | R$ 20.415,30 | R$ 169.400.632,11 | R$ 8.860.157,33 | 533,93% |
| Geral | R$ 10,00 | R$ 4.833.548,85 | R$ 161.833,91 | R$ 444.469.144,86 | R$ 17.976.235,47 | 371,91% |

> 💡 **Medida de Tedência Central:** Os coefientes de variação encontrados indicam alta dispersão e baixa homogeneidade dos dados em relação a média. Desse modo, deve-se considerar como valor central em torno do qual os dados estão distribuídos o valor da mediana ao invés da média, pois ela não sofre influência de valores extremos.
