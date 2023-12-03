# Sistemas Operacionais

## Introdução

Em um sistema computacional, o software atua como intermediário entre a camada do hardware e as aplicações. O processador, a memória e os dispositivos de E/S são interligados por meio de um único barramento.
O barramento de Von Neumann é responsável por transmitir dados, endereços e sinais de controle. O software é dividido em aplicações, que representam o propósito final, e a camada intermediária entre o hardware e o software, que é o sistema operacional.
O sistema operacional tem a função de gerenciar os recursos do sistema. Sua primeira tarefa é compartilhar esses recursos entre as aplicações que estão em execução, o que pode ocorrer de duas maneiras: compartilhamento no espaço e no tempo.

No que se refere à memória, o compartilhamento ocorre no espaço. O sistema operacional decide em quais áreas de memória os aplicativos que desejamos executar serão alocados, garantindo que cada aplicação seja alocada em locais distintos.
No caso da CPU, o compartilhamento ocorre no tempo. O sistema operacional coloca em execução um programa ou outro aplicativo e, em seguida, a CPU pode retornar ao programa que estava em execução antes do programa atual, ou seja, aquele que ganhou a CPU.

Outra função crucial do sistema operacional é impedir que uma aplicação leia ou escreva em uma área de memória destinada a outra aplicação em execução. Para garantir a proteção desses dados, o sistema operacional precisa implementar mecanismos de segurança. Geralmente, isso é realizado em nível de hardware, mas o sistema operacional deve programá-lo de maneira que a proteção dos dados seja efetiva durante a execução dos programas.
Além disso, o sistema operacional deve proteger os dispositivos de entrada e saída.

Cada dispositivo de E/S possui uma controladora, e é o sistema operacional que controla o software que é executado na CPU dessas controladoras. Isso é feito de forma protegida para evitar que uma aplicação interfira na outra ao escrever nessas controladoras.

Uma das responsabilidades do sistema operacional é estender a interface de programação por meio de abstrações e operações nessas abstrações. 
As principais abstrações do sistema operacional são os arquivos e os processos, que facilitam o desenvolvimento de aplicações e que as aplicações interajam com o hardware de forma simplificada. 
Os processos controlam a execução dos programas, enquanto os arquivos permitem o acesso aos dispositivos e operações de entrada e saída de dados. 
As operações que implementam essas abstrações são chamadas de chamadas de sistema (System Call).

O hardware possui uma interface de programação específica, que consiste em um conjunto de instruções de máquina da arquitetura do processador em uso. Por outro lado, o sistema operacional fornece uma API de nível mais alto, conhecida como conjunto de chamadas de sistema do sistema operacional.

O software desenvolvido na camada de aplicações utiliza interfaces com abstrações de nível mais alto, como os arquivos e os processos, em vez de utilizar a interface de baixo nível. Essas abstrações são implementadas por meio das chamadas de sistema. Uma chamada de sistema é um mecanismo pelo qual um programa de computador solicita um serviço do núcleo do sistema operacional em que está sendo executado. Isso pode incluir serviços relacionados ao hardware, criação e execução de novos processos e comunicação com os serviços do núcleo de maneira integral.

## Como funciona a execução de um sistema computacional 

Ao ligar o sistema computacional, a primeira coisa a ser executada é um programa embutido no hardware, que é a BIOS, responsável por realizar um diagnóstico do hardware. O PC (Program Counter), que é um registrador que indica qual será a próxima instrução a ser executada, aponta para o software ou programa armazenado no firmware (BIOS). A BIOS realiza uma verificação do hardware e carrega um programa chamado de bootloader. O bootloader, por sua vez, é responsável por carregar o sistema operacional do disco para a memória.

Em seguida, o PC passa a apontar para as instruções desse sistema operacional, dando início à execução do código do sistema operacional. O sistema operacional realiza algumas verificações adicionais e, em seguida, inicia a execução de aplicativos que funcionam como serviços do sistema operacional. Dessa forma, o sistema operacional está pronto para executar as aplicações do usuário.

Para executar uma aplicação, como um navegador, uma parte dela é carregada do disco para a memória e o PC começa a apontar para esse código. Assim, quando o sistema operacional modifica o valor do PC, o navegador passa a ser executado em vez do sistema operacional, permitindo que a aplicação seja executada.

## Temporizador (Timer)

O temporizador é um hardware programável, no qual o sistema operacional programa intervalos de tempo bem definidos para gerar interrupções. Esse temporizador está conectado ao barramento e envia um sinal de interrupção para o barramento de controle, que é recebido pela CPU e tratado pelo controlador de interrupção programável (PIC).
Quando a interrupção chega ao controlador de interrupção programável (PIC), o primeiro passo é aguardar que a CPU termine a execução da instrução em andamento. Em seguida, o PIC direciona o apontador do PC (Program Counter) para um código armazenado em uma área de memória do sistema operacional conhecida como vetor de interrupções.

O vetor de interrupções, localizado na memória do sistema operacional, contém endereços de rotinas específicas que tratam interrupções. Quando ocorre uma interrupção, o PIC espera a conclusão da instrução atual e, em seguida, faz com que o apontador de pilha aponte para a pilha do sistema operacional. Em seguida, o PC é direcionado para o endereço contido no vetor de interrupção correspondente à interrupção do temporizador, executando assim o código do sistema operacional responsável por tratar a interrupção do temporizador.

