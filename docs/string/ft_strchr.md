# ft_strchr

> 🔍 Busca a primeira ocorrência de um caractere na string

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strchr.c`

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
char *ft_strchr(const char *s, int c);
```

> 💡 **Observação:** Retorna ponteiro para o caractere encontrado, não o índice.

---

## 📖 DESCRIÇÃO

A função `ft_strchr` busca a **primeira ocorrência** do caractere `c` na string `s` e retorna um ponteiro para essa posição. Se o caractere não for encontrado, retorna `NULL`.

### 🎯 Objetivo
Localizar rapidamente um caractere específico em uma string, fundamental para parsing, validação e processamento de texto.

### 🌍 Aplicações Reais
- **Parsing de dados:** Encontrar delimitadores em CSV, JSON
- **Validação de entrada:** Verificar caracteres proibidos
- **Navegação em strings:** Encontrar pontos de divisão
- **Busca de extensões:** Localizar '.' em nomes de arquivo

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s` | `const char *` | String onde buscar |
| `c` | `int` | Caractere a buscar (convertido para char) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Encontrou | `char *` | Ponteiro para primeira ocorrência do caractere |
| Não encontrou | `NULL` | Caractere não existe na string |
| Busca por '\0' | `char *` | Ponteiro para o terminador null |

### 📊 Exemplos de Comportamento

| String | Caractere | Resultado | Explicação |
|--------|-----------|-----------|------------|
| `"Hello"` | `'e'` | `&s[1]` | Ponteiro para 'e' na posição 1 |
| `"Hello"` | `'x'` | `NULL` | Caractere não existe |
| `"Hello"` | `'\0'` | `&s[5]` | Ponteiro para terminador |
| `"aaa"` | `'a'` | `&s[0]` | Primeira ocorrência (posição 0) |
| `""` | `'a'` | `NULL` | String vazia |
| `""` | `'\0'` | `&s[0]` | Terminador em string vazia |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Algoritmo Básico
```c
1. Converter int c para char
2. Percorrer string caractere por caractere
3. Se encontrar c, retornar ponteiro atual
4. Se chegar ao fim (\0):
   - Se c == '\0', retornar ponteiro para \0
   - Senão, retornar NULL
```

### 🔍 Diferenças Importantes
```c
// ❌ CONFUSÃO COMUM: retornar índice
int index = 1;  // Posição do caractere
return (&index);  // ERRADO!

// ✅ CORRETO: retornar ponteiro
return (&s[1]);  // Ponteiro para posição 1
return (s + 1);  // Equivalente
```

### ⚠️ Caso Especial: Busca por '\0'
```c
char str[] = "Hello";
char *result = strchr(str, '\0');
// result aponta para str[5] (o '\0')
// Isso é VÁLIDO e esperado!

printf("%p\n", str);     // 0x7fff...
printf("%p\n", result);  // 0x7fff... + 5
```

### 🎯 Cast de int para char
```c
// Por que o parâmetro é int?
// Compatibilidade com funções como getchar() que retornam int
int c = getchar();  // Pode ser EOF (-1)
char *pos = strchr(string, c);  // Funciona!

// Internamente:
char ch = (char)c;  // Cast necessário
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_busca_basica(void)
{
    printf("=== BUSCA BÁSICA ===\n\n");
    
    char *texto = "Programação em C";
    char *resultado;
    
    // Busca caractere existente
    resultado = ft_strchr(texto, 'g');
    if (resultado)
    {
        printf("String: \"%s\"\n", texto);
        printf("Buscar: 'g'\n");
        printf("Encontrado na posição: %ld\n", resultado - texto);
        printf("Resto da string: \"%s\"\n\n", resultado);
    }
    
    // Busca caractere inexistente
    resultado = ft_strchr(texto, 'x');
    printf("Buscar 'x' em \"%s\": ", texto);
    if (resultado)
        printf("Encontrado!\n");
    else
        printf("❌ Não encontrado (NULL)\n\n");
}

void exemplo_parsing_csv(void)
{
    printf("=== PARSING CSV ===\n\n");
    
    char linha[] = "João,25,Programador,São Paulo";
    char *inicio = linha;
    char *virgula;
    int campo = 1;
    
    printf("Linha CSV: \"%s\"\n\n", linha);
    
    while ((virgula = ft_strchr(inicio, ',')) != NULL)
    {
        // Imprime campo atual
        *virgula = '\0';  // Temporariamente termina string
        printf("Campo %d: \"%s\"\n", campo, inicio);
        *virgula = ',';   // Restaura vírgula
        
        // Move para próximo campo
        inicio = virgula + 1;
        campo++;
    }
    
    // Último campo (após última vírgula)
    printf("Campo %d: \"%s\"\n\n", campo, inicio);
}

