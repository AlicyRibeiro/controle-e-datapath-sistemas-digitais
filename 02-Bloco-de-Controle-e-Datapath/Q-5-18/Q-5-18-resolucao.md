### Questão 5.18

Usando o método de projeto RTL da Tabela 5.1, desenvolva um projeto RTL de um filtro digital que coloca na saída a média da entrada corrente e da amostra anterior, ambas de 32 bits. Sugestão: dentro do seu bloco operacional, você pode usar um registrador deslocador à direita para implementar a divisão.

O objetivo do filtro é que a cada ciclo de relógio, a saída seja a média da entrada atual e da entrada do ciclo anterior.

---

## Etapa 1: Máquina de Estados de Alto Nível (HLSM)

Primeiro, descrevemos o comportamento do sistema de forma abstrata. Precisamos de uma variável interna para armazenar a amostra anterior e estados para controlar a operação.

**Entradas:** Din (32 bits), rst (reset, 1 bit).  
**Saída:** Dout (32 bits).  
**Registrador Interno:** prev_in (32 bits), para armazenar a amostra de entrada do ciclo anterior.

### Diagrama da HLSM

Dois estados são suficientes: um para inicializar e outro para a operação contínua de filtragem.

### Descrição do Comportamento

**Estado Init (Inicialização):**  
Ação: Zera o registrador que armazena a amostra anterior para garantir que o primeiro cálculo seja (Din + 0) / 2.  

    - prev_in := 0


Transição: Move-se incondicionalmente para o estado Filtering para começar a operação.

---

**Estado Filtering (Filtragem):**  
Ações: A cada ciclo de relógio, duas coisas acontecem em paralelo:

- A saída é calculada:  
  Dout = (Din + prev_in) >> 1.  
  O >> 1 é um deslocamento de 1 bit para a direita, que é uma forma eficiente de fazer uma divisão inteira por 2, como sugerido no enunciado.

- O registrador interno é atualizado para o próximo ciclo:  
  prev_in := Din.  
  A entrada atual se torna a "amostra anterior" para o cálculo seguinte.

Transição: Se o sinal de reset (rst) for ativado, a máquina volta ao estado Init. Caso contrário, ela permanece em Filtering.

---

## Etapa 2: Projeto do Bloco Operacional (Datapath)

O datapath é o hardware que executa as operações com dados (armazenar, somar, deslocar) definidas na HLSM.

### Componentes Necessários

- **Registrador prev_in_reg:**  
  Um registrador de 32 bits com sinais de load (prev_ld) e clear (prev_clr) para armazenar o valor da amostra anterior. Sua entrada é Din.

- **Somador (Adder):**  
  Um somador de 32 bits que calcula Din + prev_in.

- **Deslocador (Shifter):**  
  Um bloco de lógica combinacional que pega a saída do somador e a desloca 1 bit para a direita (>> 1). A saída deste bloco é a saída final do sistema, Dout.

### Interface com o Controle

- **Sinais de Controle (do Controle -> Datapath):** prev_ld, prev_clr.  
- **Sinais de Status (do Datapath -> Controle):** Nenhum sinal de status é necessário para este projeto.

---

## Etapa 3: Projeto da FSM do Bloco de Controle

O controle é uma FSM que gera os sinais prev_ld e prev_clr para comandar o datapath.

**Entradas:** rst.  
**Saídas:** prev_ld, prev_clr.  
**Estados:** Os mesmos da HLSM (Init e Filtering). Como são 2 estados, precisamos de 1 bit para o registrador de estado (vamos chamá-lo de s0).

    - Init = 0
    - Filtering = 1



### Funcionamento do Controle

**Estado Init (s0=0):**  
Ação: Ativa o sinal de limpeza (prev_clr = 1) e desativa o de carga (prev_ld = 0).  
Transição: No próximo ciclo, vai incondicionalmente para o estado Filtering (n0 = 1).

**Estado Filtering (s0=1):**  
Ação: Ativa o sinal de carga (prev_ld = 1) para que o registrador prev_in sempre capture a nova entrada Din, e desativa a limpeza (prev_clr = 0).  
Transição: Se rst=1, volta para Init (n0 = 0). Se rst=0, permanece em Filtering (n0 = 1).

---

Este design separa com sucesso a manipulação dos dados (no datapath) da lógica de controle de tempo (no controller), seguindo o método RTL.
