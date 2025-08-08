# ft_strnstr

> 🔍 Busca substring em string com limite de comprimento

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strnstr.c`

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
char *ft_strnstr(const char *haystack, const char *needle, size_t len);
```

> 💡 **Observação:** Busca apenas nos primeiros `len` caracteres da string principal.

---

## 📖 DESCRIÇÃO

A função `ft_strnstr` localiza a primeira ocorrência da string `needle` dentro da string `haystack`, limitando a busca aos primeiros `len` caracteres de `haystack`.

### 🎯 Objetivo
Encontrar substrings de forma segura e controlada, limitando o escopo da busca para evitar acessos desnecessários à memória.

### 🌍 Aplicações Reais
- **Parsing de protocolos:** Buscar headers HTTP limitados
- **Análise de logs:** Procurar padrões em linhas específicas
- **Validação de entrada:** Verificar prefixos em dados de entrada
- **Busca em buffers:** Localizar padrões em dados binários

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `haystack` | `const char *` | String onde buscar (palheiro) |
| `needle` | `const char *` | String a ser encontrada (agulha) |
| `len` | `size_t` | Número máximo de caracteres para buscar |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Encontrou | `char *` | Ponteiro para primeira ocorrência |
| Não encontrou | `NULL` | Substring não existe no limite |
| Needle vazia | `haystack` | String vazia sempre "encontrada" |
| Parâmetros inválidos | `NULL` | haystack NULL ou len = 0 |

### 📊 Exemplos de Comportamento

| haystack | needle | len | Resultado | Explicação |
|----------|--------|-----|-----------|------------|
| "Hello World" | "World" | 11 | → "World" | Encontrou dentro do limite |
| "Hello World" | "World" | 8 | `NULL` | Fora do limite (chars 1-8) |
| "Hello World" | "Hello" | 11 | → "Hello World" | Encontrou no início |
| "Test String" | "" | 5 | → "Test String" | Needle vazia |
| "Short" | "LongNeedle" | 10 | `NULL` | Needle maior que haystack |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Algoritmo Básico
```c
// 1. Verificar casos especiais (needle vazia, parâmetros inválidos)
// 2. Para cada posição i em haystack (até len):
//    a. Comparar needle[0..j] com haystack[i..i+j]
//    b. Se toda needle foi encontrada, retornar &haystack[i]
// 3. Se não encontrou, retornar NULL
```

### 🔍 Visualização da Busca
```
haystack: "Hello World Test"
needle:   "World"
len:      11

Posição:   0123456789AB
          "Hello World"  ← Busca limitada aos primeiros 11 chars
           ^^^^^         ← Tenta "Hello" != "World"
            ^^^^         ← Tenta "ello " != "World"  
             ^^^         ← Tenta "llo W" != "World"
              ^^         ← Tenta "lo Wo" != "World"
               ^         ← Tenta "o Wor" != "World"
                ^^^^     ← Tenta " Worl" != "World"
                 ^^^^    ← Tenta "World" == "World" ✅
```

### ⚠️ Diferença do strstr
```c
// strstr: busca em toda string
char *ptr1 = strstr("Hello World Test", "Test");
// ptr1 = "Test" ← Encontra

// strnstr: busca limitada
char *ptr2 = ft_strnstr("Hello World Test", "Test", 11);  
// ptr2 = NULL ← Não encontra (Test está na posição 12)
```

