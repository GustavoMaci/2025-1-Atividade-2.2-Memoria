# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: https://github.com/GustavoMaci/2025-1-Atividade-2.2-Memoria/tree/main

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
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta
## Objetivo da atividade

Simular a alocação de memória com o algoritmo **Best-Fit**, usar **memória virtual (paginação)** e também simular a **desfragmentação** da RAM. A ideia é entender como o sistema operacional lida com situações onde a memória RAM não é suficiente pra todos os processos.

---

## 1. Alocação Inicial com Best-Fit

Temos uma RAM de 64 KB e 5 processos com os seguintes tamanhos:

| Processo | Tamanho (KB) |
|----------|--------------|
| P1       | 20           |
| P2       | 15           |
| P3       | 25           |
| P4       | 10           |
| P5       | 18           |
| **Total** | **88 KB**     |

Começamos com a memória RAM vazia e vamos alocar os processos com o algoritmo **Best-Fit**, que tenta colocar o processo no menor espaço livre possível que ainda caiba ele.

### Alocação:

- **P1 (20 KB)** → ocupa de **0 a 20 KB**
- **P2 (15 KB)** → ocupa de **20 a 35 KB**
- **P3 (25 KB)** → ocupa de **35 a 60 KB**
- Restam apenas **4 KB livres**, então:
  - **P4 (10 KB)** e **P5 (18 KB)** **não cabem** → vão pra **memória virtual**

### Como ficou a RAM:

```
[ 0 - 20 ] → P1  
[20 - 35] → P2  
[35 - 60] → P3  
[60 - 64] → Livre (4 KB)
```

---

## 2. Simulação da Memória Virtual (Paginação)

Agora vamos ver onde cada processo ficou (na RAM ou no disco). Considerando páginas de 4 KB, temos:

| Processo | Tamanho | Páginas | Onde está?                        |
|----------|---------|---------|-----------------------------------|
| P1       | 20 KB   | 5       | RAM                               |
| P2       | 15 KB   | 4       | RAM                               |
| P3       | 25 KB   | 7       | 6 páginas na RAM, 1 no disco      |
| P4       | 10 KB   | 3       | Disco                             |
| P5       | 18 KB   | 5       | Disco                             |

Ou seja, a RAM ficou cheia e a memória virtual entrou em ação pra armazenar o que faltou.

---

## 3. Desfragmentação da RAM

Desfragmentar é basicamente juntar os blocos de memória ocupados pra deixar o espaço livre contínuo no final.

### Antes da desfragmentação:
```
[P1][P2][P3][Livre: 4 KB]
```

### Depois da desfragmentação:
```
[P1][P2][P3][Livre: 4 KB]
```

Infelizmente, mesmo depois de desfragmentar, o espaço livre continua sendo só 4 KB, então **P4 (10 KB)** e **P5 (18 KB)** **continuam sem espaço na RAM**.

---

## 4. Respostas das Questões

**🔹 Best-Fit foi melhor que First-Fit ou Worst-Fit nesse caso?**  
Acho que sim. O Best-Fit ajudou a usar bem os espaços da RAM, encaixando os processos onde sobraria menos espaço. First-Fit e Worst-Fit poderiam deixar buracos de memória maiores e menos aproveitáveis depois.

**🔹 Como a memória virtual evitou um deadlock?**  
Sem a memória virtual, os processos que não couberam na RAM teriam que esperar os outros terminarem pra poder rodar. Isso poderia travar tudo. Mas com a paginação, mesmo os processos fora da RAM podem continuar rodando, evitando esse tipo de problema.

**🔹 Qual o impacto da desfragmentação no desempenho?**  
Ajuda sim, principalmente quando temos vários bloquinhos de espaço livre espalhados. No nosso caso não ajudou tanto porque o espaço livre ainda foi pequeno, mas em geral desfragmentar melhora a chance de rodar novos processos direto na RAM. Claro que tem o custo de mover as coisas na memória, mas pode valer a pena dependendo da situação.

---

## Conclusão

Essa atividade foi boa pra visualizar como o sistema gerencia a memória, especialmente quando ela não é suficiente. Entender o Best-Fit, a paginação e a desfragmentação me ajudou a perceber melhor os desafios de otimizar o uso da RAM.
