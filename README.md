# ⚡ Aprendizado Online: Adaptação a Drifts em Demanda de Energia

Este repositório contém o projeto prático focado em engenharia de dados em fluxo (*data streaming*) e aprendizado de máquina adaptativo para a previsão de demanda de energia elétrica, lidando com o fenômeno de **Concept Drift** (mudança de conceito em tempo real).

O objetivo principal é avaliar como diferentes algoritmos adaptativos se ajustam sequencialmente a flutuações e transições temporais/climáticas drásticas utilizando a metodologia de **Avaliação Prequencial**.

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas

* **Linguagem principal:** Python 🐍
* **Ambiente de Desenvolvimento:** Jupyter Notebook (`.ipynb`)
* **Framework de Data Streaming:** `River` (Essencial para Machine Learning Online)
* **Manipulação e Análise de Dados:** `pandas`
* **Visualização de Dados:** `matplotlib`
* **Download Automatizado:** `kagglehub`

---

## 🔬 Metodologia e Avaliação Prequencial

Diferente do aprendizado tradicional em lote (*batch*), o modelo entra em um jogo sequencial com o ambiente. A cada novo dia simulado (janela deslizante de 7 instâncias - 1 semana), o modelo:
1. **Prevê** a demanda de energia com base no seu estado atual.
2. **Recebe** o valor real revelado pelo ambiente.
3. **Calcula o erro** do momento (`Rolling MAE`).
4. **Atualiza seus pesos** (`learn_one`) antes de receber o próximo registro.

Os testes utilizaram dados agregados por média diária para neutralizar ruídos horários, focando nas variáveis explicativas: `dayofweek`, `month`, `Temperature` e `Humidity`.

---

## 📊 Modelos Comparados e Resultados

Para entender a melhor estratégia de adaptação aos desvios de conceito, o projeto realizou um benchmark entre três algoritmos distintos do framework `River`:

1. **KNN Online (`neighbors.KNNRegressor`):** Baseado em vizinhos mais próximos com memória curta e foco no passado recente.
2. **Adaptive Random Forest (`forest.ARFRegressor`):** Um *ensemble* (floresta) considerado o estado da arte em estabilidade para streaming de dados.
3. **Hoeffding Adaptive Tree (`tree.HoeffdingAdaptiveTreeRegressor`):** Árvore de decisão solitária dotada de detectores internos de drift e poda automatizada.

### 📈 Desempenho Final (`Rolling MAE`)

* 🏆 **KNN Online:** **19.12 MW** de erro. Consagrou-se como o grande destaque pela agilidade de resposta no curto prazo e capacidade de recuperação imediata a variações repentinas.
* 🥈 **Adaptive Random Forest (ARF):** **68.52 MW** de erro. Mostrou-se extremamente robusta e estável ao longo de todo o período, evitando picos absurdos de erro.
* 🥉 **Hoeffding Tree (HAT):** **455.59 MW** de erro. Teve o pior desempenho e sofreu com picos gigantescos, evidenciando que árvores solitárias demoram mais para podar galhos obsoletos após mudanças bruscas.

---
