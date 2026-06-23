# Sistema Inteligente para Controle de Temperatura e Climatização (HVAC)
### Integração entre Machine Learning e Lógica Fuzzy

**Disciplina:** Inteligência Artificial  
**Professor:** William Malvezzi  
**Curso:** Engenharia de Software — Universidade Católica de Brasília (UCB)  
**Grupo:** Adenilson dos Reis Alves · Luan Henrique de Souza Ferreira · Natália Jesus Barbosa Cruzeiro · Pedro Paulo Sousa do Lago · Sara Cristina Barros de Oliveira · Tiago Assis Schulz · Victor Davidson Bomfim da Cunha

---

## Sobre o projeto

Este trabalho implementa um controlador inteligente para sistemas de ar-condicionado e ventilação (HVAC). A solução combina duas abordagens de IA:

- **Machine Learning (Árvore de Decisão):** lê os sensores internos do ambiente (temperatura, umidade e ocupação) e estima o Índice de Conforto do Usuário.
- **Lógica Fuzzy (scikit-fuzzy):** recebe essa estimativa e a temperatura externa e calcula, via 9 regras linguísticas e defuzzificação por centroide, a potência ideal do equipamento (0–100%).

---

## Estrutura do repositório

```
├── Trabalho_Final_IA_Controle_HVAC_Integrado.ipynb   ← Notebook principal (Google Colab)
├── HVAC_Dynamic_Fuzzy_PID_2017_with_Target.csv        ← Base de dados (10.000 registros)
├── Relatorio_Final_HVAC_ABNT.docx                     ← Relatório técnico formatado em ABNT
└── README.md                                           ← Este arquivo
```

---

## Base de dados

| Item | Detalhe |
|---|---|
| Fonte | Kaggle — [HVAC Sensor Data for Dynamic Fuzzy PID Control](https://www.kaggle.com/datasets/ziya07/hvac-sensor-data-for-dynamic-fuzzy-pid-control) |
| Arquivo | `HVAC_Dynamic_Fuzzy_PID_2017_with_Target.csv` |
| Registros | 10.000 |
| Atributos utilizados como entrada | `Temperature_C`, `Humidity_%`, `Occupancy_Count` |
| Variável-alvo | `User_Comfort_Index` (valores: 6, 7, 8 ou 9) |
| Valores ausentes | Nenhum |

---

## Como executar

### Pré-requisito
Conta Google para acessar o Google Colab (gratuito).

### Passo a passo

**1. Abrir o notebook**

Acesse o link do notebook no Google Colab:  
👉 https://colab.research.google.com/drive/1FETZnZ7jjHJT5nwkxU3hc4ET28SQI6NM?usp=sharing

**2. Fazer upload da base de dados**

No painel esquerdo do Colab, clique no ícone de **pasta** (📁) e depois em **"Fazer upload para armazenamento de sessão"**. Selecione o arquivo:

```
HVAC_Dynamic_Fuzzy_PID_2017_with_Target.csv
```

> ⚠️ O upload precisa ser feito toda vez que a sessão do Colab for reiniciada, pois o armazenamento da sessão é temporário.

**3. Executar as células em ordem**

Execute as células **de cima para baixo**, na sequência:

| Célula | O que faz |
|---|---|
| Célula 1 | Instala a biblioteca `scikit-fuzzy` e importa os pacotes |
| Célula 2 | Carrega o CSV e exibe as primeiras linhas |
| Célula 3 | Treina o modelo de Árvore de Decisão (80% treino / 20% teste) |
| Célula 4 | Define as variáveis fuzzy e as funções de pertinência |
| Célula 5 | Monta a base de 9 regras de inferência |
| Célula 6 | Executa o pipeline integrado no cenário de teste (29 °C, 70% umidade, 18 pessoas, 34 °C externo) |
| Célula 7 | Plota as funções de pertinência com os valores ativados |
| Célula 8 | Gera a superfície de controle 3D |

Para executar uma célula: clique nela e pressione **`Ctrl + Enter`** (ou clique no botão ▶ à esquerda da célula).

Para executar todas de uma vez: menu **Ambiente de execução → Executar tudo** (`Ctrl + F9`).

**4. Resultado esperado na Célula 6**

```
[ML] Índice de Conforto Estimado pelo modelo: 6
[FUZZY] Potência de atuação calculada para o ar-condicionado: 35.00%
```

---

## Dependências

Todas instaladas automaticamente pela Célula 1 no Colab. Para execução local, instale manualmente:

```bash
pip install scikit-fuzzy scikit-learn pandas numpy matplotlib
```

Versão do Python recomendada: **3.10 ou superior**.

---

## Observação sobre as métricas do modelo

A Árvore de Decisão obteve acurácia de **24,55%** no conjunto de teste, próxima ao nível do acaso para um problema de 4 classes (~25%). Isso se deve à correlação praticamente nula entre os atributos de sensores e o índice de conforto nesta base — limitação discutida em detalhes na seção 5.3 do relatório técnico. O cenário demonstra adequadamente o funcionamento do mecanismo fuzzy, independentemente da limitação preditiva do ML.
