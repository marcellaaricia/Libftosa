# ft_memcmp

> ⚖️ Compara blocos de memória byte a byte

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_memcmp.c`

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
int ft_memcmp(const void *s1, const void *s2, size_t n);
```

> 💡 **Observação:** Compara dados binários, não para em terminadores nulos.

---

## 📖 DESCRIÇÃO

A função `ft_memcmp` compara os primeiros `n` bytes das áreas de memória `s1` e `s2`, retornando um valor que indica a relação lexicográfica entre elas.

### 🎯 Objetivo
Determinar se dois blocos de memória são iguais ou qual é "maior" em ordem lexicográfica.

### 🌍 Aplicações Reais
- **Verificação de integridade:** Comparar dados antes/depois de operações
- **Ordenação:** Comparar structs ou dados binários
- **Criptografia:** Verificar hashes e checksums
- **Debugging:** Comparar estado de estruturas

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s1` | `const void *` | Primeiro bloco de memória |
| `s2` | `const void *` | Segundo bloco de memória |
| `n` | `size_t` | Número de bytes a comparar |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| `s1 == s2` | `0` | Blocos são idênticos |
| `s1 > s2` | `> 0` | Primeiro bloco é "maior" |
| `s1 < s2` | `< 0` | Primeiro bloco é "menor" |

### 📊 Interpretação dos Resultados

| Exemplo | Resultado | Explicação |
|---------|-----------|------------|
| `memcmp("ABC", "ABC", 3)` | `0` | Strings iguais |
| `memcmp("ABC", "ABD", 3)` | `< 0` | 'C' < 'D' |
| `memcmp("ABZ", "ABC", 3)` | `> 0` | 'Z' > 'C' |
| `memcmp("Hello", "Hi", 2)` | `< 0` | 'e' < 'i' |

---

## 💡 COMO ENTENDER O CONCEITO

### ⚖️ Comparação Lexicográfica
```
Comparando "ABC" vs "ABD":

Byte 0: 'A' == 'A' → continua
Byte 1: 'B' == 'B' → continua  
Byte 2: 'C' != 'D' → retorna 'C' - 'D' = -1

Resultado: < 0 (primeiro é "menor")
```

### 🔢 Valores como unsigned char
```c
// Problema com char normal:
char a = -1;  // 0xFF
char b = 1;   // 0x01
a - b = -2    // Resultado errado!

// Solução com unsigned char:
unsigned char a = 255;  // 0xFF  
unsigned char b = 1;    // 0x01
a - b = 254             // Resultado correto!
```

### 🎯 Diferença de strcmp
```c
// strcmp para no primeiro \0
strcmp("AB\0CD", "AB\0CE")  → 0 (para no \0)

// memcmp continua comparando
memcmp("AB\0CD", "AB\0CE", 5)  → < 0 ('D' < 'E')
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_comparacao(const void *s1, const void *s2, size_t n, const char *desc)
{
    int resultado = ft_memcmp(s1, s2, n);
    
    printf("%s:\n", desc);
    if (resultado == 0)
        printf("   Resultado: 0 (iguais)\n");
    else if (resultado > 0)
        printf("   Resultado: %d (primeiro > segundo)\n", resultado);
    else
        printf("   Resultado: %d (primeiro < segundo)\n", resultado);
    printf("\n");
}

int main(void)
{
    printf("=== DEMONSTRAÇÃO MEMCMP ===\n\n");
    
    // Exemplo 1: Strings normais
    demonstrar_comparacao("Hello", "Hello", 5, "1. Strings iguais");
    demonstrar_comparacao("Apple", "Banana", 5, "2. Apple vs Banana");
    demonstrar_comparacao("Zebra", "Apple", 5, "3. Zebra vs Apple");
    
    // Exemplo 2: Dados com bytes zero
    char dados1[] = "AB\0CD";
    char dados2[] = "AB\0CE";
    demonstrar_comparacao(dados1, dados2, 5, "4. Dados binários com \\0");
    
    // Exemplo 3: Arrays de números
    int nums1[] = {1, 2, 3};
    int nums2[] = {1, 2, 4};
    demonstrar_comparacao(nums1, nums2, sizeof(nums1), "5. Arrays de inteiros");
    
    // Exemplo 4: Comparação limitada
    demonstrar_comparacao("Hello World", "Hello 42!!!", 5, "6. Comparação limitada (5 bytes)");
    
    // Exemplo 5: Bytes com valores altos
    unsigned char data1[] = {200, 100, 50};
    unsigned char data2[] = {200, 150, 50};
    demonstrar_comparacao(data1, data2, 3, "7. Bytes com valores altos");
    
    // Exemplo 6: Demonstração prática - verificação de senha
    printf("=== EXEMPLO PRÁTICO ===\n");
    char senha_correta[] = "secret123";
    char tentativa1[] = "secret123";
    char tentativa2[] = "secret124";
    
    if (ft_memcmp(senha_correta, tentativa1, 9) == 0)
        printf("Tentativa 1: ✅ Senha correta!\n");
    else
        printf("Tentativa 1: ❌ Senha incorreta!\n");
        
    if (ft_memcmp(senha_correta, tentativa2, 9) == 0)
        printf("Tentativa 2: ✅ Senha correta!\n");
    else
        printf("Tentativa 2: ❌ Senha incorreta!\n");
    
    return (0);
}
```

**Saída esperada:**
```
=== DEMONSTRAÇÃO MEMCMP ===

