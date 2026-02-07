## Questão 5.6

Projete uma máquina de estados de alto nível para um contador crescente de quatro bits, com uma entrada **cnt** de controle de contagem, uma entrada **clr** para clear e uma saída **tc** de término de contagem. Use o método de projeto RTL da Tabela 5.1 para converter a máquina de estados de alto nível em um bloco de controle e um bloco operacional. Use um registrador e um incrementador combinacional no bloco operacional, não simplesmente um registrador contador. Projete o bloco de controle até o nível de registrador de estado e portas lógicas.

---

## Etapa 1: Máquina de Estados de Alto Nível (HLSM)

Primeiro, descrevemos o comportamento do contador de forma abstrata, sem nos preocuparmos com os componentes de hardware. A HLSM mostra os estados e as ações que ocorrem.

**Entradas:**  
- cnt (habilitar contagem)  
- clr (limpar)

**Saída:**  
- tc (término da contagem)

**Variável Interna (Registrador):**  
- count (um valor de 4 bits)

### Diagrama da HLSM

A FSM pode ser descrita com dois estados principais: um estado de repouso/contagem (**Counting**) e um estado momentâneo para limpar (**Clear**).

### Descrição do Comportamento

**Estado Clear:**  
Este é o estado inicial e o destino do reset. A única ação é zerar o contador (count := 0). Ele transiciona incondicionalmente para o estado Counting.

**Estado Counting:**  
- Se clr = 1, a máquina é forçada a voltar para o estado Clear.  
- Se clr = 0 e cnt = 1, a máquina incrementa o contador (count := count + 1).  
- Se clr = 0 e cnt = 0, a máquina mantém o valor do contador (count := count).

**Saída tc:**  
A saída tc é definida como 1 sempre que count == 15. Esta é uma saída condicional.

---

## Etapa 2: Projeto do Bloco Operacional (Datapath)

O datapath é o conjunto de componentes que armazena e manipula os dados. Baseado na HLSM, precisamos de hardware para:

- Armazenar o valor de 4 bits count  
- Incrementar count em 1  
- Carregar o valor 0000 em count  
- Comparar count com 15 para gerar tc  

### Componentes do Datapath

- Um Registrador de 4 bits com habilitação de carga (ld_count) para armazenar o valor atual de count  
- Um Incrementador Combinacional (+1) que recebe count e gera count + 1  
- Um Multiplexador (MUX) 2-para-1 que seleciona qual valor será carregado no registrador: o valor incrementado (count + 1) ou o valor 0000. O sinal de seleção será sel_clr  
- Um Comparador Combinacional que verifica se count == 1111_b e gera a saída tc  

### Sinais de Controle (do Controller para o Datapath)

- **ld_count (1 bit):** Habilita a escrita no registrador count  
- **sel_clr (1 bit):** Seleciona a entrada do MUX (0 = Incrementar, 1 = Limpar)

---

## Etapa 3: Projeto do Bloco de Controle (Controller)

O controller é uma FSM mais simples que lê as entradas externas (cnt, clr) e gera os sinais de controle para o datapath (ld_count, sel_clr).

**Entradas:**  
- cnt  
- clr  

**Saídas:**  
- ld_count  
- sel_clr  

Podemos criar um controller puramente combinacional, pois as ações dependem diretamente das entradas:

**Lógica para ld_count:**  
O registrador deve carregar um novo valor se estivermos limpando (clr = 1) OU se estivermos contando (cnt = 1).  
Equação:  

    ld_count = clr + cnt



**Lógica para sel_clr:**  
O MUX deve selecionar a entrada 0000 apenas quando estivermos limpando (clr = 1).  
Equação:  

    sel_clr = clr


A saída tc é gerada diretamente pelo datapath, então o controller não precisa se preocupar com ela.

---

## Etapa 4: Implementação do Bloco de Controle (Nível de Portas)

Esta etapa, pedida no enunciado, consiste em desenhar o circuito para as equações do bloco de controle. Como o nosso controller é combinacional e não precisa de estados internos, não há um "registrador de estado" para ele. A lógica é direta.

---

## Conclusão

Ao conectar a saída count do datapath de volta à sua entrada, e conectar as saídas do bloco de controle (ld_count, sel_clr) às respectivas entradas de controle do datapath, o sistema completo está projetado. Esta separação entre datapath (operações) e controller (sequenciamento) é a essência do método de projeto RTL.