void exemplo_validacao_email(void)
{
    printf("=== VALIDAÇÃO EMAIL ===\n\n");
    
    char emails[][30] = {
        "user@domain.com",
        "invalid.email",
        "another@test.org",
        "no-at-symbol.com"
    };
    
    for (int i = 0; i < 4; i++)
    {
        char *at_pos = ft_strchr(emails[i], '@');
        char *dot_pos = NULL;
        
        if (at_pos)
            dot_pos = ft_strchr(at_pos, '.');
        
        printf("Email: %-20s → ", emails[i]);
        
        if (at_pos && dot_pos && at_pos < dot_pos)
            printf("✅ Formato básico válido\n");
        else
            printf("❌ Formato inválido\n");
    }
    printf("\n");
}

void exemplo_busca_extensao(void)
{
    printf("=== BUSCA EXTENSÃO ARQUIVO ===\n\n");
    
    char arquivos[][20] = {
        "documento.txt",
        "imagem.jpg",
        "programa.c",
        "sem_extensao",
        "arquivo.backup.zip"
    };
    
    for (int i = 0; i < 5; i++)
    {
        char *ultimo_ponto = NULL;
        char *temp = arquivos[i];
        
        // Encontra o ÚLTIMO ponto (não o primeiro)
        while ((temp = ft_strchr(temp, '.')) != NULL)
        {
            ultimo_ponto = temp;
            temp++;  // Move para depois do ponto
        }
        
        printf("Arquivo: %-18s → Extensão: ", arquivos[i]);
        if (ultimo_ponto)
            printf("\"%s\"\n", ultimo_ponto + 1);
        else
            printf("(sem extensão)\n");
    }
    printf("\n");
}

void exemplo_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    // Busca por '\0'
    char *str = "Hello";
    char *null_pos = ft_strchr(str, '\0');
    printf("Buscar '\\0' em \"%s\":\n", str);
    printf("  Posição do \\0: %ld\n", null_pos - str);
    printf("  Comprimento string: %ld\n\n", strlen(str));
    
    // String vazia
    char vazia[] = "";
    char *result = ft_strchr(vazia, 'a');
    printf("Buscar 'a' em string vazia: %s\n", result ? "Encontrou" : "NULL");
    
    result = ft_strchr(vazia, '\0');
    printf("Buscar '\\0' em string vazia: %s\n\n", result ? "Encontrou" : "NULL");
    
    // Caracteres duplicados
    char duplicados[] = "abcabc";
    char *primeira_a = ft_strchr(duplicados, 'a');
    printf("String com duplicados: \"%s\"\n", duplicados);
    printf("Primeira ocorrência de 'a': posição %ld\n", primeira_a - duplicados);
}

