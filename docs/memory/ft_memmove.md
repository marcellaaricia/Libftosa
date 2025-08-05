# ft_memmove

> 🔄 Move memória com sobreposição segura

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_memmove.c`

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
void *ft_memmove(void *dst, const void *src, size_t len);
```

> 💡 **Observação:** Versão segura do memcpy - funciona mesmo com sobreposição.

---

## 📖 DESCRIÇÃO

A função `ft_memmove` copia `len` bytes da área de memória `src` para `dst`. Diferente do `memcpy`, **trata sobreposição de memória** de forma segura.

### 🎯 Objetivo
Mover dados entre áreas de memória que podem se sobrepor, garantindo integridade dos dados.

### 🌍 Aplicações Reais
- **Reorganização de arrays:** Mover elementos para posições diferentes
- **Buffers deslizantes:** Shifting de dados em streams
- **Edição de texto:** Inserir/remover caracteres em strings
- **Compactação de dados:** Eliminar espaços vazios em arrays

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `dst` | `void *` | Ponteiro para destino |
| `src` | `const void *` | Ponteiro para origem (não modificado) |
| `len` | `size_t` | Número de bytes a mover |

---

## ↩️ VALOR DE RETORNO

| Tipo | Descrição |
|------|-----------|
| `void *` | Ponteiro para o destino (`dst`) |

### 📊 Exemplos de Comportamento

| Situação | Resultado | Método |
|----------|-----------|---------|
| `dst < src` | Cópia normal (início→fim) | Como memcpy |
| `dst > src` | Cópia reversa (fim→início) | Para evitar sobreposição |
| `dst == src` | Nenhuma operação | Otimização |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔄 Problema da Sobreposição
```
Exemplo: mover "Hello" 2 posições à direita em "Hello___"

❌ MEMCPY (comportamento indefinido):
"Hello___" → copia H → "Hello___"
             copia e → "HHllo___"  (perdeu o 'e'!)

✅ MEMMOVE (cópia reversa):
"Hello___" → copia o → "Hello__o"
             copia l → "Hello_lo"
             copia l → "Hellollo"
             copia e → "Hellello"
             copia H → "HHellello" → "  Hello" 
```

### 🧠 Algoritmo de Detecção
```c
if (dst > src) {
    // Sobreposição: copia de trás para frente
    for (i = len-1; i >= 0; i--)
        dst[i] = src[i];
} else {
    // Sem sobreposição: copia normal
    for (i = 0; i < len; i++)
        dst[i] = src[i];
}
```

### 📍 Casos de Sobreposição
```
Caso 1 - dst < src (sem problema):
src:  [A][B][C][D][E]
dst:     [_][_][_][_][_]
      ↗ Cópia normal funcionará

Caso 2 - dst > src (problema!):
src:  [A][B][C][D][E]
dst:        [_][_][_][_][_]
         ↗ Precisa cópia reversa
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void print_buffer(char *buf, char *label) {
    printf("%s: \"%.15s\"\n", label, buf);
}

int main(void)
{
    printf("=== DEMONSTRAÇÃO SOBREPOSIÇÃO ===\n\n");
    
    // Exemplo 1: Sem sobreposição (funciona como memcpy)
    char buffer1[20] = "Hello World     ";
    ft_memmove(buffer1 + 12, "42!", 3);
    print_buffer(buffer1, "Sem sobreposição");
    
    // Exemplo 2: Sobreposição direita (dst > src)
    char buffer2[20] = "Hello World     ";
    printf("Antes:         \"%.15s\"\n", buffer2);
    ft_memmove(buffer2 + 3, buffer2, 8);  // Move "Hello Wo" para pos 3
    print_buffer(buffer2, "Após move →   ");
    
    // Exemplo 3: Sobreposição esquerda (dst < src)  
    char buffer3[20] = "Hello World     ";
    printf("Antes:         \"%.15s\"\n", buffer3);
    ft_memmove(buffer3, buffer3 + 6, 5);  // Move "World" para início
    print_buffer(buffer3, "Após move ←   ");
    
    // Exemplo 4: Array de números
    int nums[10] = {1, 2, 3, 4, 5, 0, 0, 0, 0, 0};
    printf("\nArray antes: ");
    for (int i = 0; i < 8; i++) printf("%d ", nums[i]);
    
    ft_memmove(nums + 2, nums, 5 * sizeof(int));  // Shift direita
    printf("\nArray após:  ");
    for (int i = 0; i < 8; i++) printf("%d ", nums[i]);
    printf("\n");
    
    return (0);
}
```

