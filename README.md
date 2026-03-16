# Predictive Cash Flow B2B: Previsão de Atrasos com Machine Learning

## 🎯 O Problema de Negócio
Dashboards de fluxo de caixa baseados exclusivamente na data de vencimento falham na vida real. A consequência de confiar cegamente no contrato é projetar um saldo positivo na sexta-feira e terminar o dia no vermelho porque o dinheiro não entrou. 

Este projeto muda a lógica da tesouraria: **para de projetar o caixa pelo vencimento e passa a usar Machine Learning para prever o comportamento real de pagamento dos clientes.**

## 🧠 A Arquitetura: Hurdle Model
No cenário B2B, a maioria das faturas é recebida em dia. Essa montanha de "zeros" (zero dias de atraso) puxa a previsão matemática de modelos de regressão tradicionais para baixo, inutilizando a predição. 

Para resolver isso, foi aplicada a arquitetura estatística de **Hurdle Model (Modelo de Barreira)** em dois estágios, utilizando Python e XGBoost:
1. **Classificador (A Barreira):** A fatura vai atrasar? (Sim/Não)
2. **Regressor (A Quantificação):** Se houver atraso, de quantos dias ele será?

## ⚙️ Engenharia de Dados: Dando "Memória" ao Algoritmo
Algoritmos baseados em árvores tratam cada fatura como um evento isolado. O maior desafio técnico foi construir a engenharia de features para dar contexto histórico ao modelo sem causar *data leakage* (vazamento de dados).

Utilizei **DuckDB** rodando nativamente no Python para processar variáveis comportamentais de forma otimizada:
* Frequência histórica de atrasos do cliente.
* Média ponderada de dias de atraso.
* Comportamento de disputas de faturas e pagamentos simultâneos.
    
Através de *Window Functions* em SQL com ordenação cronológica estrita, o modelo avalia cada nova fatura carregando todo o histórico de pagamentos daquele cliente liquidado estritamente até o dia anterior.

## 📊 Resultados e Impacto na Tesouraria
* **Classificador:** Recall de **84%**. O modelo captura a grande maioria dos atrasos reais antes que aconteçam. A precisão de 69% indica um comportamento levemente pessimista, o que é ideal para a tesouraria (o erro significa que o dinheiro entrou antes do previsto, protegendo o caixa).
* **Regressor:** Erro Médio Absoluto (MAE) estabilizado em **4 dias**, entregando uma janela de manobra realista para a equipe financeira.

**Métrica de Negócio:** O output final gera uma nova visão para o dashboard. A diferença entre a "Receita Esperada" (vencimento) e a "Receita Projetada pela IA" representa o **furo de caixa evitado**.

## 🛠️ Stack Tecnológico
* **Linguagem:** Python
* **Processamento de Dados:** DuckDB, Pandas
* **Machine Learning:** XGBoost, Scikit-Learn (Métricas de Avaliação)
* **Visualização:** Matplotlib, Seaborn
* **Ambiente:** Jupyter Notebook

## 🚀 Como Executar o Projeto

1. Clone o repositório:
```bash
git clone https://github.com/pedroteles/predictive-cashflow-b2b.git
```
2. Instale as dependências:
```bash   
pip install -r requirements.txt
```
4. Execute os notebooks para acompanhar o pipeline de dados, engenharia de features e o treinamento do modelo.
   
Nota: O pipeline foi validado com a base pública Finance Factoring da IBM.

📈 Próximos Passos
Ingestão de variáveis macroeconômicas (Selic, inflação).
Inclusão de dados setoriais para refinar a predição em cenários de estresse econômico.

📄 Licença
Distribuído sob a licença Apache 2.0. Veja o arquivo LICENSE para mais informações.


