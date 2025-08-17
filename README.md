# Dados analisados no artigo "[MODELO MATEMÁTICO PARA O PLANEJAMENTO INTEGRADO DA PRODUÇÃO E CORTE DE PAPEL, COM TEMPOS E CUSTOS DE SETUP](https://github.com/Daniel-C-Fernandes/Instances-Fernandes-2025/blob/main/Fernandes_2025_SBPO_submissao%20(5).pdf)"

The data sets used in the [article](https://github.com/Daniel-C-Fernandes/Instances-Fernandes-2025/blob/main/Fernandes_2025_SBPO_submissao%20(5).pdf) are available online at: [Data](https://github.com/Daniel-C-Fernandes/Instances-Fernandes-2025)

---

### Article Summary  
This article presents a mathematical model for the integrated planning of paper production and cutting, emphasizing resource efficiency, sustainability, and operational cost reduction. As summarized by the authors, the work extends an existing literature model to integrate *jumbo* (large paper roll) production and subsequent cutting into smaller rolls, incorporating **limited cutting machine capacity**, **sequence-independent setup times/costs** for pattern changes, and multi-period planning. The primary goal is to minimize total costs—covering paper production, cutting, inventory, and setups—while meeting demand without delays. To solve this NP-hard combinatorial problem, the **column generation technique** by Gilmore and Gomory (1961, 1963) is applied. This method iteratively solves a restricted problem (linear relaxation) via the simplex algorithm, adding attractive cutting patterns identified through an integer knapsack subproblem. Numerical tests on 270 instances (grouped into 27 classes) demonstrated **high-quality lower bounds** for optimal solutions, with computational times ranging from seconds to minutes based on complexity. The study concludes that the proposed integrated model is robust for complex industrial scenarios, reducing waste and costs, but requires further refinements such as stochastic optimization for uncertainty handling.  

---

### Mathematical Model  
The model combines **lot sizing** (jumbo production) and **multi-period cutting** (transformation into smaller rolls), formalized as follows:  

#### **Objective Function (1)**:  
Minimizes total costs:  
$$
\min \left( \sum_{m=1}^{M} \sum_{t=1}^{T} \left( c_{kmt}^{x} x_{kmt} + c_{kmt}^{w} w_{kmt} + c_{kmt}^{z} z_{kmt} \right) + \sum_{t=1}^{T} \left( \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} c_{kmt}^{y} y_{jkmt} + \sum_{i=1}^{N} \sum_{k=1}^{K} c_{ikt}^{e} e_{ikt} + \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} c_{jkmt}^{sc} s_{jkmt} \right) \right)
$$
**Explanation**:  
- Production costs ($c_{kmt}^{x} x_{kmt}$), master roll inventory ($c_{kmt}^{w} w_{kmt}$), and production setup costs ($c_{kmt}^{z} z_{kmt}$).  
- Cutting costs ($c_{kmt}^{y} y_{jkmt}$), final item inventory ($c_{ikt}^{e} e_{ikt}$), and cutting machine setup costs ($c_{jkmt}^{sc} s_{jkmt}$).  

#### **Key Constraints**:  
1. **Demand fulfillment (2)**:  
   $$
   \sum_{m=1}^{M} \sum_{j=1}^{n_m} a_{ijkmt} y_{jkmt} + e_{ikt-1} - e_{ikt} = d_{ikt}, \quad \forall i, k, t
   $$
   **Explanation**: Demand for item $i$ (grammage $k$, period $t$) is met by cutting master rolls + previous-period inventory.  

2. **Cutting machine capacity (3)**:  
   $$
   \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} \left( c_{jkm} y_{jkmt} + p_{jkm} s_{jkmt} \right) \leq \text{Cap}_t^c, \quad \forall t
   $$
   **Explanation**: Cutting time ($c_{jkm}$) and setup time ($p_{jkm}$) must not exceed cutting machine capacity in period $t$.  

3. **Setup-cutting linkage (4)**:  
   $$
   y_{jkmt} \leq Q_1 s_{jkmt}, \quad \forall j, k, m, t
   $$
   **Explanation**: Cutting only occurs if the machine is set up for pattern $j$ ($s_{jkmt} = 1$). $Q_1$ is a large number to enforce setup activation.  

