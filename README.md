# 🍷 UWine – Clube de Vinhos da UVV | Projeto de Ciência de Dados (TCC)

Projeto de Conclusão de Curso em **Ciência de Dados** – Universidade Vila Velha (UVV).
**Autor:** Ícaro Peroba 

Análise completa dos dados de clientes da **UVVine**, um clube de vinhos fictício, a partir de **1.120.000 notas fiscais** (TABLE 29). O projeto vai da amostragem estatística ao aprendizado de máquina.

| | Link |
|---|---|
| 📓 Colab – Parte 1 | [Abrir no Colab](https://colab.research.google.com/drive/16IlolpPDtJVHnVazfzRDKYhbApix1JAg?usp=sharing) |
| 📓 Colab – Parte 2 | [Abrir no Colab](https://colab.research.google.com/drive/1rLWrbC8ohe_logSa5v1iEKzaPjp2Ku8J?usp=sharing) |

---

## 📁 Estrutura do repositório

```
uvvine-data-science/
├── README.md
├── parte1_estatistica/
│   ├── TCC_Parte1_corrigido.ipynb     # notebook executado (com saídas)
│   └── TCC_Parte1_corrigido.pdf       # versão para leitura/impressão
├── parte2_machine_learning/
│   ├── TCC_Parte2.ipynb               # notebook executado (com saídas)
│   └── TCC_Parte2.pdf
└── dados/
    ├── train.csv                      # 5.300 linhas – gerado na Parte 1
    ├── validation.csv                 # 5.300 linhas – gerado na Parte 1
    └── test.csv                       # 5.300 linhas – gerado na Parte 1
```

> **Base completa (`table29.csv`, ~400 MB):** não está no repositório, porque o GitHub limita arquivos a 100 MB. Ela é fornecida pela disciplina e deve ser colocada na mesma pasta do notebook para executá-lo.

---

## 🧭 Workflow

### Parte 1 – Estatística e amostragem
Objetivo: determinar o **TAMANHO IDEAL da amostra** e caracterizar os clientes.

| Atividade | O que foi feito |
|---|---|
| **A1.1** Tamanho ideal | Estabilização marginal do erro-padrão (4 curvas bootstrap: ESSENTIAL, VIP, PRIME, GERAL) e erro-padrão estratificado com **otimização de Neyman** sobre `TOTAL (R$)` |
| **A1.2** Outliers | Regra do IQR global e **por tipo de conta** |
| **A1.3** Testes de hipóteses | Normalidade (Anderson-Darling), independência (Pearson/Spearman/Kendall + bootstrap), mesma distribuição (KS, AD k-amostras, Mann-Whitney, Kruskal-Wallis) e médias (ANOVA, Welch, bootstrap, TOST) |
| **A2** Estatística descritiva | Variáveis quantitativas (posição, dispersão, forma, valores químicos negativos) e qualitativas (região, sexo, estado civil, conta) |
| **A3** Pesquisa de satisfação | Discretização das 25 opiniões em 5 níveis e **bootstrap das proporções** (geral e por sexo) |

**Principais resultados**
- ✅ **Tamanho ideal: n = 5.300** notas, estratificadas por tipo de conta (3.390 ESSENTIAL, 1.120 VIP, 790 PRIME). A margem de erro da média de gasto fica em **1,65%**.
- ✅ As amostras independentes não diferem em distribuição nem em média, o que confirma a representatividade.
- ✅ Índice de satisfação (Satisfeito + Muito Satisfeito): **80,5%** (IC95%: 79,4% a 81,6%), sem diferença significativa entre os sexos.
- ✅ Geração dos conjuntos **train / validation / test** (5.300 linhas cada, disjuntos) para a Parte 2.

### Parte 2 – Machine Learning (scikit-learn)

| Modelo | Pergunta de negócio | Algoritmo escolhido | Desempenho (test) |
|---|---|---|---|
| **Regressão** | Quanto um cliente vai gastar? | Lasso (melhor de 7 regressores) | R² = 0,754 |
| **Clusterização** | Existem segmentos naturais de clientes? | K-Means, k = 3 (cotovelo + silhueta) | Silhueta 0,63 · ARI 0,93 vs. planos |
| **Classificação** | Qual plano é adequado ao perfil? | Linear SVM (melhor de 10 classificadores) | Acurácia 97,3% |

Cada modelo traz validação cruzada, intervalo de confiança bootstrap, avaliação em train, validation e test, e um teste final numa **amostra aleatória inédita da população**.

**Principais insights**
- 💰 A **renda** é o principal fator do negócio: explica cerca de 75% do gasto e determina o plano com ~97% de acerto.
- 🎯 Os planos **ESSENTIAL, VIP e PRIME correspondem a segmentos reais**: o K-Means os reencontrou sem conhecer os rótulos.
- 🔁 Os clientes "fora do perfil" do seu plano formam uma lista acionável para **upgrade, downgrade ou retenção**.
- 😊 A satisfação (~80%) não depende do segmento financeiro.

---

## ▶️ Como executar

1. Abra o notebook no [Google Colab](https://colab.research.google.com/) (**Arquivo > Fazer upload de notebook**).
2. Envie os dados para a aba **Arquivos** do Colab:
   - **Parte 1:** `table29.csv`
   - **Parte 2:** `train.csv`, `validation.csv`, `test.csv` e `table29.csv`
3. Execute **Ambiente de execução > Executar tudo**. Todos os sorteios usam semente fixa (`SEED = 29`), então os resultados são reprodutíveis.

**Bibliotecas:** `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`, `scikit-learn`, todas já disponíveis no Colab.

---

## 📚 Referência

Buitinck, L., Louppe, G., Blondel, M., Pedregosa, F., Mueller, A., Grisel, O., ... & Varoquaux, G. (2013). *API design for machine learning software: experiences from the scikit-learn project.* ECML PKDD Workshop: Languages for Data Mining and Machine Learning, 108–122.

> Projeto acadêmico com dados simulados. A UVVine é uma empresa fictícia.