Dessa forma, o código do sistema operacional que lida com as interrupções de tempo de execução é executado. Isso garante que, após ceder a CPU para uma aplicação, periodicamente a CPU seja devolvida ao sistema operacional. Isso permite que o sistema operacional interrompa a execução de um programa e volte a executar outro, evitando assim a monopolização por um único programa.
Consequentemente, outros programas têm a oportunidade de usar a CPU, assegurando que o sistema operacional obtenha a CPU em intervalos regulares para executar outras aplicações, ou que a aplicação atual continue a ter acesso à CPU.

## Interrupção e Exceção

Quando executamos instruções no modo núcleo podemos executar todas as instruções que o hardware prover. E quando a CPU está rodando no modo usuário, só podemos rodar um subconjunto dessas instruções. 
Se um programa no modo usuário tentar executar algo fora da faixa permitida por ele ocorrerá uma exceção. Ou seja, a unidade central de processamento irá gerar uma exceção. E uma exceção acontece sempre que uma instrução não pode ser completada. 
A unidade de controle começa a executar uma instrução e a primeira coisa que ela verifica (no PSW) é se a máquina está no modo usuário ou modo núcleo. 
Se a máquina estiver no modo usuário, ela verifica se a instrução que está sendo executada é uma instrução que pode ser executada com privilégio de usuário. Se isso não for o caso é gerada uma exceção. 
A exceção é tratada similar a interrupção mas não podemos completar a instrução (na interrupção deve-se completar a instrução) porque foi justamente um problema na execução da instrução que gerou a exceção. 
Da mesma forma que a interrupção a CPU vai fazer com que o SetPointer (SP) aponte para a pilha do núcleo, e que o PC aponte para o endereço da rotina de tratamento dessa exceção e ocorre que o sistema operacional que o programa estava executando, estava executando algo que não poderia executar e consequentemente, irá abortar a execução desse programa. 

Quando um programa do usuário quer executar algo privilegiado ele pede ajuda do sistema operacional e ele não pode fazer uma chamada de função para o PC apontar para a próxima instrução, como para um read, ou seja, um set point para a pilha do sistema operacional porque se o modo usuário chamar o sistema operacional para executar no modo usuário, essas instruções do sistema operacional do read irá gerar exceções. 
A melhor forma de fazer isso é gerando exceções de forma controlada, e para todo conjunto de instruções de processadores, existe uma instrução especial chamada de Trap que sinaliza que a aplicação do usuário precisa executar algo em modo privilegiado (System Call). 
Quando por exemplo o Read do usuário é chamado, executa-se a trap que está dentro do read dele e esse trap irá gerar uma exceção. E assim, a máquina passa automaticamente para o modo núcleo onde qualquer coisa pode ser executada. Se os parâmetros forem adequados o programa é redirecionado para o system calls adequado por exemplo o read e comeco a executar o código com o modo de privilégio núcleo. 
Ou seja, se quisermos executar algo que é privilegiado, precisa-se executar uma Trap para que a máquina libere o modo núcleo  para que possa ser executado. 

## Processos

O sistema operacional cria um processo para cada instância de uma aplicação que está em execução. Essa estrutura de dados, chamada de processo, armazena o identificador da instância de execução da aplicação, o estado do processo e os valores de todos os registradores da CPU que estavam em uso antes do programa ser interrompido.
Dessa forma, quando um processo que perdeu a CPU retoma o seu funcionamento, ele terá todas as informações necessárias salvas para continuar a execução.

## Estados de um Processo