4. **Production capacity (5)**:  
   $$
   \sum_{k=1}^{K} \left( b_{km} x_{kmt} + f_{km} z_{kmt} \right) \leq \text{Cap}_{mt}^p, \quad \forall m, t
   $$
   **Explanation**: Weight of produced rolls ($b_{km} x_{kmt}$) + setup waste ($f_{km} z_{kmt}$) must not exceed paper machine capacity.  

5. **Setup-production linkage (6)**:  
   $$
   x_{kmt} \leq Q_2 z_{kmt}, \quad \forall k, m, t
   $$
   **Explanation**: Master roll production occurs only with a setup ($z_{kmt} = 1$). $Q_2$ is a large number.  

6. **Inventory balance (7)**:  
   $$
   \sum_{j=1}^{n_m} y_{jkmt} = x_{kmt} + w_{kmt-1} - w_{kmt}, \quad \forall k, m, t
   $$
   **Explanation**: Cut master rolls ($y_{jkmt}$) equal production ($x_{kmt}$) + prior inventory ($w_{kmt-1}$) minus current inventory ($w_{kmt}$).  

#### **Knapsack Subproblem (12-14)**:  
Used in column generation to identify attractive cutting patterns:  
$$
\max \left( \sum_{i=1}^{N} \pi_i \alpha_i \right) \quad \text{subject to} \quad \sum_{i=1}^{N} l_i \alpha_i \leq L_m, \quad \alpha_i \in \mathbb{Z}_+  
$$
**Explanation**: Maximizes utility ($\pi_i$: dual prices from demand constraints) of pattern $\alpha_i$ (number of items $i$ cut), respecting the master roll width $L_m$.  

---

### Model Conclusion  
The proposed model advances existing work by integrating **cutting machine setups** and **limited capacity** into the combined lot sizing and multi-period cutting problem. Column generation proved efficient for obtaining near-optimal solutions with feasible computational effort, validating its utility for complex industrial decision-making. Future work will extend the model to stochastic settings, incorporating parameter uncertainties.

---

### Resumo do Artigo  
O artigo trata do desenvolvimento de um modelo matemático para o planejamento integrado da produção e corte de papel, com foco na eficiência de recursos, sustentabilidade e redução de custos operacionais. Conforme apresentado pelos autores, o trabalho estende um modelo existente da literatura para integrar os processos de produção de *jumbos* (bobinas grandes de papel) e seu subsequente corte em bobinas menores, considerando **capacidade limitada da cortadeira**, **tempos e custos de setup independentes da sequência** na mudança de padrões de corte. O objetivo principal é minimizar os custos totais, abrangendo produção de papel, corte, estoque e setups, enquanto atende à demanda sem atrasos. Para resolver o problema — classificado como NP-difícil devido à sua complexidade combinatória —, aplicou-se a **técnica de geração de colunas** de Gilmore e Gomory (1961, 1963). Essa abordagem resolve iterativamente um problema restrito (relaxação linear) usando o método *simplex*, adicionando padrões de corte atrativos identificados via um subproblema da mochila inteira. Resultados de testes numéricos com 270 instâncias (organizadas em 27 classes) demonstraram **limitantes inferiores de alta qualidade** para a solução ótima, com tempos computacionais variando de segundos a minutos conforme a complexidade. A conclusão destaca que o modelo integrado proposto é robusto para cenários industriais complexos, reduzindo desperdícios e custos, mas ainda requer aprimoramentos, como a inclusão de incertezas via otimização estocástica.  

---

### Modelo Matemático  
O modelo integra **dimensionamento de lotes** (produção de *jumbos*) e **corte multiperíodo** (transformação em bobinas menores), com as seguintes formulações:  

