# Semáforo Adaptativo com Q-learning e DQN

Projeto para avaliar se agentes de aprendizado por reforço (Q-learning tabular e DQN) reduzem as filas e o tempo de espera em um cruzamento semaforizado frente a dois controladores tradicionais: ciclo fixo ajustado e maior fila.

## Como funciona a simulação

O ambiente segue a API do Gymnasium para modelar um cruzamento com duas filas, norte-sul e leste-oeste:

- **Tempo:** passos de 1 segundo em episódios de 600 segundos.
- **Sinais:** verde entre 5 e 30 segundos, com transição fixa de 2 segundos de amarelo e 1 segundo de vermelho geral sem saída de veículos.
- **Atendimento:** até 1 veículo por segundo na direção aberta.
- **Ações:** manter a fase atual ou pedir troca, respeitando os tempos mínimo, máximo e de transição.
- **Estado e recompensa:** o agente observa o tamanho das duas filas, a fase atual e o tempo nessa fase, e recebe como recompensa a soma negativa das filas a cada segundo.

## Cenários de tráfego

Os testes cobrem três cenários com chegadas independentes de veículos por segundo:

| Cenário | Norte-Sul | Leste-Oeste |
| --- | ---: | ---: |
| Equilibrado | 0,30 | 0,30 |
| Predomínio norte-sul | 0,50 | 0,15 |
| Predomínio leste-oeste | 0,15 | 0,50 |

## Métodos comparados

- **Ciclo fixo ajustado:** testa combinações de verde entre 5 e 30 segundos na validação e fixa a melhor por cenário.
- **Maior fila:** troca a fase sempre que a fila no vermelho supera a do verde dentro dos limites de tempo.
- **Q-learning:** versão tabular esparsa sobre o estado exato.
- **DQN:** rede neural em PyTorch com replay buffer e rede-alvo separada.

## Estrutura do projeto

```text
├── README.md
├── requirements.txt
├── simulador.py        # Ambiente Gymnasium, cenários e métricas
├── agentes.py          # Ciclo fixo, maior fila, Q-learning e DQN
├── experimentos.py     # Treino, validação, teste, gráficos e exportação
├── testes.py           # Testes automatizados do simulador e agentes
├── resultados/         # Dados consolidados, modelos e gráficos
├── painel/             # Painel offline em HTML, CSS e JavaScript
└── entregas/           # Relatório em PDF e slides da apresentação
```
