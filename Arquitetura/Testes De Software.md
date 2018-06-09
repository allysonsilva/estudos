# Teste de Software

> Teste de Software é um processo que faz parte do desenvolvimento de software, e tem como principal objetivo revelar falhas/bugs para que sejam corrigidas até que o produto final atinja a qualidade desejada/acordada.

## Teste de Unidade

- Teste em um nível de componente ou classe. É o teste cujo objetivo é um “pedaço do código”. Como o nome sugere, este método analisa o nível do objeto. Os componentes de software individuais são testados quanto a erros. O conhecimento do programa é necessário para este teste e os códigos de teste são criados para verificar se o software se comporta como se destina.

- Os testes de unidade não deve ter dependências em sistemas externos.

- Um teste de unidade é um teste escrito por um programador para verificar se um pedaço de código relativamente pequeno está fazendo o que se destina a fazer.

- Visa avaliar pequenas unidades que compõem um software, responsáveis por funções diferentes dentro dele. Podem ser avaliados códigos, sub-rotinas entre outros. O foco aqui é descobrir se todas essas partes estão funcionando adequadamente.

- Um teste de unidade é realizado em uma unidade autônoma (geralmente uma classe ou método) e deve ser executado sempre que uma unidade foi implementada ou a atualização de uma unidade foi concluída. Isso significa que ele é executado sempre que você escreveu uma classe/método, corrigiu um bug, alterou a funcionalidade.

## Teste de Integração

- Garante que um ou mais componentes combinados (ou unidades) funcionam. Podemos dizer que um teste de integração é composto por diversos testes de unidade.

- Um teste de integração é feito para demonstrar que diferentes partes do sistema funcionam juntas. Os testes de integração abrangem aplicações inteiras, e eles exigem muito mais esforço para montar. Eles geralmente exigem recursos como instâncias de banco de dados e hardware para serem alocados para eles. Os testes de integração fazem um trabalho mais convincente de demonstrar o funcionamento do sistema (especialmente para não programadores) do que um conjunto de testes unitários podem, pelo menos na medida em que o ambiente de teste de integração se assemelhe à produção.

- Testes isolados que podem mudar o estado do sistema, por exemplo: salvar em banco, escrever em arquivo... Um teste de integração não representa por si um requisito funcional. Eles verificam a integração do nosso código com uma ferramenta de terceiros ou com diferentes camadas de nosso próprio código, por exemplo: a camada de lógica de negócio requer a camada de acesso a dados.

- Ao colocar várias unidades juntas que interagem você precisa realizar testes de integração para garantir que a integração dessas unidades em conjunto não tenha introduzido nenhum erro.

- Teste de integração é a fase do teste de software em que módulos são combinados e testados em grupo. Ela sucede o teste de unidade, em que os módulos são testados individualmente, e antecede o teste de sistema, em que o sistema completo (integrado) é testado num ambiente que simula o ambiente de produção.

- O teste de integração é alimentado pelos módulos previamente testados individualmente pelo teste de unidade, agrupando-os assim em componentes, como estipulado no plano de teste, e resulta num sistema integrado e preparado para o teste de sistema.

- O propósito do teste de integração é verificar os requisitos funcionais, de desempenho e de confiabilidade na modelagem do sistema. Com ele é possível descobrir erros de interface entre os componentes do sistema.

- Teste de Integração visa testar quão bem várias unidades interagem uns com os outros. Este tipo de teste deve ser realizado Sempre que uma nova forma de comunicação tenha sido estabelecida entre unidades ou a natureza de sua interação tenha mudado. Isso significa que ele é executado sempre que uma unidade escrita recentemente é integrada no resto do sistema ou sempre que uma unidade que interage com outros sistemas foi atualizada (e concluiu com sucesso seus testes de unidade).

- Além disso, outra coisa que diferencia testes de integração de testes unitários é o ambiente. Os testes de integração podem e usarão threads, acessam o banco de dados ou fazem o que for necessário para garantir que todo o código e as diferentes mudanças no ambiente funcionem corretamente. A principal vantagem é que eles encontrarão erros que os testes de unidade não podem encontrar por conta da integração entre diferentes partes(Banco de dados, filas, threads, etc...).

## Teste de Aceitação

- Usado para validar e verificar se o produto desenvolvido está de acordo com o que foi estabelecido nos requisitos. Esses testes são construídos a partir de situações de uso do sistema, estabelecendo valores de entrada, saída, tempo de resposta, etc. O objetivo do TDA é garantir que o sistema seja capaz de executar a funcionalidade acordada da forma desejada, e, consequentemente, garantir a qualidade do produto em desenvolvimento.

- Os testes de aceitação envolvem testar uma funcionalidade do início ao fim, dando entradas para o sistema e observando o comportamento de saída. O resultado deve estar compatível com os requisitos do sistema em termos de tempo de resposta, validade do resultado, facilidade de uso e qualquer outro critério que o cliente tenha estabelecido.

- O teste de aceitação é particularmente importante porque sua execução é feita na própria interface do software, validando o sistema para o cliente da maneira que ele será usado.

- Você deve testar que o programa funciona da maneira que um usuário/cliente espera que o aplicativo funcione. testes de aceitação garantir que a funcionalidade atende aos requisitos de negócios.

- Os testes de aceitação asseguram que um recurso ou caso de uso seja corretamente implementado. É semelhante a um teste de integração, mas com foco no caso de uso e não nos componentes envolvidos. Há muitas maneiras de realizar testes de aceitação. O cliente ou usuário pode testar manualmente o aplicativo para garantir que atenda a cada um dos requisitos da empresa. Ou você pode usar ferramentas de teste automatizadas, como Cucumber e Selenium. Os testes de aceitação atuam como a verificação final da funcionalidade comercial necessária e do bom funcionamento do sistema, emulando as condições de uso do mundo real em nome do cliente pagador ou de um grande cliente.

- Os testes de aceitação são realizados sempre que é relevante para verificar se um subsistema (possivelmente todo o sistema) cumpre suas especificações inteiras. Isso significa que ele é executado principalmente antes de terminar uma nova entrega ou anunciar a conclusão de uma tarefa maior.

- Este é o último teste que é realizado antes do software ser entregue ao cliente. Ele é realizado para garantir que o software desenvolvido atenda a todos os requisitos do cliente. Existem dois tipos de testes de aceitação - um que é realizado pelos membros da equipe de desenvolvimento, conhecido como teste de aceitação interna (teste Alpha) e o outro que é realizado pelo cliente ou usuário final conhecido como (Teste Beta)
