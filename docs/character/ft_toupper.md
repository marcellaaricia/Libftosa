# ft_toupper

> 📢 Converte letra minúscula para maiúscula

---

**Categoria:** [Funções de Caractere](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_toupper.c`

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
int ft_toupper(int c);
```

> 💡 **Observação:** Apenas letras minúsculas são convertidas; outros caracteres permanecem inalterados.

---

## 📖 DESCRIÇÃO

A função `ft_toupper` converte uma letra minúscula para sua correspondente maiúscula. Se o caractere não for uma letra minúscula, retorna o caractere original.

### 🎯 Objetivo
Transformar letras minúsculas em maiúsculas para padronização, comparações ou formatação de texto.

### 🌍 Aplicações Reais
- **Normalização de dados:** Padronizar entrada do usuário
- **Comparações case-insensitive:** Converter antes de comparar
- **Formatação de nomes:** Capitalizar nomes próprios
- **Processamento de comandos:** Tornar comandos insensíveis ao caso

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Caractere a ser convertido (valor ASCII) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| É minúscula (a-z) | Maiúscula correspondente | 'a' → 'A', 'b' → 'B', etc. |
| Não é minúscula | Caractere original | Números, símbolos, maiúsculas inalterados |

### 📊 Exemplos de Comportamento

| Entrada | ASCII | Retorno | ASCII | Explicação |
|---------|-------|---------|-------|------------|
| `'a'` | 97 | `'A'` | 65 | Conversão minúscula |
| `'z'` | 122 | `'Z'` | 90 | Última letra |
| `'A'` | 65 | `'A'` | 65 | Já é maiúscula |
| `'5'` | 53 | `'5'` | 53 | Não é letra |
| `'@'` | 64 | `'@'` | 64 | Símbolo especial |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔢 Matemática ASCII
```
Diferença constante entre maiúscula e minúscula = 32

'A' = 65,  'a' = 97   →  97 - 65 = 32
'B' = 66,  'b' = 98   →  98 - 66 = 32
'Z' = 90,  'z' = 122  →  122 - 90 = 32

Para converter: minúscula - 32 = maiúscula
```

### 📋 Mapeamento Completo
```
a(97) → A(65)    n(110) → N(78)
b(98) → B(66)    o(111) → O(79)
c(99) → C(67)    p(112) → P(80)
...              ...
m(109) → M(77)   z(122) → Z(90)
```

### 🎯 Algoritmo Visual
```
Entrada: 'h' (ASCII 104)

1. É minúscula? (104 >= 97 && 104 <= 122) → SIM
2. Converter: 104 - 32 = 72
3. Resultado: ASCII 72 = 'H'

Entrada: 'H' (ASCII 72)

1. É minúscula? (72 >= 97 && 72 <= 122) → NÃO
2. Manter original: 72
3. Resultado: ASCII 72 = 'H'
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_conversao(char c)
{
    int resultado = ft_toupper(c);
    
    if (c >= 'a' && c <= 'z')
        printf("'%c' (minúscula) → '%c' (maiúscula)\n", c, resultado);
    else if (c >= 'A' && c <= 'Z')
        printf("'%c' (já maiúscula) → '%c' (sem mudança)\n", c, resultado);
    else
        printf("'%c' (não é letra) → '%c' (sem mudança)\n", c, resultado);
}

void converter_string(char *str)
{
    printf("Original: \"%s\"\n", str);
    printf("Maiúscula: \"");
    
    for (int i = 0; str[i]; i++)
        printf("%c", ft_toupper(str[i]));
        
    printf("\"\n\n");
}

int main(void)
{
    printf("=== DEMONSTRAÇÃO TOUPPER ===\n\n");
    
    // Exemplo 1: Conversões individuais
    printf("1. Conversões individuais:\n");
    demonstrar_conversao('a');
    demonstrar_conversao('M');
    demonstrar_conversao('5');
    demonstrar_conversao('@');
    
    // Exemplo 2: Strings completas
    printf("\n2. Conversão de strings:\n");
    converter_string("hello world");
    converter_string("C Programming");
    converter_string("42 School!");
    converter_string("MiXeD cAsE");
    
    // Exemplo 3: Uso prático - comparação insensível
    printf("3. Comparação case-insensitive:\n");
    char input1[] = "hello";
    char input2[] = "HELLO";
    
    printf("Comparando \"%s\" e \"%s\":\n", input1, input2);
    
    // Converte ambas para maiúscula e compara
    int iguais = 1;
    for (int i = 0; input1[i] && input2[i]; i++)
    {
        if (ft_toupper(input1[i]) != ft_toupper(input2[i]))
        {
            iguais = 0;
            break;
        }
    }
    
    printf("São iguais (ignorando caso): %s\n", iguais ? "SIM" : "NÃO");
    
    // Exemplo 4: Demonstração da matemática
    printf("\n4. Demonstração da matemática ASCII:\n");
    printf("'a' = %d, 'A' = %d, diferença = %d\n", 'a', 'A', 'a' - 'A');
    printf("'z' = %d, 'Z' = %d, diferença = %d\n", 'z', 'Z', 'z' - 'Z');
    printf("Para converter: minúscula - 32 = maiúscula\n");
    
    return (0);
}
```

**Saída esperada:**
```
=== DEMONSTRAÇÃO TOUPPER ===

1. Conversões individuais:
'a' (minúscula) → 'A' (maiúscula)
'M' (já maiúscula) → 'M' (sem mudança)
'5' (não é letra) → '5' (sem mudança)
'@' (não é letra) → '@' (sem mudança)

2. Conversão de strings:
Original: "hello world"
Maiúscula: "HELLO WORLD"

Original: "C Programming"
Maiúscula: "C PROGRAMMING"

Original: "42 School!"
Maiúscula: "42 SCHOOL!"

Original: "MiXeD cAsE"
Maiúscula: "MIXED CASE"

3. Comparação case-insensitive:
Comparando "hello" e "HELLO":
São iguais (ignorando caso): SIM

4. Demonstração da matemática ASCII:
'a' = 97, 'A' = 65, diferença = 32
'z' = 122, 'Z' = 90, diferença = 32
Para converter: minúscula - 32 = maiúscula
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Tabela ASCII:** Posições das letras maiúsculas e minúsculas
2. **Aritmética de caracteres:** Como somar/subtrair valores ASCII
3. **Diferença constante:** Por que sempre 32 entre maiúscula/minúscula
4. **Verificação de intervalo:** Como identificar letras minúsculas

### 🎯 Perguntas para Reflexão
- Por que a diferença entre maiúscula e minúscula é sempre 32?
- O que acontece se aplicar toupper em caracteres não-ASCII?
- Como implementar conversão para outros alfabetos?
- Por que retornar int ao invés de char?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Aritmética Direta
```c
int ft_toupper(int c)
{
    if (c >= 'a' && c <= 'z')
        return (c - 32);
    return (c);
}
```

### 💭 Abordagem 2: Usando Constantes ASCII
```c
int ft_toupper(int c)
{
    if (c >= 97 && c <= 122)  // ASCII de 'a' a 'z'
        return (c - 32);
    return (c);
}
```

### 💭 Abordagem 3: Bitwise Operation
```c
int ft_toupper(int c)
{
    if (c >= 'a' && c <= 'z')
        return (c & ~32);  // Limpa o bit 5 (equivale a -32)
    return (c);
}
```

### 🔧 Dicas de Implementação
- **Verificação primeiro:** Sempre checar se é minúscula antes de converter
- **Aritmética simples:** Subtração de 32 é mais clara que operações bit
- **Manter original:** Retornar o caractere inalterado se não for minúscula
- **Usar constantes:** 'a' e 'z' são mais legíveis que 97 e 122

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_toupper com diferentes abordagens
2. Teste com todas as letras do alfabeto
3. Crie função que converte string inteira

### 🥈 Nível Intermediário
1. Implemente usando operações bitwise
2. Crie função de comparação case-insensitive
3. Desenvolva capitalizador de nomes próprios

### 🥇 Nível Avançado
1. Implemente suporte para caracteres acentuados
2. Crie sistema de conversão para múltiplos idiomas
3. Otimize usando lookup tables

---

## 🔗 FUNÇÕES RELACIONADAS

### 🔤 Mesma Categoria
- [`ft_tolower`](ft_tolower.md) - Converte para minúscula
- [`ft_isalpha`](ft_isalpha.md) - Verifica se é letra
- [`ft_isprint`](ft_isprint.md) - Verifica se é imprimível

### 🔄 Funções String Relacionadas
- [`ft_strmapi`](../additional/ft_strmapi.md) - Aplica função em cada char
- [`ft_striteri`](../additional/ft_striteri.md) - Itera sobre string

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [📊 Tabela ASCII Completa](../../resources/ascii_table.md)
- [🔢 Aritmética de Caracteres](../../resources/char_arithmetic.md)
- [📢 Case Conversion](../../resources/case_conversion.md)

### 🔗 Referências Externas
- Manual do C: `man toupper`
- [ASCII Table Reference](https://www.asciitable.com/)
- [C Reference - Character Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender a matemática ASCII (diferença de 32)
- [ ] Lembrar de verificar se é minúscula primeiro
- [ ] Decidir entre aritmética e operações bit
- [ ] Casos especiais com caracteres não-ASCII

**Descobertas importantes:**
- [ ] Diferença constante de 32 entre maiúscula/minúscula
- [ ] Importância da verificação antes da conversão
- [ ] Aplicações em comparações case-insensitive
- [ ] Elegância da aritmética ASCII

**Testes que fiz:**
- [ ] Todas as letras minúsculas (a-z)
- [ ] Letras já maiúsculas (A-Z)
- [ ] Números e símbolos
- [ ] Strings completas mistas

---
<div align="center">

[← Função Anterior: ft_isprint](ft_isprint.md) | [Próxima Função: ft_tolower →](ft_tolower.md)

🔤 [Funções de Caractere](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
