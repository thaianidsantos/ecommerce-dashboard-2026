# 📊 Relatório Executivo E-commerce 2026

Dashboard interativo desenvolvido para apresentar relatórios e métricas de desempenho de e-commerce.
Neste projeto, realizei uma Análise Exploratória de Dados (EDA) sobre um conjunto de dados de pedidos e transações de comércio eletrônico, com o objetivo de transformar dados transacionais em informações capazes de apoiar decisões de negócio.
O dataset utilizado foi o E-Commerce Orders & Transactions 2026, disponibilizado no Kaggle, contendo informações relacionadas a clientes, pedidos, produtos, pagamentos, descontos, entregas, devoluções e resultados financeiros.

🔗 Fonte dos dados: Kaggle — E-Commerce Orders & Transactions 2026

🔎 O que foi analisado?

A base possui informações como:
• ID e data do pedido
• ID e dados demográficos do cliente
• País, continente e cidade
• Produto e categoria
• Quantidade e preço
• Descontos
• Método de pagamento
• Status do pedido
• Método e custo de envio
• Devoluções e seus respectivos motivos
• Receita e lucro

🧹 1. Data Quality e identificação das inconsistências

Antes de iniciar as análises, realizei uma etapa de validação e tratamento da qualidade dos dados.
Foram avaliadas inconsistências relacionadas principalmente a:
• Status do pedido × status de pagamento;
• Pedidos cancelados com valores financeiros registrados;
• Pedidos devolvidos considerados como receita;
• Inconsistências entre pagamento e realização da venda;
• Registros que poderiam distorcer indicadores financeiros;
• Valores que não deveriam ser considerados como receita realizada.
Um dos principais aprendizados do projeto foi perceber que uma base pode possuir valores preenchidos e, ainda assim, não representar corretamente a realidade do negócio.

🛠️ 2. Tratamento dos dados

Para evitar a exclusão indiscriminada dos registros, a estratégia adotada foi preservar a informação original e criar regras de qualidade para determinar quais registros poderiam participar de cada análise.
Foi criada uma classificação dos registros e indicadores de inclusão para os principais KPIs financeiros.
Para a análise de receita realizada, foi adotada a seguinte regra de negócio:
Receita realizada = Pedido entregue + Pagamento confirmado + Sem devolução

Dessa forma, pedidos com inconsistências não foram simplesmente apagados.
Eles permaneceram na base para análises específicas, como:
📌 cancelamentos
📌 devoluções
📌 falhas de pagamento
📌 problemas operacionais
📌 perdas potenciais de receita
Enquanto isso, foram retirados dos cálculos de receita e lucro realizados, evitando distorções nos indicadores.

Execução do Tratamento de Dados:
Amostragem: 200.000 registros transacionais brutas contendo informações de clientes, pedidos, pagamentos, entregas, devoluções e valores financeiros.

Análise Exploratória (EDA) & Diagnóstico de Qualidade Antes da aplicação de qualquer filtro, a base passou por um diagnóstico inicial para identificar inconsistências financeiras e operacionais: Conflitos de Status: Identificação de pedidos marcados como Cancelado ou Devolvido, porém mantendo valores em campos de receita/lucro. Falhas de Pagamento: Pedidos em que o status de pagamento constava como Pendente ou Falhou, mas o pedido estava com status de entrega concluída. Distorção de Indicadores: Constatação de que a soma direta da coluna de receita bruta distorcia o resultado real do negócio por incluir faturamento não realizado.

Arquitetura da Solução & Regras de Negócio em vez de excluir os registros inconsistentes da base original (o que apagaria históricos valiosos sobre perdas e gargalos), a estratégia adotada dividiu o pipeline de dados em camadas: Camada Raw (Dados Brutos): Preservação dos 200.000 registros originais para análises operacionais (motivos de devolução, taxa de cancelamento, falhas de checkout). Camada Refined (Regra de Elegibilidade): Criação da regra booleana para filtrar a Receita Realizada:$$\text{Receita Realizada} = (\text{Status} = \text{"Entregue"}) \land (\text{Pagamento} = \text{"Confirmado"}) \land (\text{Devolução} = \text{"Não"})$$

Execução do Tratamento e Filtragem (Pipeline) Padronização de Tipos e Formatos: Conversão de datas para formato padrão ISO. Limpeza de valores nulos e tratamento de strings em colunas de status. Aplicação das Flags de Inclusão em KPIs: Criação de coluna auxiliar Is_Receita_Realizada (1 para válido, 0 para inválido).