1. Strings iguais:
   Resultado: 0 (iguais)

2. Apple vs Banana:
   Resultado: -1 (primeiro < segundo)

3. Zebra vs Apple:
   Resultado: 25 (primeiro > segundo)

4. Dados binários com \0:
   Resultado: -1 (primeiro < segundo)

5. Arrays de inteiros:
   Resultado: -1 (primeiro < segundo)

6. Comparação limitada (5 bytes):
   Resultado: 0 (iguais)

7. Bytes com valores altos:
   Resultado: -50 (primeiro < segundo)

=== EXEMPLO PRÁTICO ===
Tentativa 1: ✅ Senha correta!
Tentativa 2: ❌ Senha incorreta!
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **unsigned char:** Por que usar ao invés de char
2. **Comparação lexicográfica:** Como funciona a ordenação
3. **Short-circuit:** Parar na primeira diferença
4. **Valor de retorno:** Diferença vs apenas sinal

### 🎯 Perguntas para Reflexão
- Por que usar unsigned char internamente?
- Qual a diferença entre memcmp e strcmp?
- Por que retornar a diferença e não apenas -1/0/1?
- Como memcmp trata dados binários?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Otimizada com n=0
```c
int ft_memcmp(const void *s1, const void *s2, size_t n)
{
    const unsigned char *p1, *p2;
    size_t i;
    
    if (n == 0)
        return (0);  // Otimização
        
    p1 = (const unsigned char *)s1;
    p2 = (const unsigned char *)s2;
    
    for (i = 0; i < n; i++)
    {
        if (p1[i] != p2[i])
            return (p1[i] - p2[i]);
    }
    return (0);
}
```

### 💭 Abordagem 2: Ponteiros Móveis
```c
int ft_memcmp(const void *s1, const void *s2, size_t n)
{
    const unsigned char *p1 = s1;
    const unsigned char *p2 = s2;
    
    while (n--)
    {
        if (*p1 != *p2)
            return (*p1 - *p2);
        p1++;
        p2++;
    }
    return (0);
}
```

### 🔧 Dicas de Implementação
- **Cast obrigatório:** void* → unsigned char* para comparar
- **Primeira diferença:** Retornar imediatamente quando diferente
- **Otimização n=0:** Verificar caso especial primeiro
- **Valores corretos:** unsigned char evita problemas com sinais

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_memcmp básica
2. Teste com strings e dados binários
3. Verifique comportamento com n=0

### 🥈 Nível Intermediário
1. Implemente versão otimizada para word-size
2. Crie função que retorna apenas sinal (-1/0/1)
3. Teste com diferentes tipos de dados

### 🥇 Nível Avançado
1. Implemente comparação case-insensitive
2. Crie versão que ignora diferenças específicas
3. Otimize usando instruções SIMD

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria
- [`ft_memchr`](ft_memchr.md) - Busca byte em memória
- [`ft_memcpy`](ft_memcpy.md) - Copia blocos de memória
- [`ft_memmove`](ft_memmove.md) - Move memória com sobreposição
- [`ft_memset`](ft_memset.md) - Preenche memória com valor
- [`ft_bzero`](ft_bzero.md) - Zera memória

### 🔄 Funções String Relacionadas
- [`ft_strncmp`](../string/ft_strncmp.md) - Compara strings com limite
- [`ft_strcmp`](../string/ft_strcmp.md) - Compara strings completas

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [⚖️ Comparação Lexicográfica](../../resources/lexicographic_comparison.md)
- [🔢 unsigned vs signed](../../resources/signed_unsigned.md)
- [🎯 Otimização de Comparações](../../resources/comparison_optimization.md)

### 🔗 Referências Externas
- Manual do C: `man memcmp`
- [C Reference - Memory Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender por que usar unsigned char
- [ ] Diferença entre memcmp e strcmp
- [ ] Interpretar valores de retorno (não apenas sinal)
- [ ] Casos especiais (n=0, ponteiros iguais)

**Descobertas importantes:**
- [ ] Comparação lexicográfica em dados binários
- [ ] Importância do cast para unsigned char
- [ ] Short-circuit na primeira diferença
- [ ] Aplicações práticas em verificação de dados

**Testes que fiz:**
- [ ] Strings iguais e diferentes
- [ ] Dados binários com bytes zero
- [ ] Arrays de diferentes tipos
- [ ] Casos limite (n=0, mesmos ponteiros)

---
<div align="center">

[← Função Anterior: ft_memchr](ft_memchr.md) | [Próxima Função: ft_calloc →](ft_calloc.md)

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
