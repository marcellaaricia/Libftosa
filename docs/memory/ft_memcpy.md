# ft_memcpy

> 📋 Copia bloco de memória de origem para destino

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_memcpy.c`

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
void *ft_memcpy(void *dst, const void *src, size_t n);
```

> 💡 **Observação:** Não trata sobreposição de memória - use ft_memmove para isso.

---

## 📖 DESCRIÇÃO

A função `ft_memcpy` copia `n` bytes da área de memória `src` para a área de memória `dst`. As áreas **não devem se sobrepor**.

### 🎯 Objetivo
Transferir dados binários de um local para outro de forma rápida e eficiente.

### 🌍 Aplicações Reais
- **Cópia de strings:** Duplicar conteúdo de texto
- **Arrays:** Copiar arrays de qualquer tipo
- **Estruturas:** Duplicar structs complexas
- **Buffers:** Transferir dados entre buffers

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `dst` | `void *` | Ponteiro para destino da cópia |
| `src` | `const void *` | Ponteiro para origem (não modificado) |
| `n` | `size_t` | Número de bytes a copiar |

---

## ↩️ VALOR DE RETORNO

| Tipo | Descrição |
|------|-----------|
| `void *` | Ponteiro para o destino (`dst`) |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Explicação |
|---------|-----------|------------|
| `memcpy(dest, "Hello", 5)` | dest = "Hello" | Copia 5 bytes |
| `memcpy(arr2, arr1, 20)` | arr2 = cópia de arr1 | Copia array |
| `memcpy(ptr, data, 0)` | Nenhuma mudança | n=0 não copia nada |

---

## 💡 COMO ENTENDER O CONCEITO

### 📋 Cópia Byte a Byte
```
Origem:  ['H']['e']['l']['l']['o']
            ↓    ↓    ↓    ↓    ↓
Destino: ['H']['e']['l']['l']['o']
```

### 🔄 Fluxo de Dados
```c
void *src  → const unsigned char *s  (leitura)
void *dst  → unsigned char *d        (escrita)

Para cada byte i de 0 até n-1:
    d[i] = s[i]  // Copia um byte
```

### ⚠️ Problema de Sobreposição
```c
char buffer[] = "Hello";
memcpy(buffer + 2, buffer, 3);  // ❌ INDEFINIDO!

// Pode resultar em: "HeHel" ou "HeHHH" ou qualquer coisa
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

int main(void)
{
    // Exemplo 1: Cópia de string
    char origem[] = "Hello, World!";
    char destino[20];
    
    ft_memcpy(destino, origem, strlen(origem) + 1);
    printf("String copiada: \"%s\"\n", destino);
    
    // Exemplo 2: Cópia de array de inteiros
    int numeros[] = {1, 2, 3, 4, 5};
    int copia[5];
    
    ft_memcpy(copia, numeros, sizeof(numeros));
    printf("Array copiado: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", copia[i]);
    
    // Exemplo 3: Cópia de struct
    typedef struct {
        int x, y;
        char nome[10];
    } Ponto;
    
    Ponto p1 = {10, 20, "Origem"};
    Ponto p2;
    
    ft_memcpy(&p2, &p1, sizeof(Ponto));
    printf("\nStruct copiada: x=%d y=%d nome=\"%s\"\n", 
           p2.x, p2.y, p2.nome);
    
    // Exemplo 4: Cópia parcial
    char buffer[10] = "xxxxxxxxx";
    ft_memcpy(buffer, "ABC", 3);
    printf("Cópia parcial: \"%s\"\n", buffer);  // "ABCxxxxxx"
    
    return (0);
}
```

**Saída esperada:**
```
String copiada: "Hello, World!"
Array copiado: 1 2 3 4 5 
Struct copiada: x=10 y=20 nome="Origem"
Cópia parcial: "ABCxxxxxx"
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **void pointers:** Por que usar e como fazer cast
2. **const correctness:** Diferença entre `void *` e `const void *`
3. **unsigned char:** Por que é ideal para manipulação de bytes
4. **Sobreposição:** O que acontece quando áreas se sobrepõem

### 🎯 Perguntas para Reflexão
- Por que não verificar sobreposição em memcpy?
- Qual a diferença entre memcpy e memmove?
- Por que usar unsigned char ao invés de char?
- Como detectar se duas áreas de memória se sobrepõem?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Loop com Indexing
```c
void *ft_memcpy(void *dst, const void *src, size_t n)
{
    unsigned char *d = (unsigned char *)dst;
    const unsigned char *s = (const unsigned char *)src;
    size_t i = 0;
    
    while (i < n)
    {
        d[i] = s[i];
        i++;
    }
    return (dst);
}
```

### 💭 Abordagem 2: Ponteiros Móveis
```c
void *ft_memcpy(void *dst, const void *src, size_t n)
{
    unsigned char *d = (unsigned char *)dst;
    const unsigned char *s = (const unsigned char *)src;
    
    while (n--)
        *d++ = *s++;
    return (dst);
}
```

### 🔧 Dicas de Implementação
- **Proteção NULL:** Verificar se ponteiros são válidos
- **Cast correto:** void* → unsigned char*
- **const preservation:** Manter const no src após cast
- **Retorno:** Sempre retornar o ponteiro de destino original

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_memcpy com indexing
2. Teste com strings, arrays e structs
3. Compare comportamento com memcpy original

### 🥈 Nível Intermediário
1. Implemente versão com aritmética de ponteiros
2. Adicione verificações de segurança extras
3. Crie função para detectar sobreposição

### 🥇 Nível Avançado
1. Otimize para copiar words (4/8 bytes) quando alinhado
2. Implemente versão que trata sobreposição automaticamente
3. Crie benchmark comparando diferentes implementações

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria
- [`ft_memmove`](ft_memmove.md) - Copia com sobreposição segura
- [`ft_memset`](ft_memset.md) - Preenche memória com valor
- [`ft_bzero`](ft_bzero.md) - Zera memória
- [`ft_memchr`](ft_memchr.md) - Busca byte em memória
- [`ft_memcmp`](ft_memcmp.md) - Compara blocos de memória

### 🔄 Funções Complementares
- [`ft_strlen`](../string/ft_strlen.md) - Para determinar tamanho de strings
- [`ft_strdup`](../string/ft_strdup.md) - Usa memcpy internamente
- [`ft_calloc`](ft_calloc.md) - Aloca e copia dados zerados

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🧠 Ponteiros void em C](../../resources/void_pointers.md)
- [📋 Cópia de Memória](../../resources/memory_copy.md)
- [⚠️ Sobreposição de Memória](../../resources/memory_overlap.md)

### 🔗 Referências Externas
- Manual do C: `man memcpy`
- [C Reference - Memory Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender por que não tratar sobreposição
- [ ] Cast correto de void* para unsigned char*
- [ ] Diferença entre memcpy e memmove
- [ ] Preservar const ao fazer cast

**Descobertas importantes:**
- [ ] memcpy é mais rápida que memmove
- [ ] unsigned char garante 1 byte exato
- [ ] Sobreposição causa comportamento indefinido
- [ ] Importância do valor de retorno

**Testes que fiz:**
- [ ] Cópia de strings completas
- [ ] Cópia parcial de dados
- [ ] Arrays de diferentes tipos
- [ ] Estruturas complexas

---
<div align="center">

[← Função Anterior: ft_bzero](ft_bzero.md) | [Próxima Função: ft_memmove →](ft_memmove.md)

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
