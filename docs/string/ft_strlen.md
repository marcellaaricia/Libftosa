# ft_strlen

> 📏 Calcula o comprimento de uma string

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strlen.c`

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
size_t ft_strlen(const char *s);
```

> 💡 **Observação:** Retorna `size_t` (unsigned) pois comprimento nunca é negativo.

---

## 📖 DESCRIÇÃO

A função `ft_strlen` calcula o número de caracteres em uma string, **excluindo** o terminador nulo (`\0`).

### 🎯 Objetivo
Contar todos os caracteres desde o início da string até encontrar o primeiro `\0`.

### 🌍 Aplicações Reais
- **Validação de entrada:** Verificar tamanho mínimo/máximo
- **Alocação de memória:** Determinar espaço necessário
- **Processamento de texto:** Análise de dados
- **Buffers:** Controle de limites de arrays

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s` | `const char *` | Ponteiro para a string (não modificada) |

---

## ↩️ VALOR DE RETORNO

| Tipo | Descrição |
|------|-----------|
| `size_t` | Número de caracteres na string (sem contar `\0`) |

### 📊 Exemplos de Comportamento

| String | Retorno | Explicação |
|--------|---------|------------|
| `"Hello"` | `5` | 5 caracteres + `\0` |
| `"42"` | `2` | 2 caracteres + `\0` |
| `""` | `0` | String vazia (apenas `\0`) |
| `"A\0B"` | `1` | Para no primeiro `\0` |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔤 Estrutura de String em C
```
String: "Hello"
Memória: ['H']['e']['l']['l']['o']['\0']
Índices:   0    1    2    3    4    5
Retorno: 5 (não conta o \0)
```

### 🧠 Lógica Conceitual
```
1. Começar no primeiro caractere
2. Contar cada caractere que não seja \0
3. Parar quando encontrar \0
4. Retornar a contagem
```

### 🔄 Algoritmo de Contagem
- **Método:** Iteração com contador
- **Condição de parada:** Caractere atual == `\0`
- **Incremento:** Avançar ponteiro e aumentar contador

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

int main(void)
{
    char *exemplos[] = {
        "Hello, World!",
        "42",
        "",
        "Uma string mais longa",
        "A",
        NULL
    };
    int i = 0;
    
    printf("=== TESTANDO CONCEITO ===\n");
    printf("String                    | Comprimento\n");
    printf("--------------------------|------------\n");
    
    while (exemplos[i] != NULL)
    {
        size_t tamanho = ft_strlen(exemplos[i]);
        
        printf("\"%-23s\" | %zu\n", exemplos[i], tamanho);
        i++;
    }
    
    // Teste com string vazia
    printf("\"%-23s\" | %zu\n", "", ft_strlen(""));
    
    return (0);
}
```

**Saída esperada:**
```
=== TESTANDO CONCEITO ===
String                    | Comprimento
--------------------------|------------
"Hello, World!"           | 13
"42"                      | 2
"Uma string mais longa"   | 22
"A"                       | 1
""                        | 0
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Ponteiros:** Como navegar por arrays de char
2. **Terminador nulo:** O que é `\0` e por que existe
3. **size_t:** Tipo para tamanhos (unsigned)
4. **Loops:** while, for - qual usar?

### 🎯 Perguntas para Reflexão
- Por que retornar `size_t` ao invés de `int`?
- Como detectar o fim da string?
- O que acontece se a string não tiver `\0`?
- Qual a diferença entre `char *` e `const char *`?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Contador com While
```
contador = 0
enquanto (string[contador] não for \0):
    incrementa contador
retorna contador
```

### 💭 Abordagem 2: Ponteiro Móvel
```
ponteiro_inicial = string
enquanto (*string não for \0):
    avança string
retorna (string - ponteiro_inicial)
```

### 🔧 Dicas de Implementação
- Use `size_t` para o contador (tipo correto)
- Teste o caractere atual, não o próximo
- Não modifique o ponteiro original (se necessário)
- Considere usar `const` para indicar que não modifica

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente `ft_strlen` com contador simples
2. Teste com strings de diferentes tamanhos
3. Compare com a função original `strlen`

### 🥈 Nível Intermediário
1. Implemente usando aritmética de ponteiros
2. Crie uma versão que conta até um limite máximo
3. Implemente um contador de palavras usando `ft_strlen`

### 🥇 Nível Avançado
1. Otimize para strings muito longas
2. Crie uma versão que funciona com strings wide (wchar_t)
3. Implemente validação de strings em formulários

---

## 🔗 FUNÇÕES RELACIONADAS

### 📝 Mesma Categoria
- [`ft_strchr`](ft_strchr.md) - Busca caractere na string
- [`ft_strncmp`](ft_strncmp.md) - Compara strings até n caracteres
- [`ft_strlcpy`](ft_strlcpy.md) - Copia string de forma segura
- [`ft_strlcat`](ft_strlcat.md) - Concatena strings de forma segura

### 🔄 Funções Complementares
- [`ft_calloc`](../memory/ft_calloc.md) - Aloca memória para strings
- [`ft_strdup`](ft_strdup.md) - Duplica string (usa strlen internamente)

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🧠 Ponteiros em C](../../resources/pointers.md)
- [🔤 Strings e Arrays](../../resources/strings_arrays.md)
- [📏 Tipos de Dados](../../resources/data_types.md)

### 🔗 Referências Externas
- Manual do C: `man strlen`
- [C Reference - String Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender a diferença entre tamanho e índice
- [ ] Lembrar que `\0` não conta no comprimento
- [ ] Escolher entre contador ou aritmética de ponteiros
- [ ] Decidir quando usar `const char *`

**Descobertas importantes:**
- [ ] Por que `size_t` é melhor que `int`
- [ ] Como strings funcionam internamente
- [ ] Relação entre ponteiros e arrays
- [ ] Importância do terminador nulo

**Testes que fiz:**
- [ ] String normal
- [ ] String vazia
- [ ] String de 1 caractere
- [ ] String muito longa

---
<div align="center">

[Próxima Função: ft_strchr →](ft_strchr.md)

📝 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
