
## Seção 5.4: Determinado frequência de Relógio

### Questão 5.19
Assumindo que um inversor tem um atraso de 1 ns, todas as demais portas têm um atraso de 2 ns e as conexões têm um atraso de 1 ns, determine o caminho crítico do circuito somador completo mostrado na Fig. 4.31.

![Circuito da Questão 2.30 - item a](figuras/Fig.4.31.jpeg)

---

### Etapa 1: Entender o Objetivo - Caminho Crítico
O caminho crítico de um circuito é o caminho do sinal, de uma entrada até uma saída, que leva o maior tempo para ser percorrido. Esse tempo máximo de atraso determina a velocidade máxima em que o circuito pode operar.

**Valores de Atraso Fornecidos:**
- Atraso de qualquer porta lógica (E, OU, XOR): 2 ns  
- Atraso de um inversor (NÃO): 1 ns (não há inversores neste circuito)  
- Atraso de cada conexão (fio): 1 ns  

---

### Etapa 2: Analisar os Caminhos do Circuito (Figura 4.31)
O circuito somador completo tem três entradas (a, b, ci) e duas saídas (co, s). Precisamos encontrar o caminho mais longo de qualquer entrada para qualquer uma das saídas.

Analisando os caminhos para cada saída separadamente.

#### Caminho para a Saída **co** (Carry de Saída)
A saída **co** é gerada pela porta OU, que recebe os resultados das três portas E. Um sinal de entrada, como o **a**, precisa percorrer o seguinte caminho:
- Passar por um fio da entrada até uma porta E.  
- Passar pela porta E (ex: a que calcula a·b).  
- Passar por outro fio da saída da porta E até a entrada da porta OU.  
- Passar pela porta OU.  
- Passar por um último fio da saída da porta OU até a saída final **co**.

#### Caminho para a Saída **s** (Soma)
A saída **s** é gerada por uma porta XOR de 3 entradas. Um sinal de entrada, como o **a**, percorre um caminho mais curto:
- Passar por um fio da entrada até a porta XOR.  
- Passar pela porta XOR.  
- Passar por um fio da saída da porta XOR até a saída final **s**.

---

### Etapa 3: Calcular o Atraso de Cada Caminho
Somando os atrasos dos componentes em cada caminho.

**Atraso do Caminho para co:**
- Atraso = (Atraso Fio 1) + (Atraso Porta E) + (Atraso Fio 2) + (Atraso Porta OU) + (Atraso Fio 3)  
- Atraso = 1 ns + 2 ns + 1 ns + 2 ns + 1 ns  
- **Atraso Total para co = 7 ns**

**Atraso do Caminho para s:**
- Atraso = (Atraso Fio 1) + (Atraso Porta XOR) + (Atraso Fio 2)  
- Atraso = 1 ns + 2 ns + 1 ns  
- **Atraso Total para s = 4 ns**

---

### Etapa 4: Determinar o Caminho Crítico e a Resposta Final
Ao comparar os atrasos totais, vemos que o caminho para a saída **co** é o mais longo.

- Atraso para **co** (7 ns) > Atraso para **s** (4 ns)

Portanto, o caminho crítico do circuito é o caminho que leva à saída **co**, com um atraso total de **7 ns**.

Isso corresponde à resposta fornecida na sua primeira imagem. São 3 segmentos de 1 ns, totalizando 3 ns. O cálculo final:
- **Atraso das Portas:** 2 ns (Porta E) + 2 ns (Porta OU) = 4 ns  
- **Atraso dos Fios:** 1 ns + 1 ns + 1 ns = 3 ns  
- **Atraso Crítico Total:** 4 ns + 3 ns = 7 ns
