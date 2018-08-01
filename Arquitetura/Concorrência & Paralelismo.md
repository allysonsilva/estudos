## Concorrência

É o conceito de tarefas múltiplas iniciando, executando e concluindo dentro do mesmo período de tempo. Isso não significa necessariamente que as tarefas estão sendo executadas simultaneamente. Na verdade, para que as tarefas sejam executadas simultaneamente, nosso aplicativo precisa ser executado em um sistema multicore ou multiprocessador. Concorrência nos permite compartilhar o processador ou núcleos para várias tarefas; no entanto, um único núcleo só pode executar uma tarefa em um determinado momento.

## Paralelismo

É o conceito de duas ou mais tarefas executadas simultaneamente. Uma vez que cada núcleo do nosso processador só pode executar uma tarefa por vez, o número de tarefas que executam simultaneamente é limitado ao número de núcleos dentro dos nossos processadores e ao número de processadores que possuímos. Como exemplo, se possuímos um processador de quatro núcleos, estamos limitados a executar quatro tarefas simultaneamente. Os processadores de hoje podem executar tarefas tão rapidamente que pode parecer que tarefas maiores estão sendo executadas simultaneamente. No entanto, dentro do sistema, as tarefas maiores estão realmente se revezando, executando sub-tarefas nos núcleos.
