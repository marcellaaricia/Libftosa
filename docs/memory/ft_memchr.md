# ft_memchr

> 🔍 Busca byte específico em bloco de memória

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_memchr.c`

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
void *ft_memchr(const void *s, int c, size_t n);
```

> 💡 **Observação:** Busca em dados binários, não apenas strings.

---

## 📖 DESCRIÇÃO

A função `ft_memchr` localiza a primeira ocorrência do byte `c` nos primeiros `n` bytes da área de memória apontada por `s`.

### 🎯 Objetivo
Encontrar um byte específico em qualquer tipo de dados binários, independentemente de terminadores nulos.

### 🌍 Aplicações Reais
- **Parsing de dados:** Encontrar delimitadores em arquivos binários
- **Buffers de rede:** Localizar marcadores em streams
- **Análise de memória:** Debug e inspeção de estruturas
- **Processamento de imagens:** Buscar patterns em dados raw

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s` | `const void *` | Ponteiro para área de memória |
| `c` | `int` | Byte a buscar (apenas 8 bits são usados) |
| `n` | `size_t` | Número máximo de bytes a examinar |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Encontrado | `void *` | Ponteiro para primeira ocorrência |
| Não encontrado | `NULL` | Byte não existe nos primeiros `n` bytes |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Explicação |
|---------|-----------|------------|
| `memchr("Hello", 'l', 5)` | Pont. para 1º 'l' | Encontra na posição 2 |
| `memchr("Hello", 'x', 5)` | `NULL` | Não existe nos 5 bytes |
| `memchr(data, '\0', 10)` | Pont. para '\0' | Encontra terminador |
| `memchr(data, 300, 5)` | Busca byte 44 | 300 & 0xFF = 44 |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔍 Busca Linear
```
Memória: ['H']['e']['l']['l']['o'][\0]
Busca 'l' nos primeiros 5 bytes:
         ↓    ↓    ✅   ↓    ↓
Resultado: ponteiro para posição 2
```

### 🎯 Diferença de strchr
```c
// strchr para na primeira \0
strchr("AB\0CD", 'C')  → NULL (para no \0)

// memchr continua buscando
memchr("AB\0CD", 'C', 5)  → ponteiro para 'C'
```

### 🔢 Truncação de Bytes
```c
memchr(data, 300, n)  // 300 = 0x012C
                      // Usa apenas 0x2C = 44
                      
memchr(data, -1, n)   // -1 = 0xFFFFFFFF  
                      // Usa apenas 0xFF = 255
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

int main(void)
{
    printf("=== DEMONSTRAÇÃO MEMCHR ===\n\n");
    
    // Exemplo 1: Busca em string normal
    char texto[] = "Hello, World!";
    char *pos = ft_memchr(texto, ',', strlen(texto));
    
    printf("1. Busca ',' em \"%s\":\n", texto);
    if (pos)
        printf("   Encontrado na posição %ld: \"%s\"\n", pos - texto, pos);
    else
        printf("   Não encontrado\n");
    
    // Exemplo 2: Dados com bytes zero
    char dados[] = "ABC\0DEF\0GHI";  // 11 bytes total
    pos = ft_memchr(dados, 'E', 11);
    
    printf("\n2. Busca 'E' em dados binários (11 bytes):\n");
    printf("   Dados: ABC\\0DEF\\0GHI\n");
    if (pos)
        printf("   Encontrado na posição %ld\n", pos - dados);
    
    // Exemplo 3: Busca limitada por n
    pos = ft_memchr(texto, 'o', 5);  // Busca só em "Hello"
    printf("\n3. Busca 'o' limitada a 5 bytes:\n");
    printf("   Região: \"%.5s\"\n", texto);
    if (pos)
        printf("   Encontrado na posição %ld\n", pos - texto);
    else
        printf("   Não encontrado na região limitada\n");
    
    // Exemplo 4: Busca byte específico
    unsigned char buffer[] = {65, 66, 255, 68, 69};  // A, B, 255, D, E
    pos = ft_memchr(buffer, 255, 5);
    
    printf("\n4. Busca byte 255 em buffer:\n");
    printf("   Buffer: {65, 66, 255, 68, 69}\n");
    if (pos)
        printf("   Byte 255 encontrado na posição %ld\n", pos - (char*)buffer);
    
    // Exemplo 5: Valor > 255 (truncação)
    pos = ft_memchr(buffer, 300, 5);  // 300 & 0xFF = 44
    printf("\n5. Busca valor 300 (truncado para %d):\n", 300 & 0xFF);
    if (pos)
        printf("   Encontrado\n");
    else
        printf("   Não encontrado (300 truncado = 44, não existe no buffer)\n");
    
    return (0);
}
```

**Saída esperada:**
```
=== DEMONSTRAÇÃO MEMCHR ===

