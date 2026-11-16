# AgendaIA — Bloco 3
## Implementação e validação do protótipo

**Disciplina:** Inteligência Artificial  
**Etapa:** Bloco 3 — implementação, demonstração e avaliação  
**Protótipo:** [AgendaIA](./index.html)

## 1. Contexto e objetivo

O AgendaIA foi proposto para apoiar a organização da rotina quando há tarefas com prioridades, durações e prazos diferentes, além de compromissos que não podem ser movidos. O objetivo deste bloco é demonstrar uma prova de conceito que monte uma agenda diária, preserve os compromissos fixos e permita comparar duas estratégias de busca estudadas no projeto: busca gulosa e A*.

O protótipo é demonstrativo: os compromissos e as tarefas iniciais são exemplos editáveis, não dados coletados de usuários.

## 2. Requisitos atendidos

| Código | Requisito | Como foi demonstrado |
|---|---|---|
| RF01 | Cadastrar tarefas com duração, prioridade e prazo | Formulário da página |
| RF02 | Considerar compromissos fixos | Aula, almoço e reunião bloqueiam horários |
| RF03 | Evitar sobreposição e respeitar a janela diária | Planejamento entre 08:00 e 18:00 em blocos de 30 minutos |
| RF04 | Sinalizar tarefas que não cabem antes do prazo | Lista de tarefas não agendadas |
| RF05 | Comparar busca gulosa e A* | Comparação de tarefas agendadas, custo e estados explorados |
| RNF01 | Explicar o resultado | A agenda mostra horários, prioridade, prazo e compromissos |
| RNF02 | Ser executável sem instalação | Aplicação local em um navegador, sem bibliotecas externas |

## 3. Modelagem e algoritmos

O dia é representado por 20 intervalos de 30 minutos, das 08:00 às 18:00. Cada tarefa tem duração em múltiplos de 30 minutos, prazo máximo de conclusão e prioridade: alta (peso 3), média (peso 2) ou baixa (peso 1). Os compromissos fixos são tratados como horários indisponíveis.

### Busca gulosa

1. Ordena tarefas por prioridade decrescente, prazo mais próximo e menor duração.
2. Para cada tarefa, procura o primeiro intervalo livre que comporte toda a duração e termine até o prazo.
3. Se não houver espaço válido, mantém a tarefa na lista de não agendadas.

É uma estratégia rápida, mas toma decisões locais e não compara outras ordens possíveis.

### Busca A*

O estado da busca contém as tarefas ainda não decididas, os horários ocupados e as decisões já tomadas. Para cada tarefa restante, a busca considera agendá-la no primeiro intervalo válido ou deixá-la pendente. Assim, explora ordens diferentes sem deslocar tarefas para mais tarde quando já existe um horário livre anterior.

O custo utilizado é:

`g = soma(peso da prioridade × horário de conclusão) + 10.000 × soma(peso das tarefas não agendadas)`

Os horários são medidos em intervalos desde o início do dia. A heurística soma, para cada tarefa restante, seu peso multiplicado pela duração mínima. Esse valor ignora conflitos e compromissos, portanto é um limite inferior otimista. A busca prioriza estados pelo menor `g + h`. O limite alto para tarefas pendentes faz o protótipo preferir agendar tarefas viáveis.

Para manter a demonstração responsiva, o protótipo limita cada planejamento a oito tarefas. Essa é uma limitação de escopo, não uma recomendação para o produto final.

## 4. Demonstração

No cenário inicial, a A* agendou as quatro tarefas sem conflito com os compromissos fixos:

| Horário | Atividade |
|---|---|
| 08:00–09:00 | Finalizar roteiro do projeto |
| 09:00–10:00 | Aula — compromisso fixo |
| 10:00–11:00 | Revisar referências |
| 11:00–12:30 | Preparar slides da apresentação |
| 12:30–13:30 | Almoço — compromisso fixo |
| 13:30–14:00 | Responder e-mails acadêmicos |
| 15:00–15:30 | Reunião de projeto — compromisso fixo |

Resultado da comparação para **esse cenário de exemplo**:

| Estratégia | Agendadas | Custo ponderado | Estados explorados |
|---|---:|---:|---:|
| Gulosa | 4 de 4 | 57 | 4 |
| A* | 4 de 4 | 57 | 31 |

As duas estratégias chegaram ao mesmo custo nesse conjunto de dados. A A* explorou mais estados; portanto, o exemplo demonstra a diferença de estratégia e custo computacional, não uma superioridade universal da A*.

## 5. Testes funcionais

Os testes locais integrados ao protótipo foram executados no navegador; **5 de 5 passaram**.

| Teste | Resultado |
|---|---|
| Tarefa não é colocada sobre compromisso fixo | Aprovado |
| A* agenda tarefa viável antes do prazo | Aprovado |
| Prazo impossível deixa tarefa explicitamente pendente | Aprovado |
| Agenda sem tarefas não causa erro | Aprovado |
| Tarefas agendadas não se sobrepõem | Aprovado |

Os testes verificam regras essenciais da prova de conceito. Ainda não são uma avaliação com usuários nem uma comparação experimental ampla de desempenho.

## 6. Limitações e próximos passos

- O horizonte é de um único dia; não há planejamento semanal, recorrência ou dependências entre tarefas.
- A duração precisa ser múltipla de 30 minutos e há limite de oito tarefas por execução.
- Os compromissos fixos são exemplos do cenário e não são cadastráveis nesta versão.
- Não há persistência, autenticação, integração com calendários nem sincronização entre dispositivos.
- A prioridade é informada pela pessoa usuária; não há aprendizado automático de preferências.
- A prova de conceito foi implementada em HTML e JavaScript para funcionar localmente no navegador. Se a entrega final exigir a stack planejada no bloco anterior, como Python/Streamlit, os algoritmos e a interface deverão ser portados e testados nessa stack.
- Como próxima etapa, recomenda-se permitir o cadastro de compromissos, salvar agendas, testar mais cenários e conduzir uma avaliação de usabilidade com usuários voluntários.

## 7. Conclusão

A prova de conceito demonstra a ideia central do AgendaIA: organizar tarefas em torno de compromissos fixos, respeitando duração, prioridade e prazo. A comparação evidencia o compromisso entre simplicidade e exploração: a busca gulosa examinou menos estados no cenário demonstrativo, enquanto a A* avaliou mais alternativas e encontrou o mesmo custo. A implementação atende ao escopo de demonstração deste bloco e explicita os limites que precisam ser tratados antes de uma versão integrada ao uso real.

## Referências

- **Contrato Pedagógico — Inteligência Artificial.** Material fornecido para a disciplina.
- **AgendaIA — Bloco 2.** Documento do projeto fornecido pelo grupo.
