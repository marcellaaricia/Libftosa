# ft_calloc

> 💾 Aloca memória zerada (malloc + bzero em um só)

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_calloc.c`

---

## ⚠️ AVISO EDUCATIVO

**Este material é puramente didático e NÃO contém implementações completas.**

Se você é estudante da 42, use apenas para **entender conceitos** e desenvolva sua própria solução.

---

## 📋 SINOPSE

**Arquivo de Cabeçalho:**
```c
#include "libft.h"
#include <stdint.h>  // Para SIZE_MAX
```

**Protótipo:**
```c
void *ft_calloc(size_t count, size_t size);
```

> 💡 **Observação:** Combina alocação + inicialização com zeros automaticamente.

---

## 📖 DESCRIÇÃO

A função `ft_calloc` aloca memória para um array de `count` elementos, cada um com `size` bytes, e **inicializa todos os bytes com zero**.

### 🎯 Objetivo
Fornecer memória limpa e pronta para uso, eliminando lixo de memória não inicializada.

### 🌍 Aplicações Reais
- **Arrays dinâmicos:** Criar arrays zerados de qualquer tipo
- **Estruturas limpas:** Alocar structs sem lixo de memória
- **Buffers seguros:** Garantir que dados sensíveis começam zerados
- **Matrizes:** Alocar matrizes 2D inicializadas

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `count` | `size_t` | Número de elementos |
| `size` | `size_t` | Tamanho de cada elemento em bytes |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Sucesso | `void *` | Ponteiro para memória alocada e zerada |
| Erro | `NULL` | Falha na alocação ou overflow |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Explicação |
|---------|-----------|------------|
| `calloc(5, sizeof(int))` | Array de 5 ints = 0 | 20 bytes zerados |
| `calloc(10, 1)` | 10 bytes = 0 | String vazia de 10 chars |
| `calloc(0, size)` | Comportamento definido | Depende da implementação |
| `calloc(SIZE_MAX, SIZE_MAX)` | NULL | Overflow detectado |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Fórmula Básica
```c
calloc(count, size) = malloc(count * size) + bzero(ptr, count * size)
```

### 🆚 Diferença do malloc
```c
// malloc: memória com LIXO
int *nums1 = malloc(5 * sizeof(int));
// nums1 = {-1234567, 0, 999999, -42, 0}  ← lixo aleatório!

// calloc: memória LIMPA
int *nums2 = calloc(5, sizeof(int));
// nums2 = {0, 0, 0, 0, 0}  ← sempre zeros!
```

### ⚠️ Problema do Overflow
```c
// PERIGO: count * size pode estourar!
size_t count = 1000000000;  // 1 bilhão
size_t size = 1000000000;   // 1 bilhão
// count * size = 1000000000000000000 > SIZE_MAX !!

// SOLUÇÃO: verificar ANTES de multiplicar
if (count > SIZE_MAX / size)
    return NULL;  // Evita overflow
```

### 🔍 Detecção de Overflow
```
SIZE_MAX = maior valor possível para size_t

Se count > SIZE_MAX / size:
   count * size > SIZE_MAX  ← OVERFLOW!
   
Exemplo:
SIZE_MAX = 18446744073709551615 (64-bit)
count = 10000000000
size = 10000000000

SIZE_MAX / size = 1844...
count > 1844... ← TRUE = overflow!
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_diferenca_malloc_calloc(void)
{
    printf("=== MALLOC vs CALLOC ===\n\n");
    
    // malloc: pode ter lixo
    int *dirty = malloc(5 * sizeof(int));
    printf("malloc (pode ter lixo): ");
    for (int i = 0; i < 5; i++)
        printf("%d ", dirty[i]);
    printf("\n");
    
    // calloc: sempre zerado
    int *clean = ft_calloc(5, sizeof(int));
    printf("calloc (sempre limpo): ");
    for (int i = 0; i < 5; i++)
        printf("%d ", clean[i]);
    printf("\n\n");
    
    free(dirty);
    free(clean);
}

void exemplo_estruturas(void)
{
    printf("=== ESTRUTURAS ZERADAS ===\n\n");
    
    typedef struct {
        int id;
        char name[20];
        double salary;
    } Employee;
    
    // Aloca 3 funcionários zerados
    Employee *staff = ft_calloc(3, sizeof(Employee));
    
    if (staff)
    {
        printf("Funcionário 0: id=%d name=\"%s\" salary=%.2f\n",
               staff[0].id, staff[0].name, staff[0].salary);
               
        // Agora podemos usar sem medo de lixo
        staff[0].id = 42;
        strcpy(staff[0].name, "João");
        staff[0].salary = 5000.0;
        
        printf("Após preenchimento: id=%d name=\"%s\" salary=%.2f\n",
               staff[0].id, staff[0].name, staff[0].salary);
        
        free(staff);
    }
    printf("\n");
}

void exemplo_overflow_protection(void)
{
    printf("=== PROTEÇÃO CONTRA OVERFLOW ===\n\n");
    
    // Tentativa de overflow
    printf("Tentando alocar SIZE_MAX * SIZE_MAX bytes...\n");
    void *ptr = ft_calloc(SIZE_MAX, SIZE_MAX);
    
    if (ptr == NULL)
        printf("✅ Overflow detectado e bloqueado!\n");
    else
    {
        printf("❌ ERRO: Deveria ter bloqueado!\n");
        free(ptr);
    }
    
    // Alocação normal
    printf("\nTentando alocação normal (100 ints)...\n");
    int *nums = ft_calloc(100, sizeof(int));
    
    if (nums)
    {
        printf("✅ Alocação normal funcionou!\n");
        printf("Primeiro elemento: %d (deve ser 0)\n", nums[0]);
        free(nums);
    }
    else
        printf("❌ Falha inesperada na alocação normal\n");
        
    printf("\n");
}

