
# Questão 6.33

Assuma que o atraso de um somador é 3 ns. Com que rapidez podemos operar a árvore de somadores mostrada na Fig. 6.94 e a do Exercício 6.32 que usa pipeline?

![Circuito da Questão 2.30 - item a](figuras/Fig.6.94.jpeg)


---

## Árvore de Somadores Original (Fig. 6.94 - Sem Pipeline)

Para determinar a rapidez com que este circuito pode operar, precisa encontrar seu caminho crítico, que é o caminho de sinal mais longo do início ao fim.

**Análise do Caminho Crítico:**  
Em uma árvore de somadores não pipeline, um sinal de entrada (como R ou T) deve passar por todos os níveis de somadores antes que a saída final (S) seja válida. Neste caso, o caminho passa por três somadores em série.

**Cálculo do Atraso:**  
Com cada somador tendo um atraso de 3 ns, o atraso total é:  

Tatraso = 3 somadores x 3 ns / somador = 9 ns

**Velocidade de Operação:**  
O período mínimo do relógio (clock) deve ser igual ao atraso do caminho crítico, ou seja, 9 ns.  
A frequência máxima de operação é o inverso do período:  

Fmax = 1/9 ns ≈ 111 MHz

---

## Árvore de Somadores com Pipeline (Exercício 6.32)

Nesta versão, registradores são inseridos entre cada nível de somadores, quebrando o longo caminho crítico.

**Análise do Caminho Crítico:**  
O caminho crítico em um circuito pipeline não é mais o caminho total, mas sim o caminho mais longo entre dois registradores consecutivos. Neste projeto, o caminho entre quaisquer dois registradores passa por apenas um somador.

**Cálculo do Atraso:**  
O atraso que determina o período do relógio é agora o de apenas um somador.  

Tatraso = 1 somador x 3 ns/somador = 3 ns

**Velocidade de Operação:**  
O período mínimo do relógio pode ser reduzido para 3 ns.  
A frequência máxima de operação é:  

Fmax = 1/3 ns ≈ 333 MHz

---

## Conclusão

A versão com pipeline, projetada no Exercício 6.32, pode operar com um relógio de até 333 MHz, sendo três vezes mais rápida.