int main(void)
{
    printf("🔍 DEMONSTRAÇÃO FT_STRCHR\n");
    printf("=========================\n\n");
    
    demonstrar_busca_basica();
    exemplo_parsing_csv();
    exemplo_validacao_email();
    exemplo_busca_extensao();
    exemplo_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • Retorna PONTEIRO, não índice\n");
    printf("   • Primeira ocorrência quando há duplicatas\n");
    printf("   • Deve encontrar '\\0' se buscar por ele\n");
    printf("   • NULL quando não encontra\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔍 DEMONSTRAÇÃO FT_STRCHR
=========================

=== BUSCA BÁSICA ===

String: "Programação em C"
Buscar: 'g'
Encontrado na posição: 3
Resto da string: "gramação em C"

Buscar 'x' em "Programação em C": ❌ Não encontrado (NULL)

=== PARSING CSV ===

Linha CSV: "João,25,Programador,São Paulo"

Campo 1: "João"
Campo 2: "25"
Campo 3: "Programador"
Campo 4: "São Paulo"

=== VALIDAÇÃO EMAIL ===

Email: user@domain.com      → ✅ Formato básico válido
Email: invalid.email        → ❌ Formato inválido
Email: another@test.org     → ✅ Formato básico válido
Email: no-at-symbol.com     → ❌ Formato inválido

=== BUSCA EXTENSÃO ARQUIVO ===

Arquivo: documento.txt      → Extensão: "txt"
Arquivo: imagem.jpg         → Extensão: "jpg"
Arquivo: programa.c         → Extensão: "c"
Arquivo: sem_extensao       → (sem extensão)
Arquivo: arquivo.backup.zip → Extensão: "zip"

=== CASOS ESPECIAIS ===

Buscar '\0' em "Hello":
  Posição do \0: 5
  Comprimento string: 5

Buscar 'a' em string vazia: NULL
Buscar '\0' em string vazia: Encontrou

String com duplicados: "abcabc"
Primeira ocorrência de 'a': posição 0

💡 LEMBRE-SE:
   • Retorna PONTEIRO, não índice
   • Primeira ocorrência quando há duplicatas
   • Deve encontrar '\0' se buscar por ele
   • NULL quando não encontra
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Ponteiros:** Como retornar endereço de memória
2. **Cast de tipos:** Por que int vira char
3. **String terminators:** Comportamento com '\0'
4. **Comparação de ponteiros:** NULL vs endereço válido

### 🎯 Perguntas para Reflexão
- Por que retornar ponteiro ao invés de índice?
- Como lidar com o caso especial do '\0'?
- Por que o parâmetro é int se vamos usar como char?
- Qual a diferença entre primeira e última ocorrência?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Loop Clássico
```c
char *ft_strchr(const char *s, int c)
{
    char ch = (char)c;
    
    while (*s)
    {
        if (*s == ch)
            return ((char *)s);
        s++;
    }
    if (ch == '\0')
        return ((char *)s);
    return (NULL);
}
```

### 💭 Abordagem 2: Loop com Índice
```c
char *ft_strchr(const char *s, int c)
{
    char ch = (char)c;
    int  i = 0;
    
    while (s[i])
    {
        if (s[i] == ch)
            return ((char *)&s[i]);
        i++;
    }
    if (ch == '\0')
        return ((char *)&s[i]);
    return (NULL);
}
```

### 💭 Abordagem 3: Loop Unificado
```c
char *ft_strchr(const char *s, int c)
{
    char ch = (char)c;
    
    while (*s != ch)
    {
        if (*s == '\0')
            return (NULL);
        s++;
    }
    return ((char *)s);
}
```

### 🔧 Dicas de Implementação
- **Cast obrigatório:** `(char)c` para converter int
- **Verificar '\0':** Caso especial após o loop principal
- **Cast de retorno:** `(char *)s` para remover const
- **Primeira ocorrência:** Retornar imediatamente quando encontrar

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_strchr básica
2. Teste com strings simples
3. Verifique comportamento com '\0'

### 🥈 Nível Intermediário
1. Crie ft_strrchr (busca da direita para esquerda)
2. Implemente função que conta ocorrências
3. Desenvolva parser simples usando strchr

### 🥇 Nível Avançado
1. Implemente strpbrk (busca qualquer caractere de um conjunto)
2. Crie versão que ignora maiúsculas/minúsculas
3. Otimize para strings muito grandes usando SIMD

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - String
- [`ft_strrchr`](ft_strrchr.md) - Busca da direita para esquerda
- [`ft_strlen`](ft_strlen.md) - Calcula comprimento da string
- [`ft_strnstr`](ft_strnstr.md) - Busca substring em string

### 🔄 Funções Complementares
- `strpbrk` - Busca qualquer caractere de um conjunto
- `strcspn` - Conta caracteres até encontrar um do conjunto
- `strspn` - Conta caracteres enquanto estão no conjunto

### 📝 Funções que Usam strchr
- [`ft_split`](../additional/ft_split.md) - Usa para encontrar delimitadores
- [`ft_strtrim`](../additional/ft_strtrim.md) - Pode usar para buscar caracteres

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔍 Busca em Strings](../../resources/string_searching.md)
- [📍 Ponteiros e Endereços](../../resources/pointers_addresses.md)
- [🎯 Parsing de Dados](../../resources/data_parsing.md)

### 🔗 Referências Externas
- Manual do C: `man strchr`
- [C Reference - String Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender diferença entre retornar ponteiro vs índice
- [ ] Lembrar do caso especial do '\0'
- [ ] Compreender por que cast int → char
- [ ] Diferença entre primeira e última ocorrência

**Descobertas importantes:**
- [ ] Ponteiro permite acessar resto da string
- [ ] '\0' é um caractere válido para busca
- [ ] Cast é necessário para compatibilidade
- [ ] Muito usado em parsing e validação

**Testes que fiz:**
- [ ] Caracteres no início, meio e fim
- [ ] Busca por '\0' (terminador)
- [ ] String vazia
- [ ] Caracteres duplicados
- [ ] Cast de int para char

---
<div align="center">

[← Função Anterior: ft_strlen](ft_strlen.md) | [Próxima Função: ft_strrchr →](ft_strrchr.md)

🔍 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
