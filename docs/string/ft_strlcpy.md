# ft_strlcpy

> 📋 Copia string de forma segura com garantia de terminação nula

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strlcpy.c`

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
size_t ft_strlcpy(char *dst, const char *src, size_t dstsize);
```

> 💡 **Observação:** Sempre garante terminação nula, diferente de strncpy.

---

## 📖 DESCRIÇÃO

A função `ft_strlcpy` copia a string `src` para `dst` de forma segura, garantindo que `dst` sempre termine com `\0` e nunca exceda `dstsize` bytes.

### 🎯 Objetivo
Fornecer uma alternativa segura ao strcpy e strncpy, evitando buffer overflows e strings não terminadas.

### 🌍 Aplicações Reais
- **Validação de entrada:** Copiar input do usuário com limite
- **Buffers de rede:** Copiar dados recebidos com segurança
- **Configurações:** Ler arquivos config sem riscos
- **APIs seguras:** Substituir strcpy em bibliotecas

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `dst` | `char *` | Buffer de destino onde será copiada a string |
| `src` | `const char *` | String de origem a ser copiada |
| `dstsize` | `size_t` | Tamanho total do buffer de destino |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Sempre | `size_t` | Comprimento da string `src` (strlen(src)) |

> 🔍 **Importante:** O retorno é sempre o comprimento de `src`, independente de quanto foi copiado!

### 📊 Exemplos de Comportamento

| src | dstsize | dst após | Retorno | Explicação |
|-----|---------|----------|---------|------------|
| `"hello"` | 10 | `"hello"` | 5 | Copia completa |
| `"hello world"` | 6 | `"hello"` | 11 | Trunca, mas termina com \0 |
| `"test"` | 1 | `""` | 4 | Só consegue colocar \0 |
| `"abc"` | 0 | (inalterado) | 3 | dstsize=0, nada é copiado |

---

## 💡 COMO ENTENDER O CONCEITO

### 🆚 Diferenças das Outras Funções
```c
// strcpy: PERIGOSO - sem limite!
strcpy(dst, src);  // Pode estourar buffer!

// strncpy: pode não terminar com \0
strncpy(dst, src, n);  // Se src >= n, não adiciona \0!

// strlcpy: SEGURA - sempre termina com \0
strlcpy(dst, src, dstsize);  // Sempre \0, nunca estoura!
```

### 🔄 Como strlcpy Funciona
```
src = "hello world"  (comprimento = 11)
dst = [? ? ? ? ? ?]  (dstsize = 6)

Processo:
1. Copia até (dstsize-1) = 5 caracteres
2. Adiciona \0 na posição 5
3. Retorna strlen(src) = 11

Resultado:
dst = ['h' 'e' 'l' 'l' 'o' '\0']
retorno = 11

Se retorno > dstsize-1: houve truncamento!
```

### 🧮 Detectando Truncamento
```c
char buffer[10];
size_t result = ft_strlcpy(buffer, source, sizeof(buffer));

if (result >= sizeof(buffer))
    printf("AVISO: String foi truncada!\n");
    
// Ou de forma mais direta:
if (result >= 10)  // sizeof(buffer)
    printf("String original tinha %zu chars, só couberam %zu\n", 
           result, sizeof(buffer) - 1);
```

### ⚠️ Casos Especiais
```c
// dstsize = 0: não copia nada
ft_strlcpy(dst, "hello", 0);  // dst inalterado, retorno = 5

// dstsize = 1: só consegue colocar \0
ft_strlcpy(dst, "hello", 1);  // dst = "", retorno = 5

// src vazia
ft_strlcpy(dst, "", 10);      // dst = "", retorno = 0

// src muito longa
ft_strlcpy(dst, "very long string", 5);  // dst = "very", retorno = 16
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

void exemplo_basico(void)
{
    printf("=== EXEMPLO BÁSICO ===\n\n");
    
    char buffer[20];
    size_t result;
    
    // Cópia normal
    result = ft_strlcpy(buffer, "Hello, World!", sizeof(buffer));
    printf("Copiando \"Hello, World!\" para buffer[20]\n");
    printf("Resultado: \"%s\"\n", buffer);
    printf("Retorno: %zu (comprimento da string original)\n\n", result);
    
    // Demonstra que retorno != strlen(dst)
    printf("strlen(buffer) = %zu\n", strlen(buffer));
    printf("ft_strlcpy retornou = %zu\n", result);
    printf("São iguais porque a cópia foi completa!\n\n");
}

void exemplo_truncamento(void)
{
    printf("=== EXEMPLO DE TRUNCAMENTO ===\n\n");
    
    char small_buffer[8];
    char *long_string = "This is a very long string!";
    
    size_t result = ft_strlcpy(small_buffer, long_string, sizeof(small_buffer));
    
    printf("String original: \"%s\" (len=%zu)\n", long_string, strlen(long_string));
    printf("Buffer size: %zu\n", sizeof(small_buffer));
    printf("Resultado: \"%s\"\n", small_buffer);
    printf("Retorno: %zu\n\n", result);
    
    // Detecta truncamento
    if (result >= sizeof(small_buffer)) {
        printf("⚠️ TRUNCAMENTO DETECTADO!\n");
        printf("   String original: %zu caracteres\n", result);
        printf("   Espaço disponível: %zu caracteres\n", sizeof(small_buffer) - 1);
        printf("   Caracteres perdidos: %zu\n\n", result - (sizeof(small_buffer) - 1));
    }
}

void exemplo_validacao_entrada(void)
{
    printf("=== VALIDAÇÃO DE ENTRADA DO USUÁRIO ===\n\n");
    
    char username[16];  // Máximo 15 chars + \0
    char *inputs[] = {
        "john",
        "superlongusername123456789",
        "alice_42",
        "",
        NULL
    };
    
    for (int i = 0; inputs[i]; i++)
    {
        printf("Input: \"%s\"\n", inputs[i]);
        
        size_t result = ft_strlcpy(username, inputs[i], sizeof(username));
        
        if (result == 0) {
            printf("❌ Username vazio!\n");
        } else if (result >= sizeof(username)) {
            printf("⚠️ Username muito longo! Truncado para: \"%s\"\n", username);
        } else {
            printf("✅ Username válido: \"%s\"\n", username);
        }
        printf("\n");
    }
}

void exemplo_configuracao_segura(void)
{
    printf("=== LEITURA SEGURA DE CONFIGURAÇÃO ===\n\n");
    
    // Simula leitura de arquivo de configuração
    char *config_lines[] = {
        "server_name=my_awesome_server",
        "port=8080",
        "database_path=/very/long/path/to/database/file/that/exceeds/buffer/limits",
        "debug=true",
        NULL
    };
    
    char server_name[20];
    char port[8];
    char db_path[30];
    char debug[8];
    
    for (int i = 0; config_lines[i]; i++)
    {
        char *line = config_lines[i];
        printf("Processando: %s\n", line);
        
        if (strncmp(line, "server_name=", 12) == 0) {
            size_t result = ft_strlcpy(server_name, line + 12, sizeof(server_name));
            printf("  server_name: \"%s\"%s\n", server_name,
                   result >= sizeof(server_name) ? " (TRUNCADO)" : "");
        }
        else if (strncmp(line, "port=", 5) == 0) {
            size_t result = ft_strlcpy(port, line + 5, sizeof(port));
            printf("  port: \"%s\"%s\n", port,
                   result >= sizeof(port) ? " (TRUNCADO)" : "");
        }
        else if (strncmp(line, "database_path=", 14) == 0) {
            size_t result = ft_strlcpy(db_path, line + 14, sizeof(db_path));
            printf("  database_path: \"%s\"%s\n", db_path,
                   result >= sizeof(db_path) ? " (TRUNCADO)" : "");
        }
        else if (strncmp(line, "debug=", 6) == 0) {
            size_t result = ft_strlcpy(debug, line + 6, sizeof(debug));
            printf("  debug: \"%s\"%s\n", debug,
                   result >= sizeof(debug) ? " (TRUNCADO)" : "");
        }
        printf("\n");
    }
}

void exemplo_comparacao_funcoes(void)
{
    printf("=== COMPARAÇÃO: strcpy vs strncpy vs strlcpy ===\n\n");
    
    char dst1[8], dst2[8], dst3[8];
    char *src = "hello world";
    
    printf("String original: \"%s\" (len=%zu)\n", src, strlen(src));
    printf("Buffers de tamanho: 8\n\n");
    
    // strcpy: PERIGOSO!
    printf("strcpy(dst, src):\n");
    printf("  ❌ NÃO USAR! Pode estourar buffer!\n\n");
    
    // strncpy: pode não terminar com \0
    memset(dst2, 'X', sizeof(dst2));  // Simula lixo
    strncpy(dst2, src, sizeof(dst2));
    printf("strncpy(dst, src, 8):\n");
    printf("  Resultado: \"");
    for (size_t i = 0; i < sizeof(dst2); i++) {
        if (dst2[i] == '\0') printf("\\0");
        else printf("%c", dst2[i]);
    }
    printf("\"\n");
    printf("  ⚠️ Não termina com \\0! strlen() pode crashar!\n\n");
    
    // strlcpy: SEGURA!
    memset(dst3, 'X', sizeof(dst3));  // Simula lixo
    size_t result = ft_strlcpy(dst3, src, sizeof(dst3));
    printf("ft_strlcpy(dst, src, 8):\n");
    printf("  Resultado: \"%s\"\n", dst3);
    printf("  Retorno: %zu\n", result);
    printf("  ✅ Sempre termina com \\0!\n");
    printf("  ✅ Nunca estoura buffer!\n");
    printf("  ✅ Informa se houve truncamento!\n\n");
}

void exemplo_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    char dst[10];
    size_t result;
    
    // dstsize = 0
    strcpy(dst, "untouched");
    result = ft_strlcpy(dst, "hello", 0);
    printf("ft_strlcpy(dst, \"hello\", 0):\n");
    printf("  dst: \"%s\" (inalterado)\n", dst);
    printf("  retorno: %zu\n\n", result);
    
    // dstsize = 1
    result = ft_strlcpy(dst, "hello", 1);
    printf("ft_strlcpy(dst, \"hello\", 1):\n");
    printf("  dst: \"%s\" (só \\0)\n", dst);
    printf("  retorno: %zu\n\n", result);
    
    // String vazia
    result = ft_strlcpy(dst, "", sizeof(dst));
    printf("ft_strlcpy(dst, \"\", 10):\n");
    printf("  dst: \"%s\"\n", dst);
    printf("  retorno: %zu\n\n", result);
}

int main(void)
{
    printf("🧪 DEMONSTRAÇÃO FT_STRLCPY\n");
    printf("==========================\n\n");
    
    exemplo_basico();
    exemplo_truncamento();
    exemplo_validacao_entrada();
    exemplo_configuracao_segura();
    exemplo_comparacao_funcoes();
    exemplo_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • Sempre garante terminação com \\0\n");
    printf("   • Nunca escreve além de dstsize\n");
    printf("   • Retorna strlen(src) para detectar truncamento\n");
    printf("   • Use sizeof(buffer) como dstsize\n");
    printf("   • Prefira sempre sobre strcpy/strncpy\n");
    
    return (0);
}
```

**Saída esperada:**
```
🧪 DEMONSTRAÇÃO FT_STRLCPY
==========================

=== EXEMPLO BÁSICO ===

Copiando "Hello, World!" para buffer[20]
Resultado: "Hello, World!"
Retorno: 13 (comprimento da string original)

strlen(buffer) = 13
ft_strlcpy retornou = 13
São iguais porque a cópia foi completa!

=== EXEMPLO DE TRUNCAMENTO ===

String original: "This is a very long string!" (len=27)
Buffer size: 8
Resultado: "This is"
Retorno: 27

⚠️ TRUNCAMENTO DETECTADO!
   String original: 27 caracteres
   Espaço disponível: 7 caracteres
   Caracteres perdidos: 20

=== VALIDAÇÃO DE ENTRADA DO USUÁRIO ===

Input: "john"
✅ Username válido: "john"

Input: "superlongusername123456789"
⚠️ Username muito longo! Truncado para: "superlonguserna"

Input: "alice_42"
✅ Username válido: "alice_42"

Input: ""
❌ Username vazio!
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Buffer overflow:** Por que strcpy é perigoso
2. **Null termination:** Importância do '\0' em strings C
3. **Size vs length:** Diferença entre tamanho do buffer e comprimento da string
4. **Truncamento:** Como detectar se dados foram perdidos

### 🎯 Perguntas para Reflexão
- Por que retornar strlen(src) em vez de strlen(dst)?
- Como detectar se houve truncamento?
- Por que reservar 1 byte do dstsize para '\0'?
- Qual a diferença entre dstsize=0 e dstsize=1?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Clássica com Índices
```c
size_t ft_strlcpy(char *dst, const char *src, size_t dstsize)
{
    size_t src_len;
    size_t i;
    
    // Calcula comprimento de src
    src_len = 0;
    while (src[src_len])
        src_len++;
    
    // Se não há espaço, apenas retorna src_len
    if (dstsize == 0)
        return (src_len);
    
    // Copia até (dstsize - 1) caracteres
    i = 0;
    while (i < (dstsize - 1) && src[i])
    {
        dst[i] = src[i];
        i++;
    }
    
    // Sempre termina com \0
    dst[i] = '\0';
    
    return (src_len);
}
```

### 💭 Abordagem 2: Com Função Auxiliar
```c
size_t ft_strlcpy(char *dst, const char *src, size_t dstsize)
{
    size_t src_len = ft_strlen(src);
    size_t copy_len;
    
    if (dstsize == 0)
        return (src_len);
    
    // Determina quantos caracteres copiar
    copy_len = (src_len < dstsize - 1) ? src_len : dstsize - 1;
    
    // Copia os caracteres
    ft_memcpy(dst, src, copy_len);
    dst[copy_len] = '\0';
    
    return (src_len);
}
```

### 💭 Abordagem 3: Otimizada com Ponteiros
```c
size_t ft_strlcpy(char *dst, const char *src, size_t dstsize)
{
    const char *src_start = src;
    
    // Conta src_len enquanto copia (se possível)
    if (dstsize > 0)
    {
        while (--dstsize && *src)
            *dst++ = *src++;
        *dst = '\0';
    }
    
    // Termina de contar src se necessário
    while (*src++)
        ;
        
    return (src - src_start - 1);
}
```

### 🔧 Dicas de Implementação
- **Calcular src_len primeiro:** Sempre retornar strlen(src)
- **Verificar dstsize=0:** Caso especial importante
- **Reservar espaço para '\0':** Copiar no máximo (dstsize-1)
- **Sempre terminar com '\0':** Garantia de segurança

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente ft_strlcpy básica
2. Teste com diferentes tamanhos de buffer
3. Compare comportamento com strncpy

### 🥈 Nível Intermediário
1. Implemente detectando truncamento automaticamente
2. Crie wrapper que avisa sobre truncamento
3. Teste todos os casos especiais

### 🥇 Nível Avançado
1. Implemente ft_strlcat usando mesmo princípio
2. Crie versão que funciona com strings wide (wchar_t)
3. Otimize para processar múltiplos bytes por vez

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - Cópia/Concatenação
- [`ft_strlcat`](ft_strlcat.md) - Concatena strings de forma segura
- [`ft_strdup`](ft_strdup.md) - Duplica string alocando memória
- `strcpy` - Cópia insegura (evitar)
- `strncpy` - Cópia limitada mas pode não terminar com '\0'

### 🔄 Funções de Comparação
- [`ft_strncmp`](ft_strncmp.md) - Compara strings limitadamente
- [`ft_strlen`](ft_strlen.md) - Calcula comprimento (usado internamente)

### 📝 Funções de Memória
- [`ft_memcpy`](../memory/ft_memcpy.md) - Pode ser usada na implementação
- [`ft_memmove`](../memory/ft_memmove.md) - Cópia segura de memória

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🛡️ Programação Segura](../../resources/secure_programming.md)
- [📏 Buffer Management](../../resources/buffer_management.md)
- [🔍 String Safety](../../resources/string_safety.md)

### 🔗 Referências Externas
- Manual do C: `man strlcpy`
- [OpenBSD strlcpy](https://man.openbsd.org/strlcpy.3)
- [Secure Programming - String Handling](https://wiki.sei.cmu.edu/confluence/pages/viewpage.action?pageId=87152177)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Por que retornar strlen(src) e não strlen(dst)
- [ ] Diferença entre tamanho do buffer e espaço disponível
- [ ] Como detectar truncamento corretamente
- [ ] Casos especiais (dstsize=0, dstsize=1)

**Descobertas importantes:**
- [ ] strlcpy resolve problemas de strcpy E strncpy
- [ ] Valor de retorno é chave para detectar problemas
- [ ] Sempre garante string válida (terminada com '\0')
- [ ] Fundamental para programação segura

**Testes que fiz:**
- [ ] Diferentes tamanhos de buffer
- [ ] Strings vazias e muito longas
- [ ] Casos de truncamento
- [ ] Comparação com funções inseguras

---
<div align="center">

[← Função Anterior: ft_strncmp](ft_strncmp.md) | [Próxima Função: ft_strlcat →](ft_strlcat.md)

🔤 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
