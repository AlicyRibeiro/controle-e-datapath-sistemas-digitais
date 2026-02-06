# Conteúdo da Lista: Revisão e Fundamentos

O foco desta revisão abrange desde a representação básica de funções booleanas até o projeto de componentes complexos, como **registradores multifuncionais**, baseando-se na obra de **Frank Vahid**.

---

## 1. Representação de Funções Booleanas (Seção 2.6)

**Questões:** 2.30 a 2.34  

Análise detalhada de equações booleanas, construção de **tabelas-verdade** e **diagramas de circuitos lógicos**.

**Destaque:**  
Conversão de funções complexas, como:

\[
F(a, b, c) = a'bc + ab
\]

para implementações físicas utilizando **portas lógicas**.

---

## 2. Projeto Lógico Combinacional (Seção 2.7)

**Questões:** 2.53 a 2.55  

Desenvolvimento de soluções para problemas do mundo real, exemplificado pelo **sistema de segurança de um museu**:

- **Alarme de Movimento (2.53):**  
  Ativação baseada em detecção múltipla utilizando o método de **Soma de Produtos**.

- **Monitoramento de Ronda (2.54):**  
  Detecção de estado único para verificar a presença e movimentação do guarda.

- **Expansão para 10 Salões (2.55):**  
  Aplicação de **lógica inversa** e do **Teorema de De Morgan** para otimizar projetos com alta complexidade  
  (\(2^{10}\) combinações possíveis).

---

## 3. Elementos de Memória e Sincronismo (Seção 3.2)

**Questões:** 3.15 a 3.16  

Análise comparativa de comportamento temporal entre dispositivos de armazenamento:

- **Latch D vs. Flip-Flop D:**  
  Estudo da diferenciação entre:
  - Sensibilidade ao **nível** (transparência)
  - Sensibilidade à **borda** (amostragem)

---

## 4. Registradores e Pipeline (Seção 4.2)

**Questões:** 4.1, 3.21, 3.22 e 4.3  

Estudo focado em **armazenamento paralelo** e **deslocamento eficiente de dados**:

- **Estrutura em Cascata (3.21 / 3.22):**  
  Análise do comportamento de deslocamento (*shift*) de dados de **4 bits** através de estágios síncronos.

- **Carga Paralela (`ld`):**  
  Implementação de controle de escrita em **registradores de 8 bits**.

- **Projeto Multifuncional (4.3):**  
  Desenvolvimento de um registrador avançado com funções:
  - *Hold*
  - *Load*
  - *Clear*
  - *Complement*  

  Integrando **Multiplexadores (MUX)** e **Flip-Flops**.

---

##  Resumo Técnico das Equações

Abaixo, as principais funções booleanas trabalhadas e suas respectivas simplificações:

| Questão | Função Original | Simplificação / Lógica |
|-------|-----------------|------------------------|
| 2.30 | \(F = a'bc + ab\) | Ativo se (\(a'bc\)) ou (\(ab\)) |
| 2.33 | \(F = abc + ab + a + b + c\) | \(F = a + b + c\) *(Regra da Identidade)* |
| 2.34 | \(F = a' + bc'\) | Operação **OU** entre inversão e termo **AND** |
| 2.53 | \(A = m_2'm_1m_0 + m_2m_1'm_0 + m_2m_1m_0' + m_2m_1m_0\) | \(A = m_1m_0 + m_2m_0 + m_2m_1\) |

---

##  Simulações

Algumas resoluções incluem arquivos de simulação que podem ser validados e executados nos softwares **Digital** ou **Logisim**.

 Os arquivos `.dig` ou `.circ` encontram-se organizados na subpasta: /simulações

