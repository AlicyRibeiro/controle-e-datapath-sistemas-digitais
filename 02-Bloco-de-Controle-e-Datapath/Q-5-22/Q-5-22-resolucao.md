## Questão 5.22
Assumindo que um inversor tem um atraso de 1 ns e todas as demais portas têm um atraso de 2 ns, determine o caminho crítico de um somador de oito bits com propagação de “vai um”:  
(a) assumindo que as conexões não têm atraso,  
(b) assumindo que as conexões têm um atraso de 1 ns

---

## Entendendo o Caminho Crítico de um Somador "Carry-Ripple"
A característica mais importante de um somador "carry-ripple" é que ele é construído encadeando vários somadores completos (Full-Adders - FAs). A saída de "vai-um" (carry-out, co) de um somador se torna a entrada de "vem-um" (carry-in, ci) do próximo.

Isso cria uma longa reação em cadeia. O cálculo do bit 1 não pode terminar até que o "vai-um" do bit 0 esteja pronto; o bit 2 precisa esperar pelo bit 1, e assim por diante. Portanto, o caminho crítico é o caminho que o sinal de "vai-um" percorre, desde o primeiro até o último somador.

---

## Parte (a): Assumindo que as Conexões (Fios) Não Têm Atraso
Neste cenário, considera-se apenas o atraso das portas lógicas.

### Passo 1: Calcular o Atraso do "Vai-um" em um ÚNICO Somador Completo
Conforme analisamos na questão anterior (5.19), o caminho crítico dentro de um somador completo é o que vai de uma entrada (ex: ci) até a saída co.  
Este caminho passa por duas portas lógicas em série (uma porta E e uma porta OU).  
O enunciado diz que "todas as demais portas têm um atraso de 2 ns".  

Atraso dentro de 1 FA = (Atraso Porta E) + (Atraso Porta OU) = 2 ns + 2 ns = 4 ns.

### Passo 2: Calcular o Atraso Total para 8 Bits
O somador de 8 bits é composto por 8 somadores completos (FAs) em cascata.  
O caminho crítico total é a soma dos atrasos do caminho do "vai-um" em cada um dos 8 FAs.

Atraso Total = 8 × (Atraso por FA)  
Atraso Total = 8 × 4 ns = 32 ns.

---

## Parte (b): Assumindo que as Conexões (Fios) Têm Atraso de 1 ns
Neste cenário, precisamos somar o atraso das portas e o atraso dos fios. Dividindo o cálculo do atraso dos fios em três partes para maior clareza.

### Passo 1: Atraso das Portas Lógicas
O cálculo para as portas é o mesmo da parte (a).

Atraso das Portas = 8 FAs × 4 ns/FA = 32 ns.

### Passo 2: Atraso das Conexões (Fios)
**Fios Internos:**  
A resposta considera que, dentro do caminho crítico de cada um dos 8 FAs, há um fio principal conectando as duas portas.  
Atraso dos Fios Internos = 8 FAs × 1 fio/FA × 1 ns/fio = 8 ns.

**Fios de Conexão entre FAs:**  
Existem 7 fios que conectam a saída co de um FA à entrada ci do próximo (do FA0 ao FA1, do FA1 ao FA2, ..., do FA6 ao FA7).  
Atraso dos Fios de Conexão = 7 fios × 1 ns/fio = 7 ns.

**Fios de Entrada/Saída Principal:**  
Precisamos contar o fio que leva o carry-in inicial (ci0) ao primeiro FA e o fio que leva o carry-out final (co7) para a saída.  
Atraso dos Fios de E/S = 1 fio (entrada) + 1 fio (saída) = 2 fios × 1 ns/fio = 2 ns.

### Passo 3: Calcular o Atraso Crítico Total
O atraso total é a soma dos atrasos de todas as portas e todos os fios no caminho crítico.

Atraso Total = (Atraso das Portas) + (Atraso dos Fios Internos) + (Atraso dos Fios de Conexão) + (Atraso dos Fios de E/S)  
Atraso Total = 32 ns + 8 ns + 7 ns + 2 ns  

**Atraso Crítico Total = 49 ns**
