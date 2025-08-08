# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositório do aluno: [wellington gomes coutinho](https://github.com/Wellcoutinhoch)


## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0–20) — endereços 0 até 19, total 20 KB  
2. P2 (15 KB) → Ocupa [20–35) — endereços 20 até 34, total 15 KB  
3. P4 (10 KB) → Ocupa [35–45) — endereços 35 até 44, total 10 KB  
4. P5 (18 KB) → Ocupa [45–63) — endereços 45 até 62, total 18 KB → sobra 1 KB de espaço livre  
5. P3 (25 KB) → Não cabe na RAM → vai para Memória Virtual  

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.
  
| Processo | Páginas na RAM | Páginas no Disco |
|----------|----------------|------------------|
| P1       | Todas          | Nenhuma          |
| P2       | Todas          | Nenhuma          |
| P4       | Todas          | Nenhuma          |
| P5       | Todas          | Nenhuma          |
| P3       | Nenhuma        | Todas            |


### 1.2.4. Desfragmentação da RAM  
Após remover processos ou reorganizar, os blocos livres se juntam:  
- Antes da desfragmentação: espaços livres espalhados.  
- Depois: um bloco único de 1 KB + espaço liberado de processos finalizados → pode alocar novos processos maiores.

Se P2 e P4 terminarem, ficaria:  
- RAM livre = 15 KB + 10 KB + 1 KB = 26 KB contíguos após desfragmentar.

### 1.3. Questões para Reflexão
1. **Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
Sim, pois escolheu o menor bloco que cabia o processo, minimizando o espaço desperdiçado. Porém, pode causar fragmentação pequena que se acumula.  
2. **Como a memória virtual evitou um deadlock?**  
Ao mover processos que não cabem na RAM para o disco, ela libera espaço e permite que o sistema continue rodando sem travar, trocando páginas conforme necessário.  
3. **Qual o impacto da desfragmentação no desempenho do sistema?**  
Benefícios: melhora o uso da memória ao criar blocos maiores e contíguos para processos grandes.  
Custos: consome tempo e recursos para mover processos e reorganizar memória, podendo causar pausas temporárias.

---

## Resposta

### 1. Alocação Inicial com Best-Fit

O algoritmo **Best-Fit** procura sempre o menor espaço de memória que caiba o processo, evitando desperdício.  
Por exemplo: se tenho blocos de 10 KB, 20 KB e 30 KB e preciso alocar 18 KB, o processo vai no bloco de 20 KB.  
É bom para economizar espaço, mas pode gerar mais fragmentação externa.

### 2. Simular Memória Virtual (Paginação)

Na **paginação**, a memória é dividida em páginas (do processo) e molduras (da RAM) com tamanho fixo.  
A memória virtual usa o disco para armazenar partes que não cabem na RAM, carregando quando necessário.  
Se uma página não está na RAM, ocorre **page fault** e o sistema busca no disco.

### 3. Desfragmentação da RAM

A desfragmentação reorganiza a memória para juntar os blocos livres, formando espaços contíguos maiores.  
Isso evita o erro de “memória insuficiente” quando um processo precisa de um bloco grande e só existem espaços pequenos espalhados.

 ### 4. Questões para Reflexão

- O Best-Fit pode ser mais lento que o First-Fit, pois precisa procurar o melhor espaço.  
- A paginação elimina a fragmentação externa, mas pode causar fragmentação interna nas páginas.  
- A desfragmentação é útil quando há muitos blocos pequenos livres espalhados.  
- Uso intenso de memória virtual pode deixar o sistema lento e desgastar o disco.
