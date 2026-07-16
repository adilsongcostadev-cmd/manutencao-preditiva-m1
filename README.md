# 🏭 PredMaq — Sistema Preditivo de Falhas em Máquinas Industriais

> Projeto Avaliativo - Módulo 1 | Curso Desenvolvimento de IA para Análise Preditiva (T1)
> Programa SCTEC / SENAI-SC - 2026

## 🎥 Vídeo de apresentação

📌 [Link do vídeo de apresentação do projeto](COLOCAR_LINK_AQUI)

## 📌 O problema que o PredMaq resolve

Um parque fabril monitorado por sensores (contexto Indústria 4.0) precisa **antecipar
quebras mecânicas** nos equipamentos para evitar paradas não planejadas na linha de
produção. O PredMaq é um pipeline completo de Ciência de Dados que aprende, a partir do
histórico de 10.000 registros de sensores, a prever a variável binária `falha_maquina`
(1 = falha | 0 = operação normal), comparando dois algoritmos de classificação: **KNN**
e **Árvore de Decisão**.

## 🏆 Resultado final

| Modelo | Configuração | Acurácia no Teste |
|---|---|---|
| KNN | K=3 (dados escalonados) | 90,70% |
| **Árvore de Decisão** | **max_depth=None** | **93,85%** 🏆 |

**Modelo recomendado: Árvore de Decisão**, com vantagem de 3,15 pontos percentuais,
regras interpretáveis nas unidades reais dos sensores e predição praticamente instantânea.

## 🔬 Técnicas aplicadas (pipeline em 7 fases)

1. **EDA** - dimensões, tipos, estatística descritiva e 3 visualizações (distribuições,
   desbalanceamento da classe alvo de 3,39% e mapa de calor de correlações)
2. **Data Prep** - remoção de duplicados, imputação de 500 nulos/coluna pela **mediana**
   (justificada pela assimetria das distribuições) e análise de outliers via boxplot (Tukey)
3. **Feature Engineering** - criação da variável `potencia = velocidade_rotacao_rpm × torque_nm`,
   com fundamento físico
4. **Divisão e Balanceamento** - split 80/20 estratificado e **SMOTE aplicado somente no
   treino**, prevenindo Data Leakage; exclusão das colunas de causa de falha (leakage)
5. **Escalonamento** - StandardScaler apenas para o KNN (`fit_transform` no treino,
   `transform` no teste); Árvore com dados originais (imune à escala)
6. **Ajuste de hiperparâmetros** - K ∈ {3, 5, 7} e max_depth ∈ {3, 5, None}, com
   diagnóstico de overfitting comparando acurácias de treino × teste
7. **Veredito Final** - comparação dos melhores modelos no conjunto de teste

## 🛠️ Tecnologias

- **Python 3.14** | pandas · numpy · matplotlib · seaborn · scikit-learn · imbalanced-learn
- **Jupyter Notebook** (VS Code)
- **Git/GitHub** com fluxo de feature branches (main ← develop ← feature/*)

## 📂 Estrutura do repositório

manutencao-preditiva-m1/
├── data/
│   └── manutencao_preditiva.csv      # base de dados (10.000 registros, 14 colunas)
├── notebook_manutencao_preditiva.ipynb  # pipeline completo (Fases 1 a 7)
├── requirements.txt                  # dependências com versões fixadas
└── README.md

## ▶️ Como executar

1. Clone o repositório:

git clone https://github.com/adilsongcostadev-cmd/manutencao-preditiva-m1.git
cd manutencao-preditiva-m1

2. Instale as dependências:

pip install -r requirements.txt

3. Abra o `notebook_manutencao_preditiva.ipynb` (VS Code ou Jupyter) e execute todas as
   células em ordem (**Run All**). O caminho dos dados é relativo (`data/`) e a semente
   `random_state=42` garante a reprodução exata dos resultados.

## 🚀 Melhorias futuras

- Incorporar **recall, F1-score e matriz de confusão**: em manutenção preditiva, o custo de
  um falso negativo (falha não detectada) supera o de um alarme falso - a acurácia isolada
  é insuficiente em bases desbalanceadas
- Incluir a variável categórica `tipo` via one-hot encoding
- Testar modelos de ensemble (Random Forest, Gradient Boosting) e validação cruzada
- Evoluir para deploy do modelo via API (escopo do Módulo 2 do curso)

---
*Desenvolvido por Adilson Guimarães Costa — [LinkedIn](https://linkedin.com/in/adilsongcosta)*