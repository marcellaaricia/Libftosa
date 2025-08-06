# ft_strrchr

> 🔍 Busca a última ocorrência de um caractere na string (da direita para esquerda)

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strrchr.c`

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
char *ft_strrchr(const char *s, int c);
```

> 💡 **Observação:** Similar ao strchr, mas busca a ÚLTIMA ocorrência, não a primeira.

---

## 📖 DESCRIÇÃO

A função `ft_strrchr` busca a **última ocorrência** do caractere `c` na string `s` e retorna um ponteiro para essa posição. Se o caractere não for encontrado, retorna `NULL`.

### 🎯 Objetivo
Localizar a última aparição de um caractere em uma string, essencial para parsing reverso, busca de extensões de arquivo e processamento de caminhos.

### 🌍 Aplicações Reais
- **Extensões de arquivo:** Encontrar último '.' em "arquivo.backup.zip"
- **Caminhos de diretório:** Localizar último '/' ou '\' em paths
- **Parsing reverso:** Processar dados da direita para esquerda
- **Validação:** Encontrar último delimitador em formatos específicos

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
| Encontrou | `char *` | Ponteiro para última ocorrência do caractere |
| Não encontrou | `NULL` | Caractere não existe na string |
| Busca por '\0' | `char *` | Ponteiro para o terminador null |

### 📊 Exemplos de Comportamento

| String | Caractere | strchr | strrchr | Explicação |
|--------|-----------|--------|---------|------------|
| `"abcabc"` | `'a'` | `&s[0]` | `&s[3]` | Primeira vs última 'a' |
| `"abcabc"` | `'c'` | `&s[2]` | `&s[5]` | Primeira vs última 'c' |
| `"Hello"` | `'l'` | `&s[2]` | `&s[3]` | Há duas ocorrências de 'l' |
| `"Hello"` | `'e'` | `&s[1]` | `&s[1]` | Só uma ocorrência |
| `"test"` | `'x'` | `NULL` | `NULL` | Não existe |
| `"test"` | `'\0'` | `&s[4]` | `&s[4]` | Terminador |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Algoritmo Básico
```c
1. Converter int c para char
2. Percorrer TODA a string
3. A cada ocorrência de c, atualizar ponteiro "last"
4. No final:
   - Se c == '\0', retornar ponteiro para terminador
   - Senão, retornar "last" (pode ser NULL)
```

### 🔍 Diferença Fundamental: strchr vs strrchr
```c
char str[] = "programming";

strchr(str, 'r'):  // PRIMEIRA ocorrência
   ↓
   "programming" → retorna &str[1]
    ^

strrchr(str, 'r'): // ÚLTIMA ocorrência  
   ↓
   "programming" → retorna &str[4]
       ^
```

### ⚠️ Ponto Crítico: Percorrer TODA a String
```c
// ❌ ERRADO: parar na primeira ocorrência
char *ft_strrchr_wrong(const char *s, int c)
{
    while (*s)
    {
        if (*s == c)
            return ((char *)s);  // PARA aqui - ERRADO!
        s++;
    }
    return (NULL);
}

// ✅ CORRETO: percorrer toda string
char *ft_strrchr_right(const char *s, int c)
{
    char *last = NULL;
    while (*s)
    {
        if (*s == c)
            last = (char *)s;    // ATUALIZA, continua
        s++;
    }
    return (last);
}
```

### 🎯 Tratamento do '\0'
```c
char str[] = "test";

// Buscar '\0' deve funcionar
char *end = ft_strrchr(str, '\0');
// end aponta para str[4] (o terminador)

// Este é um caso especial porque '\0' não está 
// no loop principal - precisa ser tratado separadamente
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void demonstrar_diferenca_strchr_strrchr(void)
{
    printf("=== STRCHR vs STRRCHR ===\n\n");
    
    char *texto = "programming";
    char *primeira_r = strchr(texto, 'r');
    char *ultima_r = ft_strrchr(texto, 'r');
    
    printf("String: \"%s\"\n", texto);
    printf("strchr('r'):  posição %ld (primeira)\n", primeira_r - texto);
    printf("strrchr('r'): posição %ld (última)\n", ultima_r - texto);
    
    // Visualização
    printf("\n");
    printf("p r o g r a m m i n g\n");
    printf("0 1 2 3 4 5 6 7 8 9 10\n");
    printf("  ^     ^              \n");
    printf("  |     └── última 'r' \n");
    printf("  └── primeira 'r'     \n\n");
}

void exemplo_extensao_arquivo(void)
{
    printf("=== BUSCA EXTENSÃO DE ARQUIVO ===\n\n");
    
    char arquivos[][30] = {
        "documento.txt",
        "backup.tar.gz", 
        "arquivo.backup.zip",
        "script.sh",
        "sem_extensao"
    };
    
    for (int i = 0; i < 5; i++)
    {
        char *ultimo_ponto = ft_strrchr(arquivos[i], '.');
        
        printf("Arquivo: %-18s → ", arquivos[i]);
        if (ultimo_ponto)
        {
            printf("Extensão: \"%s\"\n", ultimo_ponto + 1);
        }
        else
        {
            printf("(sem extensão)\n");
        }
    }
    printf("\n");
    
    // Demonstração: por que ÚLTIMO ponto?
    char *exemplo = "arquivo.backup.zip";
    char *primeiro = strchr(exemplo, '.');
    char *ultimo = ft_strrchr(exemplo, '.');
    
    printf("Arquivo: \"%s\"\n", exemplo);
    printf("Primeiro '.': \"%s\" (extensão errada!)\n", primeiro + 1);
    printf("Último '.':   \"%s\" (extensão correta!)\n", ultimo + 1);
    printf("\n");
}

void exemplo_caminho_diretorio(void)
{
    printf("=== CAMINHO DE DIRETÓRIO ===\n\n");
    
    char caminhos[][50] = {
        "/home/user/documents/file.txt",
        "C:\\Windows\\System32\\driver.dll",
        "/usr/local/bin/program",
        "simple_file.txt"
    };
    
    for (int i = 0; i < 4; i++)
    {
        char *ultima_barra_unix = ft_strrchr(caminhos[i], '/');
        char *ultima_barra_win = ft_strrchr(caminhos[i], '\\');
        char *ultima_barra;
        
        // Escolhe a última barra encontrada
        if (ultima_barra_unix && ultima_barra_win)
            ultima_barra = (ultima_barra_unix > ultima_barra_win) ? 
                          ultima_barra_unix : ultima_barra_win;
        else
            ultima_barra = ultima_barra_unix ? ultima_barra_unix : ultima_barra_win;
        
        printf("Caminho: %s\n", caminhos[i]);
        if (ultima_barra)
        {
            *ultima_barra = '\0';  // Temporariamente
            printf("  Diretório: \"%s\"\n", caminhos[i]);
            printf("  Arquivo:   \"%s\"\n", ultima_barra + 1);
            *ultima_barra = (ultima_barra_unix > ultima_barra_win || 
                           !ultima_barra_win) ? '/' : '\\';  // Restaura
        }
        else
        {
            printf("  Arquivo local: \"%s\"\n", caminhos[i]);
        }
        printf("\n");
    }
}

void exemplo_parsing_reverso(void)
{
    printf("=== PARSING REVERSO ===\n\n");
    
    char emails[] = "user@domain.com";
    char *arroba = ft_strrchr(emails, '@');
    char *ultimo_ponto = ft_strrchr(emails, '.');
    
    printf("Email: \"%s\"\n", emails);
    
    if (arroba)
    {
        *arroba = '\0';
        printf("Usuário: \"%s\"\n", emails);
        *arroba = '@';
        
        if (ultimo_ponto && ultimo_ponto > arroba)
        {
            *ultimo_ponto = '\0';
            printf("Domínio: \"%s\"\n", arroba + 1);
            printf("TLD:     \"%s\"\n", ultimo_ponto + 1);
            *ultimo_ponto = '.';
        }
    }
    printf("\n");
}

void exemplo_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    // Teste 1: Buscar '\0'
    char *str = "Hello";
    char *terminator = ft_strrchr(str, '\0');
    printf("Buscar '\\0' em \"%s\":\n", str);
    printf("  Posição: %ld (comprimento da string)\n", terminator - str);
    printf("  Conteúdo: '%c' (null terminator)\n\n", *terminator);
    
    // Teste 2: Caractere único
    char *unico = "a";
    char *result = ft_strrchr(unico, 'a');
    printf("String com caractere único: \"%s\"\n", unico);
    printf("  strchr e strrchr retornam o mesmo: %s\n\n", 
           (strchr(unico, 'a') == result) ? "✅ Sim" : "❌ Não");
    
    // Teste 3: Todos iguais
    char *repetido = "aaaa";
    char *primeira_a = strchr(repetido, 'a');
    char *ultima_a = ft_strrchr(repetido, 'a');
    printf("String repetida: \"%s\"\n", repetido);
    printf("  Primeira 'a': posição %ld\n", primeira_a - repetido);
    printf("  Última 'a':   posição %ld\n", ultima_a - repetido);
}

int main(void)
{
    printf("🔍 DEMONSTRAÇÃO FT_STRRCHR\n");
    printf("===========================\n\n");
    
    demonstrar_diferenca_strchr_strrchr();
    exemplo_extensao_arquivo();
    exemplo_caminho_diretorio();
    exemplo_parsing_reverso();
    exemplo_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • strrchr = ÚLTIMA ocorrência\n");
    printf("   • Deve percorrer TODA a string\n");
    printf("   • Essencial para extensões e caminhos\n");
    printf("   • Também encontra '\\0' se buscar\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔍 DEMONSTRAÇÃO FT_STRRCHR
===========================

=== STRCHR vs STRRCHR ===

String: "programming"
strchr('r'):  posição 1 (primeira)
strrchr('r'): posição 4 (última)

p r o g r a m m i n g
0 1 2 3 4 5 6 7 8 9 10
  ^     ^              
  |     └── última 'r' 
  └── primeira 'r'     

=== BUSCA EXTENSÃO DE ARQUIVO ===

Arquivo: documento.txt      → Extensão: "txt"
Arquivo: backup.tar.gz      → Extensão: "gz"
Arquivo: arquivo.backup.zip → Extensão: "zip"
Arquivo: script.sh          → Extensão: "sh"
Arquivo: sem_extensao       → (sem extensão)

Arquivo: "arquivo.backup.zip"
Primeiro '.': "backup.zip" (extensão errada!)
Último '.':   "zip" (extensão correta!)

=== CAMINHO DE DIRETÓRIO ===

Caminho: /home/user/documents/file.txt
  Diretório: "/home/user/documents"
  Arquivo:   "file.txt"

Caminho: C:\Windows\System32\driver.dll
  Diretório: "C:\Windows\System32"
  Arquivo:   "driver.dll"

Caminho: /usr/local/bin/program
  Diretório: "/usr/local/bin"
  Arquivo:   "program"

Caminho: simple_file.txt
  Arquivo local: "simple_file.txt"

=== PARSING REVERSO ===

Email: "user@domain.com"
Usuário: "user"
Domínio: "domain"
TLD:     "com"

=== CASOS ESPECIAIS ===

Buscar '\0' em "Hello":
  Posição: 5 (comprimento da string)
  Conteúdo: '' (null terminator)

String com caractere único: "a"
  strchr e strrchr retornam o mesmo: ✅ Sim

String repetida: "aaaa"
  Primeira 'a': posição 0
  Última 'a':   posição 3

💡 LEMBRE-SE:
   • strrchr = ÚLTIMA ocorrência
   • Deve percorrer TODA a string
   • Essencial para extensões e caminhos
   • Também encontra '\0' se buscar
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Diferença algoritmica:** strchr para na primeira, strrchr continua
2. **Variável de estado:** Como manter rastro da "última" ocorrência
3. **Percurso completo:** Por que não podemos otimizar parando cedo
4. **Casos especiais:** Tratamento do '\0' após o loop

### 🎯 Perguntas para Reflexão
- Por que não podemos buscar de trás para frente na string?
- Como manter eficiência mesmo percorrendo toda string?
- Qual a diferença de complexidade entre strchr e strrchr?
- Quando usar strrchr ao invés de strchr?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Variável Last (Recomendada)
```c
char *ft_strrchr(const char *s, int c)
{
    char ch = (char)c;
    char *last = NULL;
    
    while (*s)
    {
        if (*s == ch)
            last = (char *)s;
        s++;
    }
    if (ch == '\0')
        return ((char *)s);
    return (last);
}
```

### 💭 Abordagem 2: Busca Reversa (Mais Complexa)
```c
char *ft_strrchr(const char *s, int c)
{
    char ch = (char)c;
    int  len = 0;
    
    // Calcula comprimento
    while (s[len])
        len++;
        
    // Se busca '\0'
    if (ch == '\0')
        return ((char *)&s[len]);
        
    // Busca de trás para frente
    while (len >= 0)
    {
        if (s[len] == ch)
            return ((char *)&s[len]);
        len--;
    }
    return (NULL);
}
```

### 💭 Abordagem 3: Recursiva (Educativa)
```c
char *ft_strrchr_helper(const char *s, int c, char *last)
{
    if (*s == '\0')
    {
        if ((char)c == '\0')
            return ((char *)s);
        return (last);
    }
    
    if (*s == (char)c)
        last = (char *)s;
        
    return (ft_strrchr_helper(s + 1, c, last));
}

char *ft_strrchr(const char *s, int c)
{
    return (ft_strrchr_helper(s, c, NULL));
}
```

### 🔧 Dicas de Implementação
- **Variável last**: Método mais simples e eficiente
- **Inicializar NULL**: Para caso de não encontrar
- **Verificar '\0'**: Após o loop, não durante
- **Cast necessário**: `(char *)s` para remover const

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_strrchr com abordagem "last"
2. Teste diferença com strchr em strings simples
3. Verifique comportamento com '\0'

### 🥈 Nível Intermediário
1. Implemente versão com busca reversa
2. Crie função que conta distância entre primeira e última
3. Desenvolva extrator de extensão de arquivo

### 🥇 Nível Avançado
1. Implemente strrchr para múltiplos caracteres
2. Crie versão insensível a maiúsculas/minúsculas
3. Otimize para strings muito longas

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - String
- [`ft_strchr`](ft_strchr.md) - Busca primeira ocorrência
- [`ft_strlen`](ft_strlen.md) - Calcula comprimento da string
- [`ft_strnstr`](ft_strnstr.md) - Busca substring em string

### 🔄 Funções Complementares
- `strpbrk` - Busca qualquer caractere de um conjunto
- `strstr` - Busca substring
- `strcspn` - Conta até encontrar caractere

### 📝 Aplicações Práticas
- Parsing de caminhos de arquivo
- Extração de extensões
- Processamento de URLs
- Validação de formatos

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🔍 Busca em Strings](../../resources/string_searching.md)
- [📁 Processamento de Caminhos](../../resources/path_processing.md)
- [⏪ Algoritmos Reversos](../../resources/reverse_algorithms.md)

### 🔗 Referências Externas
- Manual do C: `man strrchr`
- [C Reference - String Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Diferença conceitual entre primeira e última ocorrência
- [ ] Necessidade de percorrer toda string mesmo encontrando
- [ ] Gerenciamento da variável "last" 
- [ ] Casos especiais com caractere único

**Descobertas importantes:**
- [ ] strrchr é essencial para extensões de arquivo
- [ ] Não dá para otimizar parando na primeira ocorrência
- [ ] Muito usado em parsing de caminhos
- [ ] Diferença sutil mas crucial com strchr

**Testes que fiz:**
- [ ] Strings com múltiplas ocorrências
- [ ] Comparação direta strchr vs strrchr
- [ ] Busca por '\0' (terminador)
- [ ] Casos com caractere único
- [ ] Extensões de arquivo reais

---
<div align="center">

[← Função Anterior: ft_strchr](ft_strchr.md) | [Próxima Função: ft_strncmp →](ft_strncmp.md)

🔍 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