**Saída esperada:**
```
=== DEMONSTRAÇÃO SOBREPOSIÇÃO ===

Sem sobreposição: "Hello World 42!"
Antes:         "Hello World    "
Após move →   : "HelHello Wor   "
Antes:         "Hello World    "
Após move ←   : "World World    "

Array antes: 1 2 3 4 5 0 0 0 
Array após:  1 2 1 2 3 4 5 0
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Sobreposição de memória:** Como detectar e por que é problema
2. **Direção de cópia:** Quando copiar forward vs backward
3. **Comparação de ponteiros:** Como `dst > src` funciona
4. **Otimizações:** Quando não fazer nada (dst == src)

### 🎯 Perguntas para Reflexão
- Como detectar se duas áreas de memória se sobrepõem?
- Por que dst > src requer cópia reversa?
- Qual a diferença de performance entre memmove e memcpy?
- Em que casos memmove é indispensável?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Detecção Simples
```c
void *ft_memmove(void *dst, const void *src, size_t len)
{
    if (dst > src) {
        // Cópia reversa
        while (len--)
            ((char*)dst)[len] = ((char*)src)[len];
    } else {
        // Cópia normal
        for (size_t i = 0; i < len; i++)
            ((char*)dst)[i] = ((char*)src)[i];
    }
    return dst;
}
```

### 💭 Abordagem 2: Usando Buffer Temporário
```c
void *ft_memmove(void *dst, const void *src, size_t len)
{
    char temp[len];  // Requer C99
    ft_memcpy(temp, src, len);
    ft_memcpy(dst, temp, len);
    return dst;
}
```

### 💭 Abordagem 3: Detecção Completa
```c
void *ft_memmove(void *dst, const void *src, size_t len)
{
    if (dst == src || len == 0)
        return dst;  // Otimização
    
    if ((char*)dst > (char*)src && (char*)dst < (char*)src + len) {
        // Sobreposição detectada: cópia reversa
    } else {
        // Sem sobreposição: cópia normal
    }
}
```

### 🔧 Dicas de Implementação
- **Comparação de ponteiros:** Cast para mesmo tipo antes de comparar
- **Cópia reversa:** Comece do final e vá decrementando
- **Otimização:** Verifique casos especiais (dst == src, len == 0)
- **Teste:** Sempre teste casos de sobreposição

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente memmove com detecção simples (dst > src)
2. Teste todos os casos de sobreposição
3. Compare comportamento com memmove original

### 🥈 Nível Intermediário
1. Implemente detecção completa de sobreposição
2. Otimize para casos especiais (dst == src)
3. Crie visualização do processo de cópia

### 🥇 Nível Avançado
1. Implemente usando buffer temporário na stack
2. Otimize para word-aligned operations
3. Crie benchmark comparando com memcpy

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria
- [`ft_memcpy`](ft_memcpy.md) - Versão rápida sem proteção de sobreposição
- [`ft_memset`](ft_memset.md) - Preenche memória com valor
- [`ft_bzero`](ft_bzero.md) - Zera memória
- [`ft_memchr`](ft_memchr.md) - Busca byte em memória
- [`ft_memcmp`](ft_memcmp.md) - Compara blocos de memória

### 🔄 Funções Complementares
- [`ft_strlen`](../string/ft_strlen.md) - Para determinar tamanhos
- [`ft_strdup`](../string/ft_strdup.md) - Duplicação segura de strings

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔄 Sobreposição de Memória](../../resources/memory_overlap.md)
- [⚡ Otimização de Cópia](../../resources/copy_optimization.md)
- [🧠 Algoritmos de Movimentação](../../resources/move_algorithms.md)

### 🔗 Referências Externas
- Manual do C: `man memmove`
- [C Reference - Memory Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender quando há sobreposição de memória
- [ ] Implementar cópia reversa corretamente
- [ ] Decidir entre detecção simples vs completa
- [ ] Otimizar para casos especiais

**Descobertas importantes:**
- [ ] Por que memmove é mais lenta que memcpy
- [ ] Como detectar direção da sobreposição
- [ ] Importância da cópia reversa
- [ ] Casos onde otimização é possível

**Testes que fiz:**
- [ ] Sobreposição para direita (dst > src)
- [ ] Sobreposição para esquerda (dst < src)
- [ ] Mesmo ponteiro (dst == src)
- [ ] Arrays de diferentes tipos

---
<div align="center">

[← Função Anterior: ft_memcpy](ft_memcpy.md) | [Próxima Função: ft_memchr →](ft_memchr.md)

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
