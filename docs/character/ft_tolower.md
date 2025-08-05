# ft_tolower

> 🔤 Converte caractere maiúsculo para minúsculo

---

**Categoria:** [Funções de Caractere](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_tolower.c`

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
int ft_tolower(int c);
```

> 💡 **Observação:** Trabalha com int para compatibilidade com EOF (-1).

---

## 📖 DESCRIÇÃO

A função `ft_tolower` converte um caractere maiúsculo para seu equivalente minúsculo. Se o caractere não for uma letra maiúscula, retorna o caractere inalterado.

### 🎯 Objetivo
Fornecer conversão padronizada de caso para caracteres ASCII, essencial para processamento de texto insensível a maiúsculas/minúsculas.

### 🌍 Aplicações Reais
- **Normalização de texto:** Converter strings para comparação
- **Validação de entrada:** Aceitar input em qualquer caso
- **Processamento de dados:** Padronizar formato de texto
- **Busca insensível:** Encontrar texto independente do caso

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Caractere a ser convertido (ou EOF) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Maiúscula (A-Z) | `int` | Caractere em minúscula correspondente |
| Outros | `int` | Caractere original inalterado |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Explicação |
|---------|-----------|------------|
| `'A'` | `'a'` | Maiúscula → minúscula |
| `'Z'` | `'z'` | Última maiúscula → minúscula |
| `'a'` | `'a'` | Já minúscula, inalterada |
| `'5'` | `'5'` | Número, inalterado |
| `'@'` | `'@'` | Símbolo, inalterado |
| `-1 (EOF)` | `-1` | EOF preservado |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Fórmula Básica
```c
// ASCII: A=65, B=66... Z=90
// ASCII: a=97, b=98... z=122
// Diferença = 97-65 = 32

if (c >= 'A' && c <= 'Z')
    return (c + 32);  // Adiciona 32 para converter
else
    return (c);       // Mantém inalterado
```

### 📊 Tabela ASCII Relevante
```
Maiúsculas:  A(65)  B(66)  C(67)  ...  Z(90)
Minúsculas:  a(97)  b(98)  c(99)  ...  z(122)
Diferença:    +32    +32    +32         +32
```

### 🔍 Detecção de Maiúscula
```c
// Método 1: Comparação direta
if (c >= 'A' && c <= 'Z')

// Método 2: Usando valores ASCII
if (c >= 65 && c <= 90)

// Método 3: Bitwise (avançado)
if ((c | 32) != c && (c >= 'A' && c <= 'Z'))
```

### ⚡ Truque Bitwise
```
Maiúscula: A = 01000001 (65)
Minúscula: a = 01100001 (97)
                  ^
Diferença apenas no bit 5 (32)

A | 32 = 01000001 | 00100000 = 01100001 = a
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>
#include <ctype.h>

void demonstrar_conversao_basica(void)
{
    printf("=== CONVERSÃO BÁSICA ===\n\n");
    
    char chars[] = {'A', 'B', 'Z', 'a', 'x', '5', '@', '\0'};
    
    printf("Original → ft_tolower → tolower (padrão)\n");
    printf("---------------------------------------\n");
    
    for (int i = 0; chars[i]; i++)
    {
        char orig = chars[i];
        char my_result = ft_tolower(orig);
        char std_result = tolower(orig);
        
        printf("   '%c'   →     '%c'    →      '%c'", 
               orig, my_result, std_result);
               
        if (my_result == std_result)
            printf(" ✅\n");
        else
            printf(" ❌\n");
    }
    printf("\n");
}

void exemplo_normalizacao_string(void)
{
    printf("=== NORMALIZAÇÃO DE STRING ===\n\n");
    
    char input[] = "Hello WORLD! 123";
    char normalized[100];
    
    printf("Original: \"%s\"\n", input);
    
    // Normaliza toda a string
    int i = 0;
    while (input[i])
    {
        normalized[i] = ft_tolower(input[i]);
        i++;
    }
    normalized[i] = '\0';
    
    printf("Normalizada: \"%s\"\n\n", normalized);
}

void exemplo_comparacao_insensivel(void)
{
    printf("=== COMPARAÇÃO INSENSÍVEL A CASO ===\n\n");
    
    char str1[] = "Hello";
    char str2[] = "HELLO";
    char str3[] = "world";
    
    printf("Comparando \"%s\" com \"%s\":\n", str1, str2);
    
    // Compara caractere por caractere
    int i = 0;
    int equal = 1;
    
    while (str1[i] && str2[i])
    {
        if (ft_tolower(str1[i]) != ft_tolower(str2[i]))
        {
            equal = 0;
            break;
        }
        i++;
    }
    
    if (equal && str1[i] == str2[i])
        printf("✅ Strings são iguais (ignorando caso)\n");
    else
        printf("❌ Strings são diferentes\n");
        
    printf("\nComparando \"%s\" com \"%s\":\n", str1, str3);
    
    // Reseta para próxima comparação
    i = 0;
    equal = 1;
    
    while (str1[i] && str3[i])
    {
        if (ft_tolower(str1[i]) != ft_tolower(str3[i]))
        {
            equal = 0;
            break;
        }
        i++;
    }
    
    if (equal && str1[i] == str3[i])
        printf("✅ Strings são iguais (ignorando caso)\n");
    else
        printf("❌ Strings são diferentes\n");
        
    printf("\n");
}

void exemplo_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    int test_cases[] = {'A', 'a', '5', '@', ' ', '\n', -1, 0};
    
    printf("Entrada → Saída → Tipo\n");
    printf("---------------------\n");
    
    for (int i = 0; i < 8; i++)
    {
        int input = test_cases[i];
        int output = ft_tolower(input);
        
        if (input == -1)
            printf("  EOF   →  EOF  → EOF especial\n");
        else if (input == 0)
            printf("   0    →   0   → Null terminator\n");
        else if (input >= 'A' && input <= 'Z')
            printf("  '%c'   →  '%c'  → Maiúscula convertida\n", 
                   input, output);
        else if (input >= 'a' && input <= 'z')
            printf("  '%c'   →  '%c'  → Minúscula inalterada\n", 
                   input, output);
        else if (input == ' ')
            printf("  ' '   →  ' '  → Espaço inalterado\n");
        else if (input == '\n')
            printf(" '\\n'   → '\\n'  → Quebra linha inalterada\n");
        else
            printf("  '%c'   →  '%c'  → Outro caractere\n", 
                   input, output);
    }
    printf("\n");
}

int main(void)
{
    printf("🔤 DEMONSTRAÇÃO FT_TOLOWER\n");
    printf("==========================\n\n");
    
    demonstrar_conversao_basica();
    exemplo_normalizacao_string();
    exemplo_comparacao_insensivel();
    exemplo_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • Apenas A-Z são convertidas\n");
    printf("   • Diferença ASCII é sempre 32\n");
    printf("   • Outros caracteres ficam iguais\n");
    printf("   • Usado para comparações insensíveis\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔤 DEMONSTRAÇÃO FT_TOLOWER
==========================

=== CONVERSÃO BÁSICA ===

Original → ft_tolower → tolower (padrão)
---------------------------------------
   'A'   →     'a'    →      'a' ✅
   'B'   →     'b'    →      'b' ✅
   'Z'   →     'z'    →      'z' ✅
   'a'   →     'a'    →      'a' ✅
   'x'   →     'x'    →      'x' ✅
   '5'   →     '5'    →      '5' ✅
   '@'   →     '@'    →      '@' ✅

=== NORMALIZAÇÃO DE STRING ===

Original: "Hello WORLD! 123"
Normalizada: "hello world! 123"

=== COMPARAÇÃO INSENSÍVEL A CASO ===

Comparando "Hello" com "HELLO":
✅ Strings são iguais (ignorando caso)

Comparando "Hello" com "world":
❌ Strings são diferentes

=== CASOS ESPECIAIS ===

Entrada → Saída → Tipo
---------------------
  'A'   →  'a'  → Maiúscula convertida
  'a'   →  'a'  → Minúscula inalterada
  '5'   →  '5'  → Outro caractere
  '@'   →  '@'  → Outro caractere
  ' '   →  ' '  → Espaço inalterado
 '\n'   → '\n'  → Quebra linha inalterada
  EOF   →  EOF  → EOF especial
   0    →   0   → Null terminator

💡 LEMBRE-SE:
   • Apenas A-Z são convertidas
   • Diferença ASCII é sempre 32
   • Outros caracteres ficam iguais
   • Usado para comparações insensíveis
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Tabela ASCII:** Valores numéricos dos caracteres
2. **Conversão de caso:** Por que existe diferença de 32
3. **int vs char:** Por que usar int (compatibilidade EOF)
4. **Operações bitwise:** Método alternativo de conversão

### 🎯 Perguntas para Reflexão
- Por que apenas A-Z são convertidas?
- Como funciona a aritmética ASCII?
- Por que retornar int ao invés de char?
- Qual a diferença entre locale-aware e ASCII puro?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Verificação Simples
```c
int ft_tolower(int c)
{
    if (c >= 'A' && c <= 'Z')
        return (c + 32);
    return (c);
}
```

### 💭 Abordagem 2: Usando Constantes
```c
int ft_tolower(int c)
{
    if (c >= 65 && c <= 90)  // ASCII de A-Z
        return (c + 32);     // Diferença para minúscula
    return (c);
}
```

### 💭 Abordagem 3: Operação Bitwise (Avançada)
```c
int ft_tolower(int c)
{
    if (c >= 'A' && c <= 'Z')
        return (c | 32);     // Liga o bit 5
    return (c);
}
```

### 🔧 Dicas de Implementação
- **Verificar limites:** Apenas A-Z devem ser convertidas
- **Preservar outros:** Números, símbolos, já minúsculas ficam iguais
- **Compatibilidade EOF:** Função deve aceitar -1 sem problemas
- **Eficiência:** Operação simples, evitar complexidade desnecessária

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_tolower usando comparação simples
2. Teste com todas as letras maiúsculas
3. Verifique comportamento com não-letras

### 🥈 Nível Intermediário
1. Crie versão usando operações bitwise
2. Implemente função que converte string inteira
3. Compare performance de diferentes implementações

### 🥇 Nível Avançado
1. Implemente tolower com suporte a locale
2. Crie versão SIMD para strings grandes
3. Desenvolva parser que normaliza identificadores

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - Caracteres
- [`ft_toupper`](ft_toupper.md) - Converte para maiúscula
- [`ft_isalpha`](ft_isalpha.md) - Verifica se é letra
- [`ft_isalnum`](ft_isalnum.md) - Verifica se é alfanumérico

### 🔄 Funções Complementares
- `islower` - Verifica se é minúscula
- `isupper` - Verifica se é maiúscula

### 📝 Funções que Usam Conversão
- [`ft_strchr`](../string/ft_strchr.md) - Busca insensível a caso
- [`ft_strncmp`](../string/ft_strncmp.md) - Comparação insensível

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔤 Manipulação de Caracteres](../../resources/character_manipulation.md)
- [📊 Tabela ASCII](../../resources/ascii_table.md)
- [⚡ Operações Bitwise](../../resources/bitwise_operations.md)

### 🔗 Referências Externas
- Manual do C: `man tolower`
- [C Reference - Character Classification](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender por que usar int ao invés de char
- [ ] Memorizar valores ASCII de A-Z e a-z
- [ ] Compreender operações bitwise para conversão
- [ ] Diferença entre locale-aware e ASCII puro

**Descobertas importantes:**
- [ ] Diferença ASCII entre maiúscula e minúscula é sempre 32
- [ ] Operação bitwise (| 32) é equivalente a (+ 32) para maiúsculas
- [ ] EOF (-1) deve ser preservado sem conversão
- [ ] Função é base para processamento de texto insensível

**Testes que fiz:**
- [ ] Todas as letras maiúsculas (A-Z)
- [ ] Caracteres já minúsculos
- [ ] Números e símbolos especiais
- [ ] Casos especiais (EOF, null terminator)

---
<div align="center">

[← Função Anterior: ft_toupper](ft_toupper.md) | [Próxima Categoria: Funções de String →](../string/README.md)

🔤 [Funções de Caractere](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
