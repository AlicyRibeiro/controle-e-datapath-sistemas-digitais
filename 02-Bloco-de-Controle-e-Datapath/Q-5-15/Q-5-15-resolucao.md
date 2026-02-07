## Seção 5.3: Exemplos e Questões de Projeto RTL

### Questão 5.15

Usando o método de projeto RTL da Tabela 5.1, desenvolva um projeto RTL que computa a soma de todos os números positivos presentes em um conjunto de 16 registradores separados, de 32 bits cada, os quais armazenam os números na forma de complemento de dois. Torne o projeto o mais rápido possível, executando tantos cálculos concorrentemente (em paralelo) quanto for possível. Sugestão: esse é um projeto com predomínio de dados.

#### Tabela 5.1

![Circuito da Questão 2.30 - item a](Q-5-10/figuras/Tabela5.1.jpeg)

---

## Objetivo

**Tarefa:** Projetar um sistema RTL que calcula a soma de todos os números positivos em um conjunto de 16 registradores de 32 bits (em complemento de dois).  

**Restrição:** O projeto deve ser o mais rápido possível, maximizando o paralelismo.

---

## Etapa 1: Máquina de Estados de Alto Nível (HLSM)

A HLSM descreve o comportamento desejado. Para ser rápido, tentaremos fazer o máximo de operações em um único estado de processamento.

### Registradores Internos (variáveis)

- **sum (32 bits):** Para acumular a soma dos números positivos.
- **index (4 bits):** Um contador para varrer os 16 registradores (de 0 a 15).

### Entrada Externa

- **start (1 bit):** Para iniciar o processo.

### Saída Externa

- **S (32 bits):** Onde o resultado final da soma será apresentado.

### Descrição do Comportamento

- **Estado Idle (Ocioso):**  
  A máquina espera o sinal start ser ativado.

- **Estado Init (Inicialização):**  
  Ao receber start=1, a máquina vai para este estado, onde zera o acumulador sum e o contador index em um único ciclo.

- **Estado Processing (Processamento):**  
  Este é o estado principal. A cada ciclo de clock, ele executa três ações em paralelo para maximizar a velocidade:
  - Lê o dado do registrador apontado por index (data := RegFile[index]).
  - Verifica e Soma: Se o dado lido for positivo (data[31]=0), ele é somado ao acumulador (sum := sum + data).
  - Incrementa: O índice é preparado para o próximo ciclo (index := index + 1).

  **Transição de Loop:**  
  A máquina verifica se o index já processou todos os 16 registradores. Se index < 16, ela permanece no estado Processing.

- **Estado Done (Fim):**  
  Quando index chega a 16, o processo termina. A FSM vai para o estado Done, onde a saída S é atualizada com o valor final da sum e o sistema fica em repouso.

---

## Etapa 2: Projeto do Bloco Operacional (Datapath)

O datapath é o hardware que executa as ações definidas na HLSM.

### Componentes Necessários

- **Register File (Banco de Registradores):**  
  Um conjunto de 16 registradores de 32 bits. Ele tem uma entrada de endereço de 4 bits (para selecionar qual registrador ler) e uma saída de dados de 32 bits.

- **Contador index:**  
  Um contador de 4 bits com clear e increment.

- **Acumulador sum:**  
  Um registrador de 32 bits com clear e load.

- **Somador:**  
  Um somador de 32 bits para calcular sum + data.

- **Verificador de Sinal:**  
  A lógica para verificar se um número em complemento de dois é positivo é simplesmente verificar seu bit mais significativo (MSB). Se data[31] = 0, o número é positivo ou zero.

- **Multiplexador (MUX):**  
  Um MUX 2-para-1 na entrada do registrador sum para implementar a soma condicional. Ele seleciona entre o resultado da soma (se o número for positivo) ou o valor antigo de sum (se o número for negativo).

### Interface com o Controle

- **Sinais de Controle:** sum_clr, sum_ld, index_clr, index_inc.
- **Sinais de Status:** index_eq_16 (para saber quando o loop termina), data_is_pos (o bit de sinal do dado lido).

---

## Etapa 3: Projeto da FSM do Bloco de Controle

Esta FSM gera os sinais de controle para o datapath.

### Funcionamento do Controle

- **Estado Idle:**  
  Todas as saídas de controle são 0. Espera start=1.

- **Estado Init:**  
  Ativa sum_clr=1 e index_clr=1 para zerar os registradores do datapath.

- **Estado Processing:**  
  Ativa sum_ld=1 e index_inc=1 para que o datapath leia um dado, some (se positivo, controlado pelo MUX interno do datapath) e incremente o índice a cada ciclo.

  **Transição:**  
  A FSM fica em loop em Processing até que o datapath sinalize que o loop terminou (index_eq_16 = 1).

- **Estado Done:**  
  Estado final. Nenhuma ação de controle é necessária. A saída S do datapath agora contém a soma final.