int main(void)
{
    printf("🧪 DEMONSTRAÇÃO FT_CALLOC\n");
    printf("========================\n\n");
    
    demonstrar_diferenca_malloc_calloc();
    exemplo_estruturas();
    exemplo_overflow_protection();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • calloc = malloc + bzero\n");
    printf("   • Sempre verificar overflow\n");
    printf("   • Memória sempre vem zerada\n");
    printf("   • Não esquecer do free()!\n");
    
    return (0);
}
```

**Saída esperada:**
```
🧪 DEMONSTRAÇÃO FT_CALLOC
========================

=== MALLOC vs CALLOC ===

malloc (pode ter lixo): -123456 0 999999 -42 0 
calloc (sempre limpo): 0 0 0 0 0 

=== ESTRUTURAS ZERADAS ===

Funcionário 0: id=0 name="" salary=0.00
Após preenchimento: id=42 name="João" salary=5000.00

=== PROTEÇÃO CONTRA OVERFLOW ===

Tentando alocar SIZE_MAX * SIZE_MAX bytes...
✅ Overflow detectado e bloqueado!

Tentando alocação normal (100 ints)...
✅ Alocação normal funcionou!
Primeiro elemento: 0 (deve ser 0)

💡 LEMBRE-SE:
   • calloc = malloc + bzero
   • Sempre verificar overflow
   • Memória sempre vem zerada
   • Não esquecer do free()!
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Overflow aritmético:** Como `size_t` pode estourar
2. **SIZE_MAX:** Constante que define limite máximo
3. **Alocação dinâmica:** Como malloc funciona
4. **Inicialização de memória:** Por que zeros são importantes

### 🎯 Perguntas para Reflexão
- Por que calloc existe se temos malloc + bzero?
- Como detectar overflow antes de calcular?
- Qual o comportamento com count=0 ou size=0?
- Por que inicializar com zeros é importante?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Proteção Completa
```c
void *ft_calloc(size_t count, size_t size)
{
    void *ptr;
    
    // Casos especiais
    if (count == 0 || size == 0)
        return (malloc(0));
    
    // Proteção overflow
    if (count > SIZE_MAX / size)
        return (NULL);
    
    // Aloca e zera
    ptr = malloc(count * size);
    if (ptr)
        ft_bzero(ptr, count * size);
        
    return (ptr);
}
```

### 💭 Abordagem 2: Verificação Alternativa
```c
void *ft_calloc(size_t count, size_t size)
{
    size_t total;
    void *ptr;
    
    // Verificação alternativa de overflow
    if (size != 0 && count > (size_t)-1 / size)
        return (NULL);
        
    total = count * size;
    ptr = malloc(total);
    if (ptr)
        memset(ptr, 0, total);
        
    return (ptr);
}
```

### 🔧 Dicas de Implementação
- **Verificar overflow ANTES:** Nunca calcule `count * size` se pode estourar
- **Usar SIZE_MAX:** Mais claro que `(size_t)-1`
- **Casos especiais:** Decidir comportamento para count=0 ou size=0
- **Dependências:** Usar ft_bzero ou memset para zeragem

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_calloc básica sem proteção overflow
2. Teste com arrays de diferentes tipos
3. Compare com calloc original

### 🥈 Nível Intermediário
1. Adicione proteção completa contra overflow
2. Teste casos especiais (count=0, size=0)
3. Crie versão que usa ft_memset ao invés de ft_bzero

### 🥇 Nível Avançado
1. Implemente aligned_calloc (memória alinhada)
2. Crie calloc com padrão customizado (não apenas zeros)
3. Otimize para reduzir chamadas de sistema

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - Dependências
- [`ft_bzero`](ft_bzero.md) - Usada para zerar a memória alocada
- [`ft_memset`](ft_memset.md) - Alternativa para inicialização

### 🔄 Funções de Alocação
- `malloc` - Alocação sem inicialização
- `realloc` - Redimensiona memória alocada
- `free` - Libera memória alocada

### 📝 Funções que Usam Alocação
- [`ft_strdup`](../string/ft_strdup.md) - Duplica strings usando malloc
- [`ft_split`](../additional/ft_split.md) - Cria arrays de strings

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [💾 Gerenciamento de Memória](../../resources/memory_management.md)
- [⚠️ Overflow Protection](../../resources/overflow_protection.md)
- [🧹 Inicialização de Memória](../../resources/memory_initialization.md)

### 🔗 Referências Externas
- Manual do C: `man calloc`
- [C Reference - Memory Management](https://en.cppreference.com/w/c/memory)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender como detectar overflow sem calculá-lo
- [ ] Decidir comportamento para casos especiais
- [ ] Diferença entre calloc e malloc + bzero
- [ ] Importância da constante SIZE_MAX

**Descobertas importantes:**
- [ ] calloc é mais que conveniência - é segurança
- [ ] Overflow pode ser detectado matematicamente
- [ ] Memória não inicializada é fonte de bugs
- [ ] Casos especiais (0, 0) precisam ser tratados

**Testes que fiz:**
- [ ] Arrays de diferentes tipos
- [ ] Estruturas complexas
- [ ] Casos de overflow
- [ ] Casos especiais (count=0, size=0)

---
<div align="center">

[← Função Anterior: ft_memcmp](ft_memcmp.md) | [Próxima Categoria: Funções Adicionais →](../additional/README.md)

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
