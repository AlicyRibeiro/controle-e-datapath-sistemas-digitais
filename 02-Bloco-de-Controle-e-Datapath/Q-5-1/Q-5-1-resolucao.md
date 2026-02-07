## Seção 5.2: O Método de Projeto RTL

## Questão 5.1

### a)
Crie uma máquina de estados de alto nível que descreve o seguinte comportamento de sistema. O sistema tem uma entrada **A** de oito bits, uma entrada **d** de um bit e uma saída **S** de 32 bits.

A cada ciclo de relógio, se **d = 1**, o sistema deverá somar o valor de **A** à soma acumulada até o momento e colocar esse valor na saída **S**. Ao contrário, se **d = 0**, o sistema deverá subtrair.

Ignore as questões de estouros crescente e decrescente. Não esqueça de incluir um estado de inicialização.  
Sugestão: Declare e use um registrador interno para guardar a soma.

### b)
Acrescente uma entrada **reset** de um bit ao sistema. Quando **rst = 1**, o sistema deve reset tornando a soma igual a 0.

---

A questão pede o projeto de uma **Máquina de Estados de Alto Nível (HLSM)**, que é uma forma de descrever um sistema digital focando no fluxo de dados (ações) e no controle (estados e transições). projetando a máquina passo a passo, incorporando os requisitos das partes (a) e (b).


## Etapa 1: Definição dos Componentes do Sistema

Antes de desenhar os estados, vamos listar todas as peças do nosso sistema, conforme o enunciado:

### Entradas:
- **A (8 bits):** O valor a ser somado ou subtraído.
- **d (1 bit):** O sinal de controle que define a operação (1 para Somar, 0 para Subtrair).
- **rst (1 bit):** O sinal de reset (da parte b).

### Saída:
- **S (32 bits):** O resultado final da soma acumulada.

### Registrador Interno:
- **sum_reg (32 bits):** Sugerido pelo enunciado para guardar a soma acumulada. A saída **S** será uma cópia direta do valor deste registrador.

---

## Etapa 2: Definindo os Estados e suas Ações

Uma HLSM precisa de estados para controlar o comportamento. Para este problema, dois estados são suficientes: um para inicializar e outro para a operação contínua.

### Estado Init (Inicialização):
- **Propósito:** Garantir que o sistema comece em um estado conhecido e limpo.
- **Ação:** A única ação neste estado é zerar o nosso registrador interno.  
    - sum_reg := 0



### Estado Accumulate (Acumulando):
- **Propósito:** Este é o estado operacional principal, onde o sistema executa a soma ou subtração continuamente a cada ciclo de relógio.
- **Ações:** As ações são condicionais, baseadas na entrada **d**.
   - se (d = 1) então sum_reg := sum_reg + A
   - se (d = 0) então sum_reg := sum_reg - A
 
- **Saída:** A saída **S** deve sempre refletir o valor atual do registrador. Podemos declarar isso como uma ação contínua.
    - S = sum_reg
 

---

## Etapa 3: Definindo as Transições entre os Estados

As transições (setas no diagrama) definem como o sistema se move entre os estados.

### Da Inicialização para a Operação:
Após zerar o registrador no estado **Init**, o sistema deve começar a operar imediatamente. Portanto, há uma transição incondicional (sempre acontece) do estado **Init** para o **Accumulate**.

### Reset (Reinicialização):
A entrada **rst** tem a maior prioridade. Se **rst = 1**, o sistema deve voltar ao estado **Init**, não importa onde ele esteja.

Isso é representado por uma transição do estado **Accumulate** para o **Init** com a condição **rst** (que significa rst = 1).

### Operação Normal:
Enquanto não houver um reset (**rst = 0**), o sistema deve permanecer no estado **Accumulate**, executando a soma/subtração a cada ciclo de relógio.

Isso é representado por uma transição que sai e volta para o estado **Accumulate** (um loop) com a condição **rst'** (que significa rst = 0).

---

## Etapa 4: O Diagrama da Máquina de Estados de Alto Nível (HLSM)

Juntando todos os elementos acima, chegamos ao seguinte diagrama da HLSM:

### Como o circuito funciona de acordo com o diagrama:
- O sistema começa no estado **Init**. O registrador **sum_reg** é zerado (`:= 0`).
- No próximo ciclo de relógio, ele se move incondicionalmente para o estado **Accumulate**.
- Enquanto estiver no estado **Accumulate** e **rst = 0**, a cada ciclo de relógio ele verifica a entrada **d**:  
- se **d = 1**, ele soma **A** a **sum_reg**;  
- se **d = 0**, ele subtrai **A**.  
A saída **S** sempre mostra o valor atual de **sum_reg**.
- Se em qualquer momento a entrada **rst** se tornar **1**, a FSM seguirá a transição **rst** de volta para o estado **Init**, zerando a soma e reiniciando o processo.