![Estados de um Processo](https://www.boscojr.com/so/figuras/estados-processos.png)

Um processo pode estar em três estados: em rodando, bloqueado e pronto para rodar. Quando um processo é criado, ele é colocado no estado "pronto para rodar". Assim que a CPU fica disponível, o processo é movido para o estado "rodando".
Se um processo em execução faz uma chamada ao sistema, ele entra no estado "bloqueado". Isso ocorre quando há operações de entrada e saída que requerem um tempo significativo para serem concluídas, como a leitura de disco. Nesse caso, o processo é bloqueado por outro processo até que a operação de entrada e saída seja finalizada.
Quando a fatia de tempo designada para a execução de um processo acaba, ele passa do estado "rodando" para o estado "pronto para rodar". Em seguida, outro processo que estava "pronto para rodar" é movido para o estado "rodando".

Um processo no estado "bloqueado" retorna para o estado "pronto para ser executado" assim que o evento pelo qual estava aguardando ocorre. Por exemplo, se um processo estava bloqueado aguardando a leitura do disco, o sistema operacional é notificado quando o controlador do disco interrompe a CPU para informar que a operação foi concluída. Nesse momento, o sistema operacional retoma a CPU e o processo que estava bloqueado é movido para o estado "pronto para rodar".
Quando ocorre uma interrupção, a CPU finaliza a execução da instrução atual e passa a executar o tratador de interrupção correspondente a interrupção gerada. Por exemplo, se ocorrer uma interrupção de disco, o tratador de interrupção de disco será executado. 
Esse tratador realiza operações de entrada e saída no disco e verifica se existem processos bloqueados aguardando o término da operação. Se houver, esses processos bloqueados são movidos para o estado "pronto para rodar".
Essas transições de estado são realizadas pelo sistema operacional, que manipula as estruturas de dados e altera o estado dos processos. Além disso, o sistema operacional só é executado quando ocorre uma interrupção ou exceção.

Assim, podemos descrever os seguintes estados:

De "Rodando" para "Pronto para Rodar":
Esse estado ocorre quando a fatia de tempo designada para a execução de um processo chega ao fim, o que é indicado pelo temporizador. Quando o sistema operacional trata essa interrupção do temporizador, ele identifica que a fatia de tempo daquele processo foi encerrada. Em seguida, ocorre a transição de estado e o sistema operacional chama o escalonador de processos para selecionar qual processo, dentre aqueles prontos para rodar, ganhará a CPU. É importante observar que esse procedimento envolve a preservação das informações dos registradores no processo que está deixando a CPU e a restauração dessas informações do processo que irá ganhar a CPU. O último registrador a ser restaurado é o PC, que indica a próxima instrução a ser executada. Ao restaurar o PC, o controle efetivo da CPU é transferido para o novo processo.

De "Rodando" para "Bloqueado":
Esse estado ocorre sempre que um processo realiza uma chamada ao sistema bloqueante. A chamada ao sistema é implementada por meio de uma exceção, que é gerada quando o processo executa uma instrução para a qual não possui privilégios e ao executar essa instrução sem privilégio, irá gerar uma exceção na unidade central de processamento. Essas exceções são tratadas de maneira semelhante às interrupções, ou seja, o PC passa a apontar para um código do sistema operacional responsável por tratar essa exceção, que, nesse caso, é a chamada ao sistema. 
A chamada ao sistema é executada e o código do sistema operacional associado a essa chamada identifica que o processo não pode prosseguir e precisa esperar por um evento. Dessa forma, o processo é movido para o estado "bloqueado", e as informações sobre o evento que ele está aguardando são armazenadas. Em seguida, o escalonador é chamado novamente para selecionar um novo processo "pronto para rodar", que será movido para o estado "rodando".

De "Bloqueado" para "Pronto para Rodar":
Essa transição ocorre quando há uma interrupção vinda de um dispositivo que sinaliza a ocorrência de um evento. Além disso, também pode ocorrer por meio de uma chamada ao sistema executada pelo processo que está "rodando". Quando uma interrupção de dispositivo ocorre, o código do sistema operacional responsável por tratá-la é acionado. Esse código realiza operações de entrada e saída relacionadas ao dispositivo e verifica se existem processos bloqueados aguardando o término da operação. Se houver, esses processos bloqueados são movidos para o estado "pronto para rodar". Essa transição também pode ocorrer quando um processo em estado "bloqueado" realiza uma chamada ao sistema específica.

Essas transições de estado são controladas pelo sistema operacional, que manipula as estruturas de dados e atualiza o estado dos processos de acordo com as condições e eventos ocorridos. Além disso, o sistema operacional é executado apenas em caso de interrupção ou exceção.

Finalmente,
A transição de "Rodando" para "Pronto para Rodar" ocorre sempre que uma interrupção de relógio é gerada.
A transição de "Pronto para Rodar" para "Em Execução" ocorre tanto devido à transição de "Rodando" para "Bloqueado" quanto devido à transição de "Rodando" para "Bloqueado".
A transição de "Rodando" para "Bloqueado" ocorre devido a uma chamada de sistema (System Call).
A transição de "Bloqueado" para "Pronto para Rodar" ocorre por meio de uma interrupção ou de uma chamada de sistema (System Call).

## Escalonamento de Round Robin 

Quando o processo que estava rodando bloqueia ou quando a fatia de tempo dele acaba, o processo passa para o estado pronto para rodar. Assim eu preciso escolher um novo processo para ganhar a CPU. Um pedaço da CPU que faz essa decisão é o escalonador, e um dos escalonadores mais populares é o escalonador Round Robin.
A ideia do escalonamento Round Robin é tentar ser justo com todos os processos. A ideia é que todos os processos que estão ativos vão ganhar uma fração da CPU que é mais ou menos a mesma, pelo menos durante o tempo em que aqueles processos estão coexistindo no sistema. 
Para fazer isso, esse algoritmo usa uma estrutura de dados extremamente simples. Usa-se uma fila onde o processo que está na cabeça da fila é o processo que está rodando e todos os processos em sequência são os processos que estão prontos para rodar. E esses processos vão rodar nessa ordem, ou seja, quando a fatia de tempo do processo que está rodando acaba ou ele perde a CPU. Se a fatia de tempo dele acabar e ele continuar pronto para rodar, ele vai para o fim da fila e o próximo processo ganha a CPU. 
Cada processo aponta para o próximo processo a ser executado depois dele e cada processo mapeia na tabela de processos a sua instrução a ser executada. 
Além disso, existe um apontador que aponta para a cabeça de um processo. Quando um processo acaba, ela aponta para o próximo processo que estava sendo apontado por quem executou por último. Ou seja, um processo aponta para outro processo e esse outro será apontado pelo apontador de processos, como se fosse uma LinkedList onde temos um head que aponta para a cabeça do nó em execução.
Além disso, o último processo da fila precisa apontar para o primeiro processo a ser executado (que está executando), e assim os processos ficam de forma circular na fila. 

Existem outras formas no sistema que vão impactar a forma sobre como eu atualizo essa minha lista (fila). 
Por exemplo, digamos que o processo 1 esteja executando e antes da fatia de tempo do processo 1 acabar, o processo 1 faz um chamada ao sistema que o bloqueia, e que faz com o programa 1 passe do estado rodando para bloqueado. 
No estado bloqueado, o programa 1 não consegue ganhar a CPU porque ele está esperando um evento e ele não consegue executar a próxima instrução do seu código até que o evento pelo qual ele está esperando ocorra. Portanto, eu preciso retirar o processo 1 dessa fila que é o processo que está bloqueado.
Eu sei que a cabeça está apontando para o processo 1, eu elimino o processo para qual a cabeça aponta que é o processo 1 e passo para a cabeça apontar para o processo pelo qual o programa 1 apontava. E faz-se o último processo da fila (tail) apontar para a nova cabeça que será quem o processo será executado e isso é o evento rodando para o bloqueado. 

Quando um processo é criado, o pai dele está rodando, pois o pai dele é quem está executando o System Call que vai criar aquele processo semelhante ao pai (que é o processo criado que estamos falando), só que esse processo vai estar no estado pronto para rodar e ele vai ser colocado no fim dessa fila. 

O estado que é bloqueado fica pronto para rodar quando o evento que um processo estava bloqueado aconteça, ou seja, o processo bloqueado está esperando por determinado evento e o evento acontece. Quando o evento acontece, o processo que estava bloqueado passa para o estado pronto para rodar e ele precisa ser incluído na fila novamente. Ou seja, quando sai de rodando para bloqueado ele sai da fila e quando ele sai de bloqueado para pronto para rodar ele volta para a fila.
O processo quando sai de bloqueado para pronto para rodar deve voltar a fila, mas ele não vai para o fim da fila e a razão para isso é que o tempo que esse processo passou bloqueado pode ser muito grande, sobretudo comparando com o tempo de uma fatia. Alguns processos desse passam por algumas dezenas de milissegundos executando e se um processo bloqueou por alguns segundos, os processos que estavam prontos para rodar executaram dezenas ou até centenas de vezes antes que esse processo que estava bloqueado pudesse ser executado pois estava bloqueado. Enquanto esse processo estava bloqueado, outros processos executaram varias vezes enquanto ele aguardava e seria injusto coloca-lo no fim da fila. 
E assim, é importante dar uma prioridade para esse processo que passou de bloqueado para pronto para rodar, e para o nosso exemplo anterior, o processo 1 que foi bloqueado voltaria para a fila, mas para o segundo colocado na fila enquanto o primeiro lugar seria quem ainda está rodando, e o segundo processo daria espaço para entrar o processo que passou de bloqueado para pronto para rodar, sendo assim, o terceiro processo seria colocado em terceiro lugar a ser executado para poder dar espaço para esse processo poder ficar no segundo lugar da nossa fila. 
As empresas usam round-robin ou variações dele para fazer escalonamento.

## Criação de processos

Quando temos um processo que executa um programa shell, o shell escreve na tela o prompt e lê o que o usuário digita no teclado.
Quando o usuário escreve “ls” e tecla enter, a linha é passada para o shell que interpreta essa linha e o shell cria um novo processo que vai executar o programa /bin/ls que vai listar os nomes dos arquivos do diretório corrente no terminal. Quando o ls morre, o shell volta a executar, coloca novamente o prompt e fica esperando para o próximo comando. 

### Como o shell cria esse processo?

Os processos estão armazenados na tabela de processos, essa tabela de processos contém informações na memória sobre quem é o processo, qual o estado do processo, quais são os valores dos registradores daquele processo na última vez que ele executou para que eu possa retomar a execução do processo de onde ele parou, etc. 
Além disso, na memória, o processo ocupa uma determinada área da memória. Nessa área de memória, temos a imagem de um processo e ela é formada por quatro pedaços que são o código, os dados estáticos, o heap e a pilha.
Quando um processo quer criar um outro processo, ela terá que criar uma entrada na tabela de processos para armazenar as informações do novo processo, inicializar essa estrutura de dados processos com os dados adequados e criar uma nova imagem para esse novo processo que vai ser criado e isso é feito via chamada ao sistema. 
Nos sistemas da família UNIX essa chamada ao sistema se chama FORK, que significa encruzilhada do inglês. 
A ideia é que tenha um processo tenha um fluxo de execução até que ele chame o System Call FORK, e aí nesse ponto a gente passa a ter dois processos executando, o pai e o filho. A diferença é que no pai do System Call FORK retorna o identificador do filho, enquanto que no filho do System Call retorna zero. 

No processo 1, eu tenho o código fonte, os dados e a pilha. E em algum lugar do código fonte do processo 1 eu tenho uma chamada para FORK. Quando o programa 1 chama o FORK, irá ocorrer um System Call e será gerado uma exceção, o PC(Program Counter) da CPU que estava apontando para uma área do processo do processo 1 passará a apontar para uma área sistema operacional que tem o código do System Call FORK. 

A execução desse código aloca na tabela de processos uma entrada que esteja livre.
Digamos que o P1 (processo 1) esteja ocupando a entrada Processo#1 e que a entrada Processo#3 esteja vazia, então o Fork aloca essa entrada 3 para ser o lugar onde a gente vai guardar informações do processo P1’ que é o filho de P1 e P1’ será criado semelhante ao pai. 
Então, o FORK aloca uma área de memória que seja pelo menos do mesmo tamanho que a área de memória onde o pai está executando. Fazemos uma cópia do pai na memória. 
P1’ será exatamente o mesmo código do pai P1, uma cópia dos dados do pai, uma cópia da pilha do pai e termos uma chamada Fork em P1’ também. 
O Fork do pai retorna o identificador do filho enquanto que o filho vai retornar o zero. 
O ID do filho é o ID do pai até que faça ele pegar o ID dele mesmo. 

# Threads

Em um exemplo de Threads, acontece quando vários clientes mandam uma requisição, o dispatch recebe essas requisições, ler para verificar o tipo de requisição e repassa para ser executado por outra entidade, o dispatch executa várias tarefas de forma independente e essas tarefas não precisa ser serializadas, ou seja, numa determinada ordem e pode executadas requisições em paralelo. 

O dispatch era um listener, antes era um processo que esperava o trabalho dos clientes, esperava até chegar uma requisição dos clientes e quando a requisição chegava, o listener realizava o FORK para atender cada uma das requisições. Cada requisição era atendido por um flho do dispatcher, ou seja, ele cria vários filhos igual ao dispatch, que ia executar o código necessário para processar essa requisição para atender o cliente. 
As threads funcionam da mesma forma. 
Entretanto, o FORK usava múltiplos processos para implementar essa solução, FORK é uma chamada ao sistema cara para executar, quando cria um filho a partir de um processo, esse filho tem a mesma memória do pai, ele é uma cópia do pai, essa memória precisa ser copiada e isso custa caro. 
Se os filhos de um processo precisa se comunicar com entre eles ou com o processo pai, a gente precisa implementar alguma forma de comunicação entre os processos, uma vez que os filhos por padrão não compartilham memória, cada um tem sua própria memória.
A abstração Thread vem para nos salvar, uma Thread é uma alternativa para esse mesmo problema de dispatch. Então, uma Thread é uma unidade escalonada, tal como o processo, ou seja, o escalonador pode decidir quem vai executar. E cada Thread é um fluxo de execução, tal como é um processo, é uma sequência de instruções que vão ser executadas na máquina. As Threads são partes mais ou menos independentes de uma aplicação maior. 
As vantagens das Threads em comparação a múltiplos processos é que as Threads são menos caras de criadas, pois elas compartilham a memória do pai, a gente não precisa copiar a memória do pai quando criamos uma Thread nova. 
Por isso, para compartilhar a memória do pai é mais fácil programar com Threads, inclusive, arquitetar uma aplicação com múltiplos Threads por causa da memória compartilhada. 
Podemos usar múltiplas Threads para fazer várias tarefas ao mesmo tempo para melhorar o desempenho no computador em aplicações pesadas. 
Onde, uma Thread faria uma tarefa e outra Thread faria outra tarefa diferente. 
Os benefícios de uso das Threads são: 
As Threads melhoram a responsividade da interface com o usuário.
As Threads melhoram o desempenho de renderização. 
As Threads melhoram o desempenho para o IO e fazem a estruturação da aplicação. 

## Condições de corrida

O fluxo de execução pode perder a CPU em uma hora inadequada por uma interrupção de relógio e essa interrupção faz com que a CPU pare de executar o programa e passe a executar o sistema operacional no pedaço que trata interrupções de relógio.
Supondo que essa interrupção de relógio causou o fim da fatia de tempo desse fluxo de execução. Então, o sistema operacional chama o escalonador para que um outro fluxo possa ser executado. 
Antes da troca, os valores dos registradores foram salvos para quando trouxesse o processo de volta a execução pudesse restaurar os valores nos registradores. 

A condição de corrida acontece sempre que um código manipula uma área de memória que é compartilhada por mais de um fluxo, quando essa manipulação não é apenas de leitura. Se for apenas de leitura não há problemas no compartilhamento, mas se eles escrevem nessa área de memória compartilhada, então teremos condição de corrida. 
Para resolver esse problema é necessário a implementação de exclusão mútua na região crítica. 
Preciso adicionar código a minha aplicação, que garanta que quando o fluxo está executando a sua região crítica, nenhum outro fluxo pode está executando a região crítica correspondente a aquele fluxo. E isso garante que a execução da aplicação será determinística. 

Exemplo de condições de corrida:
Imagine que você é a Thread 1 e eu sou a Thread 2.
Temos uma variavel que será usada por nós para incrementar valores.
Como eu e você interessamos em escrever ou ler essa variável teremos condições de corrida para essa variável.
Digamos que a variável tenha um valor inicial 1000.
Eu ganho a CPU e eu leio o valor da variável que é 1000 e quero incrementar 2000 a ela. E suponha que antes de incrementar e salvar eu acabe a minha fatia de tempo e eu perco a CPU. Assim, você será o proximo a ganhar a CPU.
Antes da troca, os valores dos registradores foram salvos para quando eu voltar a execução pudesse restaurar os valores nos registradores.
Agora, você ganha a CPU, você ler o valor da variável que ainda é 1000(pois não foi incrementada pois perdi a CPU antes de incrementar).
Suponha que você queira incrementar 500 e você consegue, você fez (1000 + 500 = 1500) e coloca na variável esse valoer 1500, ou seja, tira o valor que há nele, não importa qual seja e coloca 1500. Você acabou de realizar a tarefa que você desejava, portanto, você morre feliz.
Mas eu ainda não acabei de executar, eu volto a ganhar a CPU em algum momento depois do ocorrido, e eu volto a fazer o que gostaria que era incrementar e salvar o valor. Eu incremento 1000 que eu li com 2000 que gostaria de incrementar e esse resultado da 3000. Em seguida eu retiro o vaalor atual da variavel, não importa qual esteja e coloco o valor que eu somei que foi 3000. Ou seja, retirei esses 1500 e coloquei 3000 no lugar.

Valor atual da variável: 3000

Valor que deveria ser na variável: 3500

Imagine se isso fosse o saldo de uma empresa em um banco, imagine que cada Thread fosse outras empresas realizando pagamento milhonarios a essa empresa.

Como essa variável está sendo utilizada para escrever por vários processos, então temos uma região crítica para essa variável.
Se essa variável fosse apenas de leitura, então haveria problemas, pois o valor nunca muda e não precisariamos nos preocupar com problemas sobre condições de corrida.

## Solução de condições de corrida com espera ocupada

Um dos requisitos para soluções de exclusão mútua é que as soluções devem garantir a exclusão mútua no acesso às regiões críticas. Não podemos fazer hipóteses sobre o número de processadores ou a velocidade relativa dos processadores envolvidos na computação, mas devemos impedir que um fluxo entre na região crítica se outro fluxo estiver usando a região crítica. 
Ou seja, deve-se evitar inanição (starvation), ou seja, todos os fluxos precisam ganhar em algum momento a região crítica para que eles possam avançar na sua computação.
Um starvation ocorre quando um processo nunca é executado, pois processos de prioridade maior sempre o impedem.

As soluções que vou discutir podem ser implementadas sem o auxílio do sistema operacional, ou seja, não preciso fazer um System Call para implementar essas soluções, eu posso fazer tudo com o código do usuário.
Além disso, essas soluções vão incluir algum código antes da entrada da região crítica que vai verificar se esse fluxo pode ou não entrar na região crítica.
O fluxo que não vai poder entrar na região crítica ficará em loop até que a região crítica seja liberada e ele (o processo que estava em loop) possa entrar na região crítica. Para isso, devemos tipicamente, no final da região crítica, executar algum código que sinalize que a região crítica não está sendo ocupada por aquele fluxo.
Deve-se colocar uma flag booleana global para sinalizar que a região crítica está sendo executada.

Exemplo:
class Exemplo {
    public int valor;

    
}


Seguindo o exemplo, o problema é que fazendo isso eu resolvo o problema de uma região crítica mas crio outra região crítica e o que pode ocorrer é que se esse fluxo de execução for interrompido depois da checagem que a flag era false e antes de escrever que flag era true, eu perder a CPU.
Assim, outro fluxo pode entrar na região crítica, onde um fluxo irá escrever na memória compartilhada e no fim da execução, voltará ao antigo fluxo que perdeu a CPU que fará flag igual a true e irá sobrescrever os dados do fluxo que ganhou a CPU após ele perder. Ou seja, poderá haver mais de um processo sendo executado, manipulando a mesma região crítica.

Para resolver esse problema, podemos usar a instrução TSL (Test and Set Lock) que são instruções em Assembly que fazem duas operações ao mesmo tempo.
TSL é uma instução de máquina (uma operação atômica) super rápida que impede que o problema do exemplo acima aconteça. Entretanto, essa instrução de máquina não é encontrado em todos os processadores (como processadores mais antigos).

### Como funciona o TSL (Test-And-Set-Lock)?

O Lock TSL ou TAS (significa a mesma coisa) é um mecanismo de sincronização para implementar a exclusão mútua. Ele é uma operação atômica simples que envolve testar uma variável e definir seu valor para um novo valor em uma única operação, sem interrupções.

Por exemplo, suponha que temos uma variável global Flag que é do tipo AtomicBoolean que serve como um sinalizador. Essas variáveis atomicas espera um valor inicial.
Suponha também que temos uma variável inteira para incementar.

> private AtomicBoolean flag = new AtomicInteger(false);
> private int incrementador = 0;

Digamos que vem uma requisição desejando incrementar, e o que ela precisa fazer é perguntar se a região crítica está livre.
O que fazemos é esse codigo: 
> flag.testAndSet(true);

Esse trecho de código faz é pegar o valor do estado atual, no caso é False (pois iniciamos o AtomicBoolean como false) e em seguida ele tenta modifificar o valor para True.
Como a variavel inicialmente está False, então ao realizar

flag.testAndSet(true) 

Ele vai pegar o valor inicial (que no nosso caso está como False inicialmente) para retorna-lo após tentar trocas o estado atual para True.
Como a variável está False, então ele consegue trocar para True e retorna o estado anterior que é False.

Novo Estado: True
Valor retornado: False

Se ele tentar fazer de novo esse trecho de código flag.testAndSet(true), ele pegará o valor atual que agora é True e tentará mudar esse valor True para True. Em seguida ele retorna o estado anterior (antes de tentar a troca) que é True.
Novo Estado: True
Valor retornado: True

Ele so consegue modificar o estado quando ele está como False. Retornado False e alterando para True.

Para alterar um valor de uma variável ao fim da região crítica, basta fazer essa instrução para que outra Thread possa ser executada.
> flag.set(false);

E assim, outra Thread ao fazer flag.testAndSet(true); poderá modificar o valor e entrar na região crítica.

      import java.util.concurrent.atomic.AtomicBoolean;

      class Exemplo {
          private int n;
          private AtomicBoolean flag;
    
          public Exemplo() {
             this.flag = new AtomicBoolean(false);
          }
    
          public void runThreads() {
              for (int i = 0; i < 10; i++) {
                 final int threadID = i;
                 Thread thread = new Thread(() -> run(threadID));
                 thread.start();
              }
    
              while (this.n < 10);
          }
    
          public void run(int i) {
              while (this.flag.getAndSet(true));
              System.out.println(String.format("Thread %d está executando!", i));
              this.n += 1;
              System.out.println(String.format("Thread %d terminou de executar!", i));
              this.flag.set(false);
          }
      }

>
>
      public class Main {
    
          public static void main(String[] args) {
              Exemplo exemplo = new Exemplo();
              exemplo.runThreads();
          }
      }


Nesse código acima garantimos que a saida sempre será neste padrão:

    Thread N está executando!
    Thread N terminou de executar!

E nunca será desta forma (um de muitos possiveis casos):

    Thread X está executando!
    Thread X terminou de executar!
    Thread Y está executando!
    Thread Z está executando!
    Thread Z terminou de executar!
    Thread Y terminou de executar!

### Continuando...

Depois que executa-se a TSL, move-se o conteúdo de flag para o R1 e na mesma instrução movemos algo que é diferente de zero para a flag, ou seja, R1 receberá o conteúdo antigo de flag e flag receberá um novo conteúdo diferente de zero. 
Depois disso, executamos uma instrução de comparação que compara o valor zero a R1 e a comparação é feita subtraindo os dois valores que queremos comparar. 
Se essa subtração for zero é porque esses valores são iguais. 
Se der diferente de zero, se der menor que zero é porque um é maior que o outro.
E se der maior que zero é porque um é menor que o outro. 
Em seguida, tentamos saber qual foi o valor da última comparação e se a comparação for diferente de zero, ou seja, R1 for diferente de zero ele fica em loop. 
Essas instruções podem ser colocadas no início da região crítica onde só entraria na região crítica se quando executasse a região crítica, ela estivesse vazia, ou seja flag igual zero, ou seja, disponível. 

    
    enter_region_critical:              // Indica o início da região crítica
    loop:                               // Marcação que indica um início de loop.
    TSL R1, Flag                        // Instrução que testa e define o valor da variável Flag 
                                    
    CMP R1, #0
    JNZ loop
    leave_region_critical:
    MV #0, Flag
    
    Onde, 
    enter_region_critical:              // Indica o início da região crítica
    loop:                               // Marcação que indica um início de loop.
    
    TSL R1, Flag                        // Instrução que testa e define o valor da variável Flag atomicamente. 
                                        // Se a variável Flag for 0, ela a define como 1 e carrega 0 em R1
                                        // Se a variável Flag já for 1, ela a deixa inalterada e carrega 1 em R1
    
    CMP R1, #0                          // Instrução que compara o valor de R1 (0 ou 1) com o valor imediato 0.
    JNZ loop;                           // Volta para a marcação do loop se a comparação anterior indicar que
                                        // R1 não é igual a 0. O loop continua enquanto a Flag já estiver definida 
                                        // (ou seja, a região crítica está sendo executada por outro thread).
    
    leave_region_critical:              // Marcação indicando o fim da região crítica.
    MV #0, Flag                         // Instrução que armazena 0 a variável Flag, indicando que a região
                                        // crítica está disponível para outras Threads a executarem. 

Se Flag = 0, então, a região crítica está disponível.
Se Flag = 1, então a região crítica está ocupada.

Este código implementa exclusão mútua entre várias threads que acessam uma região crítica. 

Quando executo a primeira instrução TSL, se a região crítica estivesse disponível, ou seja flag igual a zero, ela agora não vai mais está disponível porque na execução dessa instrução, zero foi copiado em R1 e um valor diferente de zero foi copiado em flag e se o processo perder a CPU depois no TSL, ou seja, antes da comparação, não haverá problema porque a outra thread ficará em loop pois, o valor de thread já está diferente de zero. 
No fim da região crítica devemos mover zero para flag para liberar a região crítica para outra thread. 
O loop aguarda a liberação da região crítica e é executado fora da região crítica. 





Semáforos 

A execução com vários fluxos e memória compartilhada são bem mais rápidos. E deve-se fazer loops antes da região crítica invés de dentro dela. 
Podemos resolver esse problema de exclusão mútua usando outro tipo de solução, soluções que são oferecidas pelo sistema operacional que permitem que um processo ou uma thread ao invés de ficar em espera ocupada, peça ao sistema operacional para bloquear aquela thread e colocar uma outra thread para executar. E o nome da solução que implementa isso são semáforos. 
 
Semáforos são abstrações do sistema operacional para permitir que processos possam se sincronizar, normalmente é implementado como tipo abstrato de dados que oferece duas características primitivas. 
Uma é chamada de down, que serve para que um processo ou uma thread solicite seu bloqueio, dependendo do estado do semáforo. 
E a outra é chamada de up, que serve para notificar um outro processo que uma determinada condição foi satisfeita. 
Ou seja, down serve para que um processo ou thread o bloqueie. 
E up serve para notificar um outro processo que uma condição foi satisfeita. 

No código, uma variável carrega o valor do semáforo e outra variável carrega uma lista dos processos que estão bloqueados neste semáforo. A variável do semáforo é global.  

Se o semáforo for 0, então a região crítica está protegida de outras threads. 
Se o semáforo for 1, então a região crítica protegida pelo semáforo está disponível e pode ser acessada por outro processo ou thread. 

O down é implementado da seguinte forma, se o valor do semáforo já é zero, então o processo que fez a chamada do down deve bloquear e para isso a gente adiciona uma referência para esse processo na lista de processos bloqueados desse semáforo. Remove a referência desse processo na lista de processos ou threads que estão prontos para rodar, onde essa fila é de escalonados para rodar. E em seguida, chamamos o escalonador para que um um novo processo seja colocado para rodar. 
Ou seja, o processo é bloqueado e a CPU passa para um outro processo que está pronto para rodar. 
E caso contrário, caso o valor do semáforo não seja igual a zero, então o valor é positivo e o valor do semáforo será decrementado, a função down retorna e o processo não vai bloquear. 

Já o up funciona da seguinte forma: se não tiver nenhum processo bloqueado nesse semáforo, ou seja, se a lista de processos bloqueados estiver vazia, então o up incrementa o valor do semáforo. 
Caso contrário, um dos processos que estava bloqueado é removido da lista de bloqueados e é adicionado na lista de processos que estão prontos para rodar. 
Quando o escalador executar novamente ele vai passar a considerar esse processo que foi desbloqueado, como um processo passível de ser colocado para executar. 

Para se implementar a exclusão mútua usando semáforos, basta-se criar um semáforo binário, ou seja, iniciado com o valor 1 e antes de entrar na região crítica o processo chama mutex.down para bloquear outro processo ou thread e após executar a região crítica, o processo chama mutex.up para liberá-la. 

Quando mutex.down é executado, o primeiro processo que fizer isso vai encontrar o valor do mutex igual a 1 e o down vai entrar na parte que vai decrementar o semáforo para zero e vai retornar. Ou seja, o primeiro processo que chamará mutex down não vai bloquear. 
Se antes desse processo que está dentro da região crítica terminar a execução na região crítica, um outro processo fizer mutex.down, o que vai acontecer é que o valor do semáforo é zero e esse processo vai bloquear porque agora, a região crítica não está disponível. 

Depois que o processo executa a região crítica ele chama up e temos duas situações onde uma delas é que não tenho nenhum processo bloqueado nesse semáforo, nesse caso o up vai simplesmente incrementar o valor do semáforo e voltar ao valor original que é 1. 
Caso contrário, o que vai acontecer é que um dos processos que estavam bloqueados neste semáforo vai ser acordado e o semáforo que continua com o valor zero vai simplesmente incrementar o valor do semáforo e voltar ao valor original que é 1 para que esse processo que estava bloqueado possa acessar a região crítica. Caso contrário, o que vai acontecer é que um dos processos que estavam bloqueados neste semáforo vai ser acordado e ele vai verificar se o semáforo está positivo(região crítica liberada) e vai entrar na região crítica e em seguida vai bloqueá-la e o semáforo volta ao valor zero. Caso o semáforo esteja em zero, ele volta a ficar bloqueado esperando o próximo up. 

Monitores

Existem duas funcionalidades básicas para o uso de monitor, onde uma é a definição de regiões críticas e isso é feito através de anotações no código onde ficam as regiões críticas, ou seja, de alguma forma o programador indica para o compilador que pedaços do código compõem as regiões críticas. E isso permite que o compilador insira instruções automaticamente que protejam as regiões críticas do programa. 

A outra funcionalidade é denominada de forma genérica de variáveis condicionais e a ideia dessas variáveis é que elas podem ser definidas para que um processo ou uma thread sinalize que está aguardando um evento ou que uma condição seja satisfeita. 
Ou de outro lado, que notifique que um evento ocorreu ou que uma condição foi satisfeita. 
Quando um processo sinaliza que está aguardando um evento ou uma condição, esse processo vai ser bloqueado e um outro processo que notifica que aquele evento que o primeiro estava esperando foi satisfeita, então o processo que estava bloqueado volta a ficar pronto para ser executado. 

Em Java, dizemos a anotação synchronized para algum método da classe, ou blocos dentro do método, significa que esses trechos de código não podem ser executados em paralelo, ou ao mesmo tempo por duas threads diferentes. Ou seja, uma vez que uma thread, esteja executando um dos métodos marcados com synchronized em um objeto particular, nenhuma outra thread vai poder executar métodos daquele mesmo objeto que também estejam marcados com synchronized. 
Synchronized também pode ser usado indicado um objeto específico e a thread que detém o bloco de um determinado objeto, quando ela entra dentro desse bloco synchronized qualquer outra thread que queira adquirir aquele bloco vai ser bloqueado até que a thread que detém o bloco termine de executar o bloco sincronizado. 
Em relação a variáveis condicionais, em Java qualquer objeto pode ser usado como uma variável condicional .
A sinalização para aguardar para um evento naquela condição é simplesmente chamar o método obj.wait(), ou seja chamar wait do objeto que está sendo usado para sincronização. Essa chamada precisa ser feita dentro de um bloco sincronizado nesse objeto que é usado como variável condicional. 
Para notificar que um evento ocorreu, há duas possibilidades, onde uma é chamar a função notify do objeto que é variável condicional (obj.notify()) e isso vai fazer com que uma das threads que tiver bloqueado naquele objeto, se tiver alguma, uma dessas threads seja desbloqueada, ou seja, volte a ficar prontos para rodar. Ou podemos usar o obj.notifyAll() para fazer com que todas as threads sejam acordadas. 
Nos dois casos tipicamente esse wait() é colocado um teste para saber se uma determinada condição ou evento ocorreu e se não tiver ocorrido, deve-se esperar. 
Já para o notifyAll() que acorda as threads é importante que esse teste que é feito antes de fazer o wait() esteja dentro de um loop de um while ao invés de simplesmente um if. 
 Ou seja, o wait serve para sinalizar que está aguardando um evento ou que uma condição seja satisfeita. 
E os dois notify servem para notificar que um evento ocorreu ou que uma condição foi satisfeita. 


