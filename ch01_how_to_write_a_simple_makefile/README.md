# Registro de Colisão: Automação de Build com GNU Make

### 1. OVERVIEW
O objetivo foi construir um sistema de build automatizado para um projeto C que detectasse mudanças em dependências de cabeçalho, resultando na compreensão de que o `make` é um motor de decisão baseado em estados de arquivos (timestamps), e não apenas um executor de scripts.

---

### 2. TECHNICAL CONTEXT
* **Core Concept:** **Grafo de Dependências Dirigido (DAG)** e **Invalidação de Cache por Timestamp**. O `make` reconstrói apenas os nós do grafo onde o "pai" (alvo) é mais antigo que o "filho" (pré-requisito).
* **Tools Used:** `GNU Make`, `GCC` (Compiler), `LD` (Linker), `WSL/Ubuntu Shell`.

---

### 3. DEBUGGING & LEARNING
* **Issue:** Ocorreram três falhas críticas:
    1.  **Missing Separator:** O `make` falhou por causa de espaços em vez de `TAB` (Sintaxe rigorosa).
    2.  **Silent Failure (The Trap):** Ao alterar o arquivo `.h`, o sistema afirmou estar "up to date" e não refletiu as mudanças de segurança no binário final.
* **Solution:**A inclusão explícita do arquivo de cabeçalho (`auth.h`) na lista de pré-requisitos da regra do objeto.
* **Technical Learning:** Aprendi que o compilador enxerga o `#include`, mas o `make` é cego para ele. Se a dependência não for mapeada explicitamente no `Makefile`, o sistema criará um "Build Sujo", onde o código-fonte e o executável estão em estados divergentes.

---

### 4. HOW TO REPRODUCE
1.  **Configurar o Caos:** Crie os arquivos `main.c`, `auth.c` e `auth.h` (com `MIN_PASSWORD_LENGTH 4`).
2.  **Build Inicial:** Crie um `Makefile` sem listar o `.h` como dependência e execute `make`. Verifique com `./app`.
3.  **Simular a Falha:** Altere o valor no `.h` para `12` e rode `make` novamente. Observe o erro: o `make` ignorará a mudança.
4.  **Aplicar a Vacina:** Atualize a regra no `Makefile` para `auth.o: auth.c auth.h`. Execute `make` e veja o `gcc` ser invocado automaticamente para aplicar a correção.