Resultado do Filtro:
112.260 registros (56,1%) qualificados como válidos para o cálculo do faturamento real.
87.740 registros (43,9%) isolados para análise exclusiva de perdas e falhas operacionais.

Consolidação das Métricas Financeiras: Cálculo final sobre a base tratada: Receita Realizada: US$ 59,37M 
Lucro Realizado: US$ 17,12M Margem de Lucro: 28,83%. 


📊 3. Resultado da tratativa

Após o processo de tratamento:

200.000 registros foram analisados.
Desses, aproximadamente:
✅ 112.260 pedidos foram considerados válidos para a análise de receita realizada;
⚠️ 87.740 registros foram classificados fora do KPI de receita realizada.
Isso representa aproximadamente 56% dos registros que não deveriam ser utilizados diretamente para medir receita realizada.
O resultado financeiro após a aplicação das regras de negócio foi de aproximadamente:
💰 US$ 59,37 milhões em receita realizada
💵 US$ 17,12 milhões em lucro realizado
📈 28,83% de margem realizada
O ticket médio dos pedidos considerados válidos também foi calculado para apoiar a análise de comportamento de compra.

📈 4. Análises de negócio propostas

A partir da base tratada, foram estruturadas possibilidades de análises como:
Vendas e rentabilidade

•	Evolução mensal da receita;
•	Lucro e margem;
•	Ticket médio;
•	Performance por categoria;
•	Performance regional.
Descontos
•	Relação entre desconto, volume de vendas e margem;
•	Identificação de faixas de desconto com maior impacto na rentabilidade;
•	Avaliação da eficiência das promoções.
Clientes
•	Frequência de compras;
•	Valor financeiro dos clientes;
•	Segmentação de clientes;
•	Identificação de clientes de maior valor.
Produtos
•	Produtos com maior faturamento;
•	Produtos mais rentáveis;
•	Produtos com maior índice de devolução;
•	Relação entre volume, margem e devoluções.
Operação
•	Análise de métodos de pagamento;
•	Custos de envio;
•	Devoluções;
•	Motivos de devolução;
•	Possíveis impactos logísticos na rentabilidade.

💡 5. Principais aprendizados

Um dos principais insights deste projeto foi entender que qualidade de dados não é apenas uma etapa técnica: é uma etapa de negócio.
Antes de responder:
“Quanto a empresa vendeu?”
é necessário responder:
“Quais registros realmente representam uma venda realizada?”
Da mesma forma, um pedido cancelado, devolvido ou com pagamento inconsistente não deve necessariamente ser eliminado. Ele pode representar uma informação extremamente importante para entender onde o processo comercial ou operacional está falhando.
Por isso, a estratégia adotada foi separar:
Dados originais → Dados tratados → Regras de negócio → KPIs confiáveis → Insights
Essa abordagem permite preservar o histórico e, ao mesmo tempo, evitar que inconsistências contaminem os indicadores utilizados na tomada de decisão.

🎯 Conclusão
Este projeto reforçou uma das principais competências que considero essenciais na área de Análise de Dados:
não basta gerar um gráfico ou calcular um KPI. É necessário compreender o contexto do negócio, validar a qualidade dos dados, estabelecer regras coerentes e transformar os resultados em informações que possam apoiar decisões.
A partir dessa base, o próximo passo seria aprofundar análises de rentabilidade, comportamento dos clientes, eficiência de descontos, devoluções e performance de produtos e regiões, podendo posteriormente transformar esses indicadores em um dashboard executivo para acompanhamento da operação.

🛠️ Principais competências aplicadas:
Data Quality • Tratamento de Dados • Análise Exploratória (EDA) • Regras de Negócio • KPIs • Análise de Rentabilidade • Análise de Vendas • Data Storytelling


## 🔗 Link de Acesso
> 🌐 [Clique aqui para visualizar o Dashboard ao vivo](https://thaianidsantos.github.io/ecommerce-dashboard-2026/)

---

## 🎯 Objetivo do Projeto
Apresentar uma visão executiva e consolidada dos principais KPIs de vendas, faturamento, conversão e desempenho geral do negócio de e-commerce.

---

## 🛠️ Tecnologias Utilizadas
* **HTML5** - Estruturação da página.
* **CSS3** - Estilização e layout responsivo.
* **JavaScript** - Interatividade do relatório.

---

## 🚀 Como Visualizar Localmente
1. Faça o download ou clone este repositório:
   ```bash
   git clone [https://github.com/thaianidsantos/ecommerce-dashboard-2026.git](https://github.com/thaianidsantos/ecommerce-dashboard-2026.git)

