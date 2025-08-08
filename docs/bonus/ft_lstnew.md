# ft_lstnew

> 🔗 Cria um novo nó para lista ligada (linked list)

---

**Categoria:** [Funções de Lista](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_lstnew_bonus.c`

---

## ⚠️ AVISO EDUCATIVO

**Este material é puramente didático e NÃO contém implementações completas.**

Se você é estudante da 42, use apenas para **entender conceitos** e desenvolva sua própria solução.

---

## 📋 SINOPSE

**Arquivo de Cabeçalho:**
```c
#include "libft.h"
```

**Protótipo:**
```c
t_list *ft_lstnew(void *content);
```

**Estrutura:**
```c
typedef struct s_list
{
    void            *content;
    struct s_list   *next;
} t_list;
```

> 💡 **Observação:** Primeiro passo para trabalhar com listas ligadas dinâmicas.

---

## 📖 DESCRIÇÃO

A função `ft_lstnew` aloca memória para um novo nó de lista ligada e **inicializa seus campos** com o conteúdo fornecido e ponteiro next como NULL.

### 🎯 Objetivo
Criar a unidade básica (nó) para construir estruturas de dados dinâmicas como listas ligadas.

### 🌍 Aplicações Reais
- **Listas de tarefas:** Cada nó representa uma tarefa
- **Histórico de navegação:** Cada página visitada é um nó
- **Playlist de música:** Cada música é um nó na lista
- **Estruturas complexas:** Base para stacks, queues, etc.

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `content` | `void *` | Ponteiro para dados a serem armazenados no nó |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Sucesso | `t_list *` | Ponteiro para o novo nó criado |
| Erro | `NULL` | Falha na alocação de memória |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Explicação |
|---------|-----------|------------|
| `lstnew("Hello")` | Nó com content="Hello", next=NULL | String armazenada |
| `lstnew(&num)` | Nó com content=&num, next=NULL | Endereço de int |
| `lstnew(NULL)` | Nó com content=NULL, next=NULL | Nó vazio válido |
| Falha malloc | NULL | Sem memória disponível |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Estrutura Básica
```c
typedef struct s_list
{
    void            *content;    // Dados do nó
    struct s_list   *next;      // Próximo nó (NULL se último)
} t_list;
```

### 🔗 Anatomia de um Nó
```
┌─────────────────────┐
│     NOVO NÓ         │
├───────────┬─────────┤
│ content   │  next   │
│ (dados)   │  NULL   │
└───────────┴─────────┘
```

### 🎯 Casos de Uso Visuais

#### Nó com String
```c
t_list *node = ft_lstnew("Hello World");

┌─────────────────────┐
│       NODE          │
├───────────┬─────────┤
│ content → │  next   │
│"Hello..."│  NULL   │
└───────────┴─────────┘
           │
           ▼
   "Hello World"
```

#### Nó com Número
```c
int num = 42;
t_list *node = ft_lstnew(&num);

┌─────────────────────┐
│       NODE          │
├───────────┬─────────┤
│ content → │  next   │
│    &num  │  NULL   │
└───────────┴─────────┘
           │
           ▼
         num = 42
```

### ⚠️ Conceitos Importantes

#### 🔍 void* - Ponteiro Genérico
```c
// Pode apontar para QUALQUER tipo
int num = 42;
char *str = "Hello";
struct pessoa p = {...};

t_list *n1 = ft_lstnew(&num);    // int*
t_list *n2 = ft_lstnew(str);     // char*
t_list *n3 = ft_lstnew(&p);      // struct*
```

#### 🚨 Responsabilidade de Dados
```c
// ❌ PERIGO: dados locais podem sumir!
t_list *criar_node_perigoso(void)
{
    int local = 42;                    // Variável local!
    return ft_lstnew(&local);          // ❌ local será destruída!
}

// ✅ CORRETO: dados persistentes
t_list *criar_node_seguro(void)
{
    int *num = malloc(sizeof(int));    // Memória persistente
    *num = 42;
    return ft_lstnew(num);             // ✅ Seguro
}
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_criacao_basica(void)
{
    printf("=== CRIAÇÃO BÁSICA DE NÓS ===\n\n");
    
    // Nó com string
    t_list *node1 = ft_lstnew("Primeiro nó");
    if (node1)
    {
        printf("Nó 1 criado:\n");
        printf("  Content: %s\n", (char *)node1->content);
        printf("  Next: %p (deve ser NULL)\n", node1->next);
        free(node1);
    }
    
    // Nó com número
    int *num = malloc(sizeof(int));
    *num = 42;
    t_list *node2 = ft_lstnew(num);
    if (node2)
    {
        printf("\nNó 2 criado:\n");
        printf("  Content: %d\n", *(int *)node2->content);
        printf("  Next: %p (deve ser NULL)\n", node2->next);
        free(num);
        free(node2);
    }
    
    // Nó vazio (NULL content)
    t_list *node3 = ft_lstnew(NULL);
    if (node3)
    {
        printf("\nNó 3 (vazio) criado:\n");
        printf("  Content: %p (NULL)\n", node3->content);
        printf("  Next: %p (deve ser NULL)\n", node3->next);
        free(node3);
    }
    
    printf("\n");
}

void exemplo_tipos_diferentes(void)
{
    printf("=== DIFERENTES TIPOS DE DADOS ===\n\n");
    
    // Struct personalizada
    typedef struct {
        char name[20];
        int age;
        float score;
    } Student;
    
    Student *aluno = malloc(sizeof(Student));
    strcpy(aluno->name, "João");
    aluno->age = 20;
    aluno->score = 8.5;
    
    t_list *student_node = ft_lstnew(aluno);
    if (student_node)
    {
        Student *s = (Student *)student_node->content;
        printf("Nó com struct Student:\n");
        printf("  Nome: %s\n", s->name);
        printf("  Idade: %d\n", s->age);
        printf("  Nota: %.1f\n\n", s->score);
        
        free(aluno);
        free(student_node);
    }
    
    // Array de caracteres
    char *texto = malloc(50);
    strcpy(texto, "Texto dinâmico alocado");
    
    t_list *text_node = ft_lstnew(texto);
    if (text_node)
    {
        printf("Nó com texto dinâmico:\n");
        printf("  Conteúdo: %s\n\n", (char *)text_node->content);
        
        free(texto);
        free(text_node);
    }
}

void demonstrar_inicio_lista(void)
{
    printf("=== INICIANDO UMA LISTA ===\n\n");
    
    // Criar primeiro nó da lista
    char *primeiro = malloc(20);
    strcpy(primeiro, "Primeiro");
    
    t_list *head = ft_lstnew(primeiro);
    
    // Criar segundo nó
    char *segundo = malloc(20);
    strcpy(segundo, "Segundo");
    
    t_list *segundo_no = ft_lstnew(segundo);
    
    // Conectar os nós
    head->next = segundo_no;
    
    printf("Lista criada:\n");
    printf("Head → [%s] → [%s] → NULL\n\n", 
           (char *)head->content, 
           (char *)head->next->content);
    
    // Percorrer a lista
    printf("Percorrendo a lista:\n");
    t_list *current = head;
    int i = 1;
    while (current)
    {
        printf("  Nó %d: %s\n", i++, (char *)current->content);
        current = current->next;
    }
    
    // Limpeza
    free(primeiro);
    free(segundo);
    free(head);
    free(segundo_no);
    
    printf("\n");
}

void testar_falhas_alocacao(void)
{
    printf("=== TESTE DE FALHAS ===\n\n");
    
    // Teste com dados válidos
    t_list *node = ft_lstnew("Teste");
    if (node)
    {
        printf("✅ Criação normal funcionou\n");
        printf("   Content: %s\n", (char *)node->content);
        free(node);
    }
    else
        printf("❌ Falha inesperada na criação\n");
    
    // Teste com NULL
    t_list *null_node = ft_lstnew(NULL);
    if (null_node)
    {
        printf("✅ Criação com NULL funcionou\n");
        printf("   Content: %p (deve ser NULL)\n", null_node->content);
        free(null_node);
    }
    else
        printf("❌ Falha com content NULL\n");
    
    printf("\n💡 Em caso de falha na alocação, malloc retornaria NULL\n\n");
}

int main(void)
{
    printf("🔗 DEMONSTRAÇÃO FT_LSTNEW\n");
    printf("=========================\n\n");
    
    demonstrar_criacao_basica();
    exemplo_tipos_diferentes();
    demonstrar_inicio_lista();
    testar_falhas_alocacao();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • ft_lstnew cria UM nó isolado\n");
    printf("   • next sempre começa NULL\n");
    printf("   • content pode ser qualquer tipo\n");
    printf("   • Gerenciar memória dos dados é sua responsabilidade!\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔗 DEMONSTRAÇÃO FT_LSTNEW
=========================

=== CRIAÇÃO BÁSICA DE NÓS ===

Nó 1 criado:
  Content: Primeiro nó
  Next: (nil) (deve ser NULL)

Nó 2 criado:
  Content: 42
  Next: (nil) (deve ser NULL)

Nó 3 (vazio) criado:
  Content: (nil) (NULL)
  Next: (nil) (deve ser NULL)

=== DIFERENTES TIPOS DE DADOS ===

Nó com struct Student:
  Nome: João
  Idade: 20
  Nota: 8.5

Nó com texto dinâmico:
  Conteúdo: Texto dinâmico alocado

=== INICIANDO UMA LISTA ===

Lista criada:
Head → [Primeiro] → [Segundo] → NULL

Percorrendo a lista:
  Nó 1: Primeiro
  Nó 2: Segundo

=== TESTE DE FALHAS ===

✅ Criação normal funcionou
   Content: Teste
✅ Criação com NULL funcionou
   Content: (nil) (deve ser NULL)

💡 Em caso de falha na alocação, malloc retornaria NULL

💡 LEMBRE-SE:
   • ft_lstnew cria UM nó isolado
   • next sempre começa NULL
   • content pode ser qualquer tipo
   • Gerenciar memória dos dados é sua responsabilidade!
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Estruturas (struct):** Como definir e usar
2. **Self-referencing structs:** struct que aponta para si mesma

### 🎯 Perguntas para Reflexão
- Por que usar void* ao invés de um tipo específico?
- O que acontece se malloc falhar?
- Por que next deve começar como NULL?
- Como esta função se relaciona com outras funções de lista?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem Direta
```c
t_list *ft_lstnew(void *content)
{
    t_list *new_node;
    
    // Aloca memória para o nó
    new_node = malloc(sizeof(t_list));
    if (!new_node)
        return (NULL);
    
    // Inicializa os campos
    new_node->content = content;
    new_node->next = NULL;
    
    return (new_node);
}
```

### 💭 Abordagem com Validação Extra
```c
t_list *ft_lstnew(void *content)
{
    t_list *new_node;
    
    // Aloca e verifica
    new_node = (t_list *)malloc(sizeof(t_list));
    if (new_node == NULL)
        return (NULL);
    
    // Inicialização explícita
    new_node->content = content;
    new_node->next = (t_list *)NULL;
    
    return (new_node);
}
```

### 🔧 Dicas de Implementação
- **Verificar malloc:** Sempre verificar se alocação funcionou
- **Inicializar tudo:** content e next devem ser definidos
- **Não validar content:** NULL é um valor válido para content
- **Retorno consistente:** NULL para erro, ponteiro válido para sucesso

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_lstnew básica
2. Teste com diferentes tipos de dados
3. Verifique comportamento com content=NULL

### 🥈 Nível Intermediário
1. Crie função para imprimir o conteúdo de um nó
2. Implemente contador de nós criados
3. Crie versão que aceita função de inicialização

### 🥇 Nível Avançado
1. Implemente versão com pool de memória pré-alocada
2. Crie ft_lstnew_range para criar múltiplos nós
3. Adicione debug info (timestamp, ID único)

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - Lista
- [`ft_lstadd_front`](ft_lstadd_front.md) - Adiciona nó no início
- [`ft_lstadd_back`](ft_lstadd_back.md) - Adiciona nó no final
- [`ft_lstsize`](ft_lstsize.md) - Conta nós da lista
- [`ft_lstlast`](ft_lstlast.md) - Encontra último nó
- [`ft_lstdelone`](ft_lstdelone.md) - Deleta um nó
- [`ft_lstclear`](ft_lstclear.md) - Limpa lista inteira
- [`ft_lstiter`](ft_lstiter.md) - Itera sobre lista
- [`ft_lstmap`](ft_lstmap.md) - Mapeia função sobre lista

### 🔄 Dependências
- `malloc` - Para alocação de memória
- `free` - Para liberação (em outras funções)

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔗 Listas Ligadas](../../resources/linked_lists.md)
- [🧠 Estruturas de Dados](../../resources/data_structures.md)
- [💾 Gerenciamento de Memória](../../resources/memory_management.md)

### 🔗 Referências Externas
- [Linked Lists em C](https://www.learn-c.org/en/Linked_lists)
- [Data Structures Visualization](https://visualgo.net/en/list)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender ponteiros para struct
- [ ] Conceito de void* (ponteiro genérico)
- [ ] Diferença entre dados locais e dinâmicos
- [ ] Self-referencing structs (struct que aponta para si)

**Descobertas importantes:**
- [ ] ft_lstnew é a base de todas operações de lista
- [ ] void* permite flexibilidade total de tipos
- [ ] next=NULL indica nó isolado/último
- [ ] Responsabilidade pelos dados é do programador

**Testes que fiz:**
- [ ] Diferentes tipos de dados
- [ ] Content NULL vs malloc failure
- [ ] Verificação de inicialização correta
- [ ] Conexão manual de nós

---
<div align="center">

[← Função Anterior: ft_calloc](../memory/ft_calloc.md) | [Próxima Função: ft_lstadd_front →](ft_lstadd_front.md)

🔗 [Funções de Lista](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>