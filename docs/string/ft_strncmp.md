# ft_strncmp

> 🔍 Compara duas strings até um limite específico de caracteres

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strncmp.c`

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
int ft_strncmp(const char *s1, const char *s2, size_t n);
```

> 💡 **Observação:** Compara apenas os primeiros `n` caracteres, evitando comparações desnecessárias.

---

## 📖 DESCRIÇÃO

A função `ft_strncmp` compara lexicograficamente as strings `s1` e `s2` até no máximo `n` caracteres, retornando um valor que indica a relação entre elas.

### 🎯 Objetivo
Permitir comparação parcial de strings, útil para prefixos, validações limitadas e otimizações.

### 🌍 Aplicações Reais
- **Verificação de prefixos:** Checar se string começa com determinado texto
- **Comandos de shell:** Comparar comandos parciais (`ls`, `list`, `l`)
- **Validação de protocolos:** HTTP, FTP, etc. (`http://`, `ftp://`)
- **Comparação segura:** Evitar buffer overflow em comparações

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s1` | `const char *` | Primeira string para comparação |
| `s2` | `const char *` | Segunda string para comparação |
| `n` | `size_t` | Número máximo de caracteres a comparar |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| s1 < s2 | `< 0` | s1 é lexicograficamente menor |
| s1 == s2 | `0` | Strings são iguais nos primeiros n caracteres |
| s1 > s2 | `> 0` | s1 é lexicograficamente maior |

### 📊 Exemplos de Comportamento

| s1 | s2 | n | Resultado | Explicação |
|----|----|----|-----------|------------|
| `"abc"` | `"abc"` | 3 | 0 | Strings idênticas |
| `"abc"` | `"abd"` | 3 | < 0 | 'c' < 'd' na posição 2 |
| `"abc"` | `"ab"` | 3 | > 0 | s1 tem mais caracteres |
| `"abc"` | `"abd"` | 2 | 0 | Iguais nos 2 primeiros caracteres |
| `"abc"` | `"xyz"` | 0 | 0 | n=0, não compara nada |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔤 Comparação Lexicográfica
```
Lexicográfica = ordem do dicionário
"apple" < "banana" < "cherry"

Baseada nos valores ASCII:
'A' = 65, 'a' = 97
'A' < 'a' (maiúscula vem antes)
```

### 🆚 Diferença do strcmp
```c
// strcmp: compara TODA a string
strcmp("hello world", "hello universe");  // Compara 13+ caracteres

// strncmp: compara apenas N caracteres
strncmp("hello world", "hello universe", 5);  // Só "hello"
// Resultado: 0 (iguais nos 5 primeiros)
```

### 🔍 Processo de Comparação
```
s1 = "programming"
s2 = "program"
n = 7

Posição:  0 1 2 3 4 5 6
s1:      'p r o g r a m' (continua...)
s2:      'p r o g r a m' (termina)
         ↑ ↑ ↑ ↑ ↑ ↑ ↑
Compara:  = = = = = = =

Resultado: s1[7] = 'm', s2[7] = '\0'
'm' > '\0', então s1 > s2 → retorno > 0
```

### ⚠️ Casos Especiais
```c
// n = 0: não compara nada
strncmp("abc", "xyz", 0);  // → 0

// Uma string termina antes de n
strncmp("hi", "hello", 5);  // s1 termina no índice 2
// Compara: 'h'='h', 'i'='e' → 'i' > 'e' → resultado > 0

// Strings idênticas mas uma é mais longa
strncmp("test", "testing", 4);  // → 0 (iguais nos 4 primeiros)
strncmp("test", "testing", 5);  // → 't' vs '\0' → resultado < 0
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>
#include <string.h>

void demonstrar_comparacao_basica(void)
{
    printf("=== COMPARAÇÕES BÁSICAS ===\n\n");
    
    char *str1 = "programming";
    char *str2 = "program";
    
    // Comparações com diferentes valores de n
    printf("str1: \"%s\"\n", str1);
    printf("str2: \"%s\"\n\n", str2);
    
    for (int n = 1; n <= 8; n++)
    {
        int result = ft_strncmp(str1, str2, n);
        printf("ft_strncmp(str1, str2, %d) = %2d ", n, result);
        
        if (result < 0)
            printf("(str1 < str2)\n");
        else if (result > 0)
            printf("(str1 > str2)\n");
        else
            printf("(str1 == str2)\n");
    }
    printf("\n");
}

void exemplo_prefixos(void)
{
    printf("=== VERIFICAÇÃO DE PREFIXOS ===\n\n");
    
    char *urls[] = {
        "https://google.com",
        "http://example.org",
        "ftp://files.server.com",
        "file:///local/path",
        NULL
    };
    
    char *protocols[] = {"http://", "https://", "ftp://", NULL};
    
    for (int i = 0; urls[i]; i++)
    {
        printf("URL: %s\n", urls[i]);
        
        for (int j = 0; protocols[j]; j++)
        {
            int len = strlen(protocols[j]);
            int result = ft_strncmp(urls[i], protocols[j], len);
            
            printf("  %s: %s\n", protocols[j], 
                   result == 0 ? "✅ MATCH" : "❌ NO");
        }
        printf("\n");
    }
}

void exemplo_comandos_shell(void)
{
    printf("=== COMANDOS DE SHELL (PREFIXOS) ===\n\n");
    
    char *input[] = {"ls", "list", "l", "cat", "ca", "copy", NULL};
    char *commands[] = {"ls", "list", "cat", "copy", NULL};
    
    for (int i = 0; input[i]; i++)
    {
        printf("Entrada: \"%s\"\n", input[i]);
        
        for (int j = 0; commands[j]; j++)
        {
            int input_len = strlen(input[i]);
            int result = ft_strncmp(input[i], commands[j], input_len);
            
            if (result == 0)
                printf("  → Possível match: \"%s\"\n", commands[j]);
        }
        printf("\n");
    }
}

void exemplo_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    // n = 0
    printf("n = 0 (não compara nada):\n");
    printf("ft_strncmp(\"abc\", \"xyz\", 0) = %d\n\n", 
           ft_strncmp("abc", "xyz", 0));
    
    // Strings vazias
    printf("Strings vazias:\n");
    printf("ft_strncmp(\"\", \"\", 5) = %d\n", 
           ft_strncmp("", "", 5));
    printf("ft_strncmp(\"abc\", \"\", 3) = %d\n", 
           ft_strncmp("abc", "", 3));
    printf("ft_strncmp(\"\", \"abc\", 3) = %d\n\n", 
           ft_strncmp("", "abc", 3));
    
    // Caracteres especiais
    printf("Caracteres especiais:\n");
    printf("ft_strncmp(\"ABC\", \"abc\", 3) = %d (maiúscula < minúscula)\n",
           ft_strncmp("ABC", "abc", 3));
    printf("ft_strncmp(\"123\", \"12a\", 3) = %d (número < letra)\n",
           ft_strncmp("123", "12a", 3));
    printf("\n");
}

void comparar_com_original(void)
{
    printf("=== COMPARAÇÃO COM STRNCMP ORIGINAL ===\n\n");
    
    struct {
        char *s1;
        char *s2;
        size_t n;
    } tests[] = {
        {"hello", "hello", 5},
        {"hello", "world", 5},
        {"test", "testing", 4},
        {"test", "testing", 6},
        {"ABC", "abc", 3},
        {"", "", 0},
        {NULL, NULL, 0}
    };
    
    for (int i = 0; tests[i].s1; i++)
    {
        int orig = strncmp(tests[i].s1, tests[i].s2, tests[i].n);
        int mine = ft_strncmp(tests[i].s1, tests[i].s2, tests[i].n);
        
        printf("s1=\"%s\", s2=\"%s\", n=%zu\n", 
               tests[i].s1, tests[i].s2, tests[i].n);
        printf("  Original: %d, ft_strncmp: %d %s\n\n",
               orig, mine, 
               ((orig < 0 && mine < 0) || 
                (orig > 0 && mine > 0) || 
                (orig == 0 && mine == 0)) ? "✅" : "❌");
    }
}

int main(void)
{
    printf("🧪 DEMONSTRAÇÃO FT_STRNCMP\n");
    printf("==========================\n\n");
    
    demonstrar_comparacao_basica();
    exemplo_prefixos();
    exemplo_comandos_shell();
    exemplo_casos_especiais();
    comparar_com_original();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • Compara apenas os primeiros n caracteres\n");
    printf("   • Retorno: <0, 0, >0 (menor, igual, maior)\n");
    printf("   • Para no primeiro \\0 ou diferença\n");
    printf("   • Útil para prefixos e validações\n");
    
    return (0);
}
```

**Saída esperada:**
```
🧪 DEMONSTRAÇÃO FT_STRNCMP
==========================

=== COMPARAÇÕES BÁSICAS ===

str1: "programming"
str2: "program"

ft_strncmp(str1, str2, 1) =  0 (str1 == str2)
ft_strncmp(str1, str2, 2) =  0 (str1 == str2)
ft_strncmp(str1, str2, 3) =  0 (str1 == str2)
ft_strncmp(str1, str2, 4) =  0 (str1 == str2)
ft_strncmp(str1, str2, 5) =  0 (str1 == str2)
ft_strncmp(str1, str2, 6) =  0 (str1 == str2)
ft_strncmp(str1, str2, 7) =  0 (str1 == str2)
ft_strncmp(str1, str2, 8) = 109 (str1 > str2)

=== VERIFICAÇÃO DE PREFIXOS ===

URL: https://google.com
  http://: ❌ NO
  https://: ✅ MATCH
  ftp://: ❌ NO

URL: http://example.org
  http://: ✅ MATCH
  https://: ❌ NO
  ftp://: ❌ NO

=== COMANDOS DE SHELL (PREFIXOS) ===

Entrada: "ls"
  → Possível match: "ls"

Entrada: "list"
  → Possível match: "list"

Entrada: "l"
  → Possível match: "ls"
  → Possível match: "list"

💡 LEMBRE-SE:
   • Compara apenas os primeiros n caracteres
   • Retorno: <0, 0, >0 (menor, igual, maior)
   • Para no primeiro \0 ou diferença
   • Útil para prefixos e validações
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Ordem lexicográfica:** Como strings são ordenadas
2. **Valores ASCII:** Tabela de caracteres e seus códigos
3. **Unsigned char casting:** Por que é importante
4. **Comparação limitada:** Diferença de strcmp

### 🎯 Perguntas para Reflexão
- Por que retornamos diferença entre caracteres?
- Como tratar caracteres com sinal negativo?
- O que acontece quando n > comprimento das strings?
- Por que n=0 sempre retorna 0?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Clássica com Loop
```c
int ft_strncmp(const char *s1, const char *s2, size_t n)
{
    size_t i;
    
    i = 0;
    while (i < n && (s1[i] || s2[i]))
    {
        if (s1[i] != s2[i])
            return ((unsigned char)s1[i] - (unsigned char)s2[i]);
        i++;
    }
    return (0);
}
```

### 💭 Abordagem 2: Ponteiros com Incremento
```c
int ft_strncmp(const char *s1, const char *s2, size_t n)
{
    while (n > 0 && (*s1 || *s2))
    {
        if (*s1 != *s2)
            return ((unsigned char)*s1 - (unsigned char)*s2);
        s1++;
        s2++;
        n--;
    }
    return (0);
}
```

### 💭 Abordagem 3: Verificação Antecipada
```c
int ft_strncmp(const char *s1, const char *s2, size_t n)
{
    if (n == 0)
        return (0);
        
    while (n-- > 0)
    {
        if (*s1 != *s2 || *s1 == '\0')
            return ((unsigned char)*s1 - (unsigned char)*s2);
        s1++;
        s2++;
    }
    return (0);
}
```

### 🔧 Dicas de Implementação
- **Cast para unsigned char:** Evitar problemas com caracteres negativos
- **Verificar n=0:** Retornar 0 imediatamente
- **Parar em \0:** Mesmo que n não tenha sido atingido
- **Diferença aritmética:** Retornar s1[i] - s2[i], não apenas -1/0/1

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_strncmp básica
2. Teste com strings idênticas de diferentes tamanhos
3. Compare com strncmp original

### 🥈 Nível Intermediário
1. Teste com caracteres especiais e acentuados
2. Implemente versão case-insensitive (strncasecmp)
3. Crie testes para todos os casos especiais

### 🥇 Nível Avançado
1. Otimize para comparar palavras inteiras por vez
2. Implemente versão que ignora espaços
3. Crie ft_strnrcmp (compara do final para início)

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - Comparação
- `strcmp` - Compara strings completas
- `strcasecmp` - Compara ignorando maiúsculas/minúsculas
- `strncasecmp` - Versão limitada da anterior

### 🔄 Funções de Busca
- [`ft_strchr`](ft_strchr.md) - Busca caractere na string
- [`ft_strrchr`](ft_strrchr.md) - Busca caractere do final
- [`ft_strnstr`](ft_strnstr.md) - Busca substring limitada

### 📝 Funções de Memória
- [`ft_memcmp`](../memory/ft_memcmp.md) - Compara blocos de memória

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔤 Ordenação Lexicográfica](../../resources/lexicographic_order.md)
- [🔢 Tabela ASCII](../../resources/ascii_table.md)
- [🎯 Comparação de Strings](../../resources/string_comparison.md)

### 🔗 Referências Externas
- Manual do C: `man strncmp`
- [C Reference - String Comparison](https://en.cppreference.com/w/c/string/byte/strncmp)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender ordem lexicográfica
- [ ] Por que cast para unsigned char
- [ ] Diferença entre retornar -1/0/1 vs diferença real
- [ ] Casos especiais (n=0, strings vazias)

**Descobertas importantes:**
- [ ] strncmp é fundamental para prefixos
- [ ] Útil para validação de protocolos
- [ ] Evita comparações desnecessárias
- [ ] Base para muitas operações de string

**Testes que fiz:**
- [ ] Strings com caracteres especiais
- [ ] Diferentes valores de n
- [ ] Comparação com versão original
- [ ] Casos de prefixos reais

---
<div align="center">

[← Função Anterior: ft_strrchr](ft_strrchr.md) | [Próxima Função: ft_strlcpy →](ft_strlcpy.md)

🔤 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