#### **Função Objetivo (1)**:  
Minimiza os custos totais:  
$$
\min \left( \sum_{m=1}^{M} \sum_{t=1}^{T} \left( c_{kmt}^{x} x_{kmt} + c_{kmt}^{w} w_{kmt} + c_{kmt}^{z} z_{kmt} \right) + \sum_{t=1}^{T} \left( \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} c_{kmt}^{y} y_{jkmt} + \sum_{i=1}^{N} \sum_{k=1}^{K} c_{ikt}^{e} e_{ikt} + \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} c_{jkmt}^{sc} s_{jkmt} \right) \right)
$$
**Explicação**:  
- Custos de produção ($c_{kmt}^{x} x_{kmt}$), estoque de bobinas-mestre ($c_{kmt}^{w} w_{kmt}$) e setup na produção ($c_{kmt}^{z} z_{kmt}$).  
- Custos de corte ($c_{kmt}^{y} y_{jkmt}$), estoque de itens finais ($c_{ikt}^{e} e_{ikt}$) e setup na cortadeira ($c_{jkmt}^{sc} s_{jkmt}$).  

#### **Restrições Principais**:  
1. **Atendimento à demanda (2)**:  
   $$
   \sum_{m=1}^{M} \sum_{j=1}^{n_m} a_{ijkmt} y_{jkmt} + e_{ikt-1} - e_{ikt} = d_{ikt}, \quad \forall i, k, t
   $$
   **Explicação**: A demanda do item $i$ (gramatura $k$, período $t$) é atendida pelo corte de bobinas-mestre + estoque do período anterior.  

2. **Capacidade da cortadeira (3)**:  
   $$
   \sum_{k=1}^{K} \sum_{m=1}^{M} \sum_{j=1}^{n_m} \left( c_{jkm} y_{jkmt} + p_{jkm} s_{jkmt} \right) \leq \text{Cap}_t^c, \quad \forall t
   $$
   **Explicação**: Tempo de corte ($c_{jkm}$) e setup ($p_{jkm}$) não podem exceder a capacidade da cortadeira no período $t$.  

3. **Relacionamento setup-corte (4)**:  
   $$
   y_{jkmt} \leq Q_1 s_{jkmt}, \quad \forall j, k, m, t
   $$
   **Explicação**: O corte só ocorre se a cortadeira estiver preparada para o padrão $j$ ($s_{jkmt} = 1$). $Q_1$ é um número grande para forçar a ativação da variável de setup.  

4. **Capacidade de produção (5)**:  
   $$
   \sum_{k=1}^{K} \left( b_{km} x_{kmt} + f_{km} z_{kmt} \right) \leq \text{Cap}_{mt}^p, \quad \forall m, t
   $$
   **Explicação**: Peso das bobinas produzidas ($b_{km} x_{kmt}$) + perda de setup ($f_{km} z_{kmt}$) não excedem a capacidade da máquina de papel.  

5. **Relacionamento setup-produção (6)**:  
   $$
   x_{kmt} \leq Q_2 z_{kmt}, \quad \forall k, m, t
   $$
   **Explicação**: Produção de bobinas-mestre só ocorre se houver setup ($z_{kmt} = 1$). $Q_2$ é um número grande.  

6. **Balanço de estoque (7)**:  
   $$
   \sum_{j=1}^{n_m} y_{jkmt} = x_{kmt} + w_{kmt-1} - w_{kmt}, \quad \forall k, m, t
   $$
   **Explicação**: Bobinas-mestre cortadas ($y_{jkmt}$) devem ser iguais às produzidas ($x_{kmt}$) + estoque anterior ($w_{kmt-1}$) menos estoque atual ($w_{kmt}$).  

#### **Subproblema da Mochila (12-14)**:  
Usado na geração de colunas para identificar padrões de corte atrativos:  
$$
\max \left( \sum_{i=1}^{N} \pi_i \alpha_i \right) \quad \text{sujeito a} \quad \sum_{i=1}^{N} l_i \alpha_i \leq L_m, \quad \alpha_i \in \mathbb{Z}_+  
$$
**Explicação**: Maximiza a utilidade ($\pi_i$: preços duais das restrições de demanda) do padrão $\alpha_i$ (quantidade de itens $i$ cortados), respeitando a largura máxima $L_m$ da bobina-mestre.  

---

### Conclusão do Modelo  
O modelo proposto avança ao integrar **setup na cortadeira** e **capacidade limitada** ao problema combinado de dimensionamento de lotes e corte multiperíodo. A aplicação da geração de colunas mostrou-se eficiente para obter soluções próximas do ótimo com esforço computacional viável, validando sua utilidade para decisões industriais complexas. Contudo, os autores planejam estender o modelo para cenários estocásticos, incorporando incertezas nos parâmetros.