### 🔍 Casos Especiais Importantes
```c
// 1. Needle vazia sempre retorna haystack
ft_strnstr("Hello", "", 5) → "Hello"

// 2. Haystack NULL retorna NULL
ft_strnstr(NULL, "test", 5) → NULL

// 3. Len = 0 retorna NULL
ft_strnstr("Hello", "He", 0) → NULL

// 4. Needle maior que limite
ft_strnstr("Hi", "Hello", 5) → NULL
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>
#include <string.h>

void demonstrar_busca_basica(void)
{
    printf("=== BUSCA BÁSICA ===\n\n");
    
    const char *texto = "Aprendendo programação na 42";
    char *resultado;
    
    // Busca simples
    resultado = ft_strnstr(texto, "programação", strlen(texto));
    printf("Buscando 'programação': %s\n", 
           resultado ? resultado : "NÃO ENCONTRADO");
    
    // Busca no início
    resultado = ft_strnstr(texto, "Aprendendo", strlen(texto));
    printf("Buscando 'Aprendendo': %s\n", 
           resultado ? resultado : "NÃO ENCONTRADO");
    
    // Busca inexistente
    resultado = ft_strnstr(texto, "Python", strlen(texto));
    printf("Buscando 'Python': %s\n", 
           resultado ? resultado : "NÃO ENCONTRADO");
    
    printf("\n");
}

void exemplo_limite_comprimento(void)
{
    printf("=== LIMITE DE COMPRIMENTO ===\n\n");
    
    const char *frase = "Estudar é fundamental para crescer";
    char *resultado;
    
    printf("Frase completa: \"%s\"\n", frase);
    printf("Comprimento total: %zu\n\n", strlen(frase));
    
    // Busca dentro do limite
    resultado = ft_strnstr(frase, "fundamental", 20);
    printf("Busca 'fundamental' nos primeiros 20 chars: %s\n", 
           resultado ? "ENCONTRADO" : "NÃO ENCONTRADO");
    
    // Busca fora do limite  
    resultado = ft_strnstr(frase, "crescer", 20);
    printf("Busca 'crescer' nos primeiros 20 chars: %s\n", 
           resultado ? "ENCONTRADO" : "NÃO ENCONTRADO");
    
    // Mostra exatamente os primeiros 20 chars
    printf("\nPrimeiros 20 chars: \"");
    for (int i = 0; i < 20 && frase[i]; i++)
        printf("%c", frase[i]);
    printf("\"\n\n");
}

void exemplo_parsing_http(void)
{
    printf("=== PARSING HTTP ===\n\n");
    
    const char *header = "GET /api/users HTTP/1.1\r\nHost: example.com\r\nUser-Agent: curl";
    char *resultado;
    
    printf("Header HTTP:\n%s\n\n", header);
    
    // Buscar método HTTP (só no começo)
    resultado = ft_strnstr(header, "GET", 10);
    printf("Método encontrado: %s\n", 
           resultado ? "SIM" : "NÃO");
    
    // Buscar Host header
    resultado = ft_strnstr(header, "Host:", strlen(header));
    if (resultado)
    {
        printf("Host header: ");
        char *linha = resultado;
        while (*linha && *linha != '\r' && *linha != '\n')
            printf("%c", *linha++);
        printf("\n");
    }
    
    // Buscar User-Agent limitado
    resultado = ft_strnstr(header, "User-Agent", 50);
    printf("User-Agent nos primeiros 50 chars: %s\n", 
           resultado ? "ENCONTRADO" : "NÃO ENCONTRADO");
    
    printf("\n");
}

void exemplo_validacao_entrada(void)
{
    printf("=== VALIDAÇÃO DE ENTRADA ===\n\n");
    
    const char *comandos[] = {
        "list --all",
        "create new_file.txt", 
        "delete old_file.txt",
        "help commands",
        "exit program"
    };
    
    int total = sizeof(comandos) / sizeof(comandos[0]);
    
    // Verificar comandos válidos
    const char *validos[] = {"list", "create", "delete", "help"};
    int total_validos = sizeof(validos) / sizeof(validos[0]);
    
    for (int i = 0; i < total; i++)
    {
        printf("Comando: \"%s\" → ", comandos[i]);
        
        int encontrou = 0;
        for (int j = 0; j < total_validos; j++)
        {
            // Busca só no início (primeiras 10 chars)
            char *resultado = ft_strnstr(comandos[i], validos[j], 10);
            if (resultado == comandos[i]) // Deve ser no início
            {
                printf("✅ VÁLIDO (%s)\n", validos[j]);
                encontrou = 1;
                break;
            }
        }
        
        if (!encontrou)
            printf("❌ INVÁLIDO\n");
    }
    
    printf("\n");
}

void demonstrar_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    char *resultado;
    
    // Needle vazia
    resultado = ft_strnstr("Hello World", "", 5);
    printf("Needle vazia: %s\n", resultado ? resultado : "NULL");
    
    // Haystack vazio
    resultado = ft_strnstr("", "test", 5);
    printf("Haystack vazio: %s\n", resultado ? resultado : "NULL");
    
    // Limite zero
    resultado = ft_strnstr("Hello", "He", 0);
    printf("Limite zero: %s\n", resultado ? resultado : "NULL");
    
    // Needle maior que haystack
    resultado = ft_strnstr("Hi", "Hello", 10);
    printf("Needle > haystack: %s\n", resultado ? resultado : "NULL");
    
    // Padrão na borda do limite
    resultado = ft_strnstr("Hello", "lo", 5);
    printf("Padrão na borda (exato): %s\n", resultado ? resultado : "NULL");
    
    resultado = ft_strnstr("Hello", "lo", 4);
    printf("Padrão na borda (insuficiente): %s\n", resultado ? resultado : "NULL");
    
    printf("\n");
}

int main(void)
{
    printf("🔍 DEMONSTRAÇÃO FT_STRNSTR\n");
    printf("==========================\n\n");
    
    demonstrar_busca_basica();
    exemplo_limite_comprimento();
    exemplo_parsing_http();
    exemplo_validacao_entrada();
    demonstrar_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • strnstr limita a busca aos primeiros len chars\n");
    printf("   • Needle vazia sempre retorna haystack\n");
    printf("   • Retorna ponteiro para primeira ocorrência\n");
    printf("   • NULL se não encontrar ou parâmetros inválidos\n");
    printf("   • Mais segura que strstr para buffers limitados\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔍 DEMONSTRAÇÃO FT_STRNSTR
==========================

=== BUSCA BÁSICA ===

Buscando 'programação': programação na 42
Buscando 'Aprendendo': Aprendendo programação na 42
Buscando 'Python': NÃO ENCONTRADO

=== LIMITE DE COMPRIMENTO ===

Frase completa: "Estudar é fundamental para crescer"
Comprimento total: 34

Busca 'fundamental' nos primeiros 20 chars: ENCONTRADO
Busca 'crescer' nos primeiros 20 chars: NÃO ENCONTRADO

Primeiros 20 chars: "Estudar é fundament"

=== PARSING HTTP ===

Header HTTP:
GET /api/users HTTP/1.1
Host: example.com
User-Agent: curl

Método encontrado: SIM
Host header: Host: example.com
User-Agent nos primeiros 50 chars: NÃO ENCONTRADO

=== VALIDAÇÃO DE ENTRADA ===

Comando: "list --all" → ✅ VÁLIDO (list)
Comando: "create new_file.txt" → ✅ VÁLIDO (create)
Comando: "delete old_file.txt" → ✅ VÁLIDO (delete)
Comando: "help commands" → ✅ VÁLIDO (help)
Comando: "exit program" → ❌ INVÁLIDO

=== CASOS ESPECIAIS ===

Needle vazia: Hello World
Haystack vazio: NULL
Limite zero: NULL
Needle > haystack: NULL
Padrão na borda (exato): lo
Padrão na borda (insuficiente): NULL

💡 LEMBRE-SE:
   • strnstr limita a busca aos primeiros len chars
   • Needle vazia sempre retorna haystack
   • Retorna ponteiro para primeira ocorrência
   • NULL se não encontrar ou parâmetros inválidos
   • Mais segura que strstr para buffers limitados
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Algoritmos de busca:** Como funciona a busca de substring
2. **Ponteiros:** Retorno de endereços dentro de strings
3. **Limites de array:** Importância de não ultrapassar len
4. **Casos especiais:** Tratamento de entradas inválidas

### 🎯 Perguntas para Reflexão
- Por que strnstr é mais segura que strstr?
- Como tratar o caso da needle vazia?
- O que acontece se needle for maior que o limite?
- Como garantir que não acessamos memória além de len?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Dois Loops
```c
char *ft_strnstr(const char *haystack, const char *needle, size_t len)
{
    size_t i, j;
    
    if (!needle || *needle == '\0')
        return ((char *)haystack);
    if (!haystack || len == 0)
        return (NULL);
        
    i = 0;
    while (haystack[i] && i < len)
    {
        j = 0;
        while (needle[j] && haystack[i + j] && (i + j) < len
               && haystack[i + j] == needle[j])
            j++;
        if (needle[j] == '\0')
            return ((char *)&haystack[i]);
        i++;
    }
    return (NULL);
}
```

### 💭 Abordagem 2: Com Helpers
```c
char *ft_strnstr(const char *haystack, const char *needle, size_t len)
{
    size_t needle_len;
    size_t i;
    
    if (!needle || !*needle)
        return ((char *)haystack);
    if (!haystack || len == 0)
        return (NULL);
        
    needle_len = ft_strlen(needle);
    if (needle_len > len)
        return (NULL);
        
    i = 0;
    while (haystack[i] && i <= len - needle_len)
    {
        if (ft_strncmp(&haystack[i], needle, needle_len) == 0)
            return ((char *)&haystack[i]);
        i++;
    }
    return (NULL);
}
```

### 🔧 Dicas de Implementação
- **Casos especiais primeiro:** Tratar needle vazia e parâmetros NULL
- **Verificar limites:** Sempre garantir que (i + j) < len
- **Otimização:** Se needle é maior que len, retornar NULL imediatamente
- **Dependências:** Pode usar ft_strlen e ft_strncmp para simplificar

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente strnstr básica com dois loops
2. Teste com strings simples e diferentes valores de len
3. Verifique todos os casos especiais

### 🥈 Nível Intermediário
1. Otimize usando ft_strlen e ft_strncmp
2. Adicione proteção contra overflow nos índices
3. Crie versão case-insensitive (strncasestr)

### 🥇 Nível Avançado
1. Implemente algoritmo de busca mais eficiente (KMP ou Boyer-Moore)
2. Crie versão para wide characters (wcsnstr)
3. Adicione suporte a expressões regulares simples

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - String
- [`ft_strlen`](ft_strlen.md) - Pode ser usada para otimizações
- [`ft_strncmp`](ft_strncmp.md) - Pode ser usada para comparar substrings
- [`ft_strchr`](ft_strchr.md) - Busca caractere único
- [`ft_strrchr`](ft_strrchr.md) - Busca caractere do final

### 🔄 Funções de Busca
- `strstr` - Busca em string completa
- `strcasestr` - Busca case-insensitive
- `memmem` - Busca em memória bruta

### 📝 Aplicações Práticas
- [`ft_split`](../additional/ft_split.md) - Usa conceitos de busca
- Parsing de protocolos
- Validação de entrada

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔍 String Searching Algorithms](../../resources/string_search.md)
- [📏 Bounded String Operations](../../resources/bounded_strings.md)
- [🛡️ Safe String Handling](../../resources/safe_strings.md)

### 🔗 Referências Externas
- Manual do C: `man strnstr`
- [GNU C Library - String Search](https://www.gnu.org/software/libc/manual/html_node/Search-Functions.html)
- [String Algorithms - Substring Search](https://www.cs.princeton.edu/courses/archive/cos126/fall06/lectures/strings.pdf)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender como limitar a busca corretamente
- [ ] Tratar o caso da needle vazia
- [ ] Garantir que não acesso memória além de len
- [ ] Implementar a lógica de comparação aninhada

**Descobertas importantes:**
- [ ] strnstr é fundamental para parsing seguro
- [ ] Needle vazia tem comportamento especial
- [ ] Limites devem ser verificados em ambos os loops
- [ ] Retorno de ponteiro permite acesso ao contexto

**Testes que fiz:**
- [ ] Busca básica em diferentes posições
- [ ] Casos com limite menor que a string
- [ ] Needle vazia e parâmetros NULL
- [ ] Padrões na borda do limite

---
<div align="center">

[← Função Anterior: ft_strlcat](ft_strlcat.md) | [Próxima Função: ft_strdup →](ft_strdup.md)

🔤 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