1. Busca ',' em "Hello, World!":
   Encontrado na posição 5: ", World!"

2. Busca 'E' em dados binários (11 bytes):
   Dados: ABC\0DEF\0GHI
   Encontrado na posição 5

3. Busca 'o' limitada a 5 bytes:
   Região: "Hello"
   Encontrado na posição 4

4. Busca byte 255 em buffer:
   Buffer: {65, 66, 255, 68, 69}
   Byte 255 encontrado na posição 2

5. Busca valor 300 (truncado para 44):
   Não encontrado (300 truncado = 44, não existe no buffer)
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Dados binários vs strings:** Diferença fundamental
2. **Truncação de int para byte:** Como `int c` vira `unsigned char`
3. **Ponteiros void:** Cast e aritmética de ponteiros
4. **Busca limitada:** Importância do parâmetro `n`

### 🎯 Perguntas para Reflexão
- Por que memchr não para em '\0' como strchr?
- Como funciona a truncação de valores > 255?
- Quando usar memchr ao invés de strchr?
- Por que retornar void* ao invés de unsigned char*?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Loop com Indexing
```c
void *ft_memchr(const void *s, int c, size_t n)
{
    const unsigned char *ptr = (const unsigned char *)s;
    unsigned char byte = (unsigned char)c;
    size_t i = 0;
    
    while (i < n)
    {
        if (ptr[i] == byte)
            return ((void *)(ptr + i));
        i++;
    }
    return (NULL);
}
```

### 💭 Abordagem 2: Ponteiros Móveis
```c
void *ft_memchr(const void *s, int c, size_t n)
{
    const unsigned char *ptr = (const unsigned char *)s;
    unsigned char byte = (unsigned char)c;
    
    while (n--)
    {
        if (*ptr == byte)
            return ((void *)ptr);
        ptr++;
    }
    return (NULL);
}
```

### 🔧 Dicas de Implementação
- **Cast obrigatório:** void* → unsigned char* para desreferenciar
- **Truncação:** `(unsigned char)c` garante apenas 8 bits
- **Retorno:** Cast de volta para void* (compatibilidade)
- **Verificação de limite:** Sempre respeitar parâmetro `n`

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_memchr com loop de indexing
2. Teste com strings e dados binários
3. Verifique comportamento com valores > 255

### 🥈 Nível Intermediário
1. Implemente versão com aritmética de ponteiros
2. Crie função que encontra todas as ocorrências
3. Otimize para busca de múltiplos bytes simultaneamente

### 🥇 Nível Avançado
1. Implemente busca usando word-size operations
2. Crie memchr reversa (busca do fim para início)
3. Implemente algoritmo Boyer-Moore para padrões

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria
- [`ft_memcmp`](ft_memcmp.md) - Compara blocos de memória
- [`ft_memcpy`](ft_memcpy.md) - Copia blocos de memória
- [`ft_memmove`](ft_memmove.md) - Move memória com sobreposição
- [`ft_memset`](ft_memset.md) - Preenche memória com valor
- [`ft_bzero`](ft_bzero.md) - Zera memória

### 🔄 Funções String Relacionadas
- [`ft_strchr`](../string/ft_strchr.md) - Busca caractere em string
- [`ft_strrchr`](../string/ft_strrchr.md) - Busca do final para início
- [`ft_strnstr`](../string/ft_strnstr.md) - Busca substring

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔍 Algoritmos de Busca](../../resources/search_algorithms.md)
- [🎯 Dados Binários vs Strings](../../resources/binary_vs_strings.md)
- [🔢 Truncação de Tipos](../../resources/type_truncation.md)

### 🔗 Referências Externas
- Manual do C: `man memchr`
- [C Reference - Memory Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender diferença entre memchr e strchr
- [ ] Compreender truncação de int para unsigned char
- [ ] Trabalhar com dados binários (incluindo \0)
- [ ] Cast correto de void* para retorno

**Descobertas importantes:**
- [ ] memchr funciona com qualquer tipo de dados
- [ ] Truncação automática de valores > 255
- [ ] Importância do limite n na busca
- [ ] Por que retornar void* ao invés de char*

**Testes que fiz:**
- [ ] Busca em strings normais
- [ ] Dados com bytes zero intercalados
- [ ] Valores maiores que 255
- [ ] Busca limitada por n

---
<div align="center">

[← Função Anterior: ft_memmove](ft_memmove.md) | [Próxima Função: ft_memcmp →](ft_memcmp.md)

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
