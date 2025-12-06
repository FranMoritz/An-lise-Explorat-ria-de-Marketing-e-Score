# An-lise-Explorat-ria-de-Marketing-e-Score
Este repositório contém um notebook de Análise Exploratória de Dados (EDA) focado em entender como diferentes **canais** e **campanhas de marketing** impactam a **qualidade dos clientes adquiridos** por um banco digital, medida pelo **score de crédito**.

Bases Utilizadas

O notebook utiliza três tabelas principais:

fato_aquisicao_5000.csv

Eventos do funil de aquisição:

id_cliente, id_campanha

flags do funil: flag_clicou, flag_virou_lead, flag_enviou_proposta, flag_aprovado, flag_conta_aberta

datas de cada etapa (data_evento_primeiro_contato, data_proposta, data_aprovacao, data_conta_aberta)

custo_atribuido, modelo_atribuicao, etapa_maxima_funil

dim_cliente_5000.csv

Atributos dos clientes:

dados demográficos: data_nascimento, sexo, renda_mensal, faixa_renda, cidade, estado

informações de jornada: tipo_dispositivo, canal_aquisicao_principal, data_primeiro_contato, data_abertura_conta, flag_conta_aberta

risco de crédito: score_credito, faixa_score, flag_score_alto

status_cliente

dim_campanha_5000.csv

Dados das campanhas de marketing:

id_campanha, nome_campanha

canal, plataforma, objetivo_campanha, segmento_publico

score_minimo_alvo

data_inicio, data_fim

verba_total, impressoes, cliques, leads_gerados

🔧 Pipeline da Análise (o que o notebook faz)
1. Leitura dos dados

Leitura das três bases a partir do caminho /content/ (formato Colab).

Verificação de shape e estrutura básica.

2. Construção da ABT (tabela analítica)

Deduplicação de dim_cliente e dim_campanha por id_cliente e id_campanha.

LEFT JOIN:

df_aquisicao ⟵ dim_cliente (chave id_cliente)

resultado ⟵ dim_campanha (chave id_campanha)

Granularidade final: cliente em uma jornada de aquisição.

3. Metadados e Qualidade de Dados

Função generate_metadata() para criar:

tipo da coluna,

quantidade e % de nulos,

cardinalidade (número de valores distintos).

Exclusão de colunas com mais de 80% de valores nulos.

Preenchimento de nulos:

colunas numéricas (float64) → 9999;

colunas categóricas (object) → "não consta".

4. EDA – Score de Crédito

Histograma da distribuição de score_credito.

Criação da variável score_alto (score_credito >= 700).

Proporção de clientes com score ≥ 700 por canal.

Boxplots e scatter plots comparando canal × score.

5. EDA – Canais e CAC

Cálculo do CAC médio por canal a partir de custo_atribuido.

Cálculo de taxas de conversão por canal:

impressão → clique,

clique → lead,

lead → abertura de conta,

abertura → aprovação.

Matriz Eficiência x Qualidade:

eixo X: CAC médio por canal;

eixo Y: proporção de clientes com score ≥ 700.

Interpretação direta dos quadrantes (canais “estrela”, intermediários e ineficientes).

6. EDA – Campanhas

Ranking de campanhas que mais geram clientes.

Ranking de campanhas que mais geram clientes com score ≥ 700.

Heatmap nome_campanha × canal_aquisicao_principal para ver em quais canais cada campanha roda e com qual intensidade.

7. Relação entre Verba e Score

Scatter plot verba_total × score_credito com linha de regressão.

Conclusão: não há relação relevante entre o tamanho da verba da campanha e o score dos clientes captados.

8. Resumo Executivo

O notebook encerra com um resumo textual das recomendações de negócio, com foco em redistribuição de verba entre canais.

🧠 Principais Insights do Notebook

Social e Email Marketing:

Maior proporção de clientes com score ≥ 700.

CAC igual ou abaixo da média.

Canais prioritários para aumento de investimento.

Orgânico:

Qualidade razoável, levemente acima da média.

Não escalável via investimento direto.

Deve ser mantido, mas não é alavanca principal de verba.

Google Ads:

Maior CAC médio.

Menor proporção de clientes com score ≥ 700.

Posicionado no pior quadrante da matriz Eficiência x Qualidade.

Principal candidato à redução/realocação de investimento.

Campanhas Premium:

“Campanha molestias Premium” se destaca como principal geradora de clientes de alto score e roda em múltiplos canais.

Outras campanhas Premium têm desempenho intermediário e dependem mais de canais como Google Ads, abrindo espaço para realocação entre canais mais qualificados.

Verba não compra qualidade:

A análise verba_total × score_credito mostra que campanhas mais caras não geram clientes com melhor perfil de risco.

A alavanca correta é canal + segmentação, não apenas aumento de budget.
