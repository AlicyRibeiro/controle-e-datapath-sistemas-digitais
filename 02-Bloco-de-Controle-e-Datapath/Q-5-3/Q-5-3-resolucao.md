## Questão 5.3

Crie uma máquina de estados de alto nível para um controlador de mistura de água fria e quente de banheira. O sistema tem uma entrada de três bits que indica a razão desejada entre água fria e quente, e uma entrada **abrir** de um bit indicando que a água deve fluir. O sistema tem duas saídas **aguaquente** e **aguafria** de quatro bits cada, que controlam as taxas ou velocidades de fluxo de água quente e fria. A soma dessas duas taxas deve ser sempre igual a 16. A sua máquina de estados de alto nível deve determinar os valores de saída de **aguaquente** e **aguafria**, de modo que a razão entre os fluxos de água fria e quente seja o mais próximo possível da razão desejada, ao passo que o fluxo total deve ser sempre 16.  
Sugestão: Como só há 8 razões possíveis, uma solução aceitável pode usar um estado para cada razão.

---

## Etapa 1: Análise e Esclarecimento de uma Inconsistência

Antes de começar, há um ponto importante no enunciado:

- As saídas **aguaquente** e **aguafria** são de 4 bits. Um número de 4 bits pode representar valores de 0 a 15.
- A soma das saídas deve ser 16.

É impossível que uma das saídas seja 16, e se uma for, por exemplo, 10, a outra teria que ser 6 (ambos valores cabem em 4 bits). No entanto, se uma for 1, a outra teria que ser 15, mas se uma for 0, a outra teria que ser 16, o que não cabe em 4 bits.

Para resolver isso, vamos assumir uma pequena correção comum em problemas como este: a soma total do fluxo será 15, que é o valor máximo representável se uma das saídas for 0.

**Nova regra:**  

    aguaquente + aguafria = 15

quando a água está fluindo.

---

## Etapa 2: Calculando as Taxas de Fluxo para Cada Razão

O coração do problema é traduzir a entrada **razao** (um número de 3 bits, de 0 a 7) em taxas de fluxo **aguaquente** e **aguafria** que somem 15. A sugestão é ter um estado para cada razão, então vamos pré-calcular esses valores.

Vamos fazer a quantidade de água fria ser proporcional ao valor de **razao**. A fórmula será:

    aguafria = arredondamento( (valor_decimal_da_razao / 7) * 15 )
    aguaquente = 15 - aguafria


### Tabela de Valores

| Entrada razão (binário) | Valor decimal (R) | Cálculo para aguafria              | Aguafria | Aguaquente |
|-------------------------|-------------------|------------------------------------|----------|------------|
| 000                     | 0                 | `round((0/7) * 15) = 0 `             | 0 (0000) | 15 (1111)  |
| 001                     | 1                 | `round((1/7) * 15) = 2`              | 2 (0010) | 13 (1101)  |
| 010                     | 2                 | `round((2/7) * 15) = 4`              | 4 (0100) | 11 (1011)  |
| 011                     | 3                 | `round((3/7) * 15) = 6`              | 6 (0110) | 9 (1001)   |
| 100                     | 4                 | `round((4/7) * 15) = 9 `             | 9 (1001) | 6 (0110)   |
| 101                     | 5                 | `round((5/7) * 15) = 11 `            | 11 (1011)| 4 (0100)   |
| 110                     | 6                 | `round((6/7) * 15) = 13 `            | 13 (1101)| 2 (0010)   |
| 111                     | 7                 | `round((7/7) * 15) = 15 `            | 15 (1111)| 0 (0000)   |


#### Legenda:
O cálculo de Aguafria utiliza a função round para converter o valor obtido da proporção 𝑅 / 7×15 (r dvidido por 7) — que pode resultar em um número fracionário — para o inteiro mais próximo. Esse arredondamento é necessário porque os níveis de controle de água são representados por valores inteiros de 4 bits, garantindo uma distribuição proporcional e uniforme dos níveis ao longo de toda a faixa de entrada.


---

## Etapa 3: Desenhando a Máquina de Estados de Alto Nível (HLSM)

Agora, vamos criar o diagrama de estados. Precisamos de um estado para quando a água está desligada e oito estados para quando está ligada, um para cada razão.

### Estado Desligado:
- **Ação:**
  
    aguaquente := 0;

    aguafria := 0;

- **Transição:**  
Se **abrir = 0**, a máquina permanece ou volta para este estado.  
Se **abrir = 1**, a máquina transiciona para o estado correspondente à entrada **razao**.

### Estados Razao0 a Razao7:
- **Ação:**  
Cada estado **RazaoX** define as saídas **aguaquente** e **aguafria** de acordo com a tabela.  
Exemplo: no estado **Razao3**, a ação é:

    aguaquente := 9;

    aguafria := 6;

- **Transição:**  
Enquanto **abrir = 1**, a máquina permanece em um dos estados de razão.  
Se a entrada **razao** mudar, a máquina transiciona para o novo estado de razão correspondente.

---

## Descrição do Diagrama de Estados

O sistema começa no estado **Desligado**. A água não flui (**aguaquente** e **aguafria** são 0).

Se o usuário aciona **abrir = 1** com a entrada **razao = 011** (decimal 3), a FSM segue a seta e vai para o estado **Razao3**.

No estado **Razao3**, as saídas são imediatamente definidas para **aguaquente = 9** e **aguafria = 6**.

Se o usuário mudar a **razao** para **101** (decimal 5) mas mantiver **abrir = 1**, a FSM seguirá a seta de transição para o estado **Razao5**, e as saídas mudarão para **aguaquente = 4** e **aguafria = 11**.

Se em qualquer momento o usuário desligar a água (**abrir = 0**), a FSM seguirá a seta correspondente e voltará imediatamente para o estado **Desligado**, zerando as saídas.

