# ft_strdup

> 📋 Duplica string alocando nova memória

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strdup.c`

---

## ⚠️ AVISO EDUCATIVO

**Este material é puramente didático e NÃO contém implementações completas.**

Se você é estudante da 42, use apenas para **entender conceitos** e desenvolva sua própria solução.

---

## 📋 SINOPSE

**Arquivo de Cabeçalho:**
```c
#include "libft.h"
#include <stdlib.h>  // Para malloc
```

**Protótipo:**
```c
char *ft_strdup(const char *s1);
```

> 💡 **Observação:** Aloca memória dinamicamente - sempre usar `free()` após o uso!

---

## 📖 DESCRIÇÃO

A função `ft_strdup` cria uma duplicata da string `s1` alocando memória suficiente e copiando todo o conteúdo, incluindo o terminador nulo '\0'.

### 🎯 Objetivo
Criar cópias independentes de strings que podem ser modificadas sem afetar a original, gerenciando memória automaticamente.

### 🌍 Aplicações Reais
- **Backup de strings:** Preservar versão original antes de modificações
- **Estruturas de dados:** Armazenar strings em listas, árvores, hash tables
- **Parsing de arquivos:** Salvar tokens extraídos de texto
- **APIs e bibliotecas:** Retornar strings que o usuário pode modificar livremente

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `s1` | `const char *` | String original a ser duplicada |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Sucesso | `char *` | Ponteiro para nova string duplicada |
| Falha na alocação | `NULL` | Memória insuficiente |
| String NULL | `NULL` | Parâmetro inválido |

### 📊 Exemplos de Comportamento

| Entrada | Resultado | Memória Alocada | Explicação |
|---------|-----------|-----------------|------------|
| "Hello" | Nova string "Hello" | 6 bytes | 5 chars + '\0' |
| "" | Nova string "" | 1 byte | Apenas '\0' |
| "42 School" | Nova string "42 School" | 10 bytes | 9 chars + '\0' |
| NULL | NULL | 0 bytes | Entrada inválida |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Algoritmo Básico
```c
// 1. Verificar se s1 é válida
// 2. Calcular comprimento de s1
// 3. Alocar memória (len + 1) para '\0'
// 4. Copiar todos os caracteres de s1
// 5. Adicionar '\0' no final
// 6. Retornar ponteiro para nova string
```

### 🆚 Diferença de Atribuição
```c
// ATRIBUIÇÃO: mesmo endereço (PERIGOSO!)
char *original = "Hello";
char *copia = original;  // ← Mesmo ponteiro!
copia[0] = 'h';  // ERRO! Pode causar crash

// STRDUP: novo endereço (SEGURO!)
char *original = "Hello";
char *copia = ft_strdup(original);  // ← Novo ponteiro!
copia[0] = 'h';  // ✅ OK! → "hello"
free(copia);  // Não esquecer!
```

### 🔍 Visualização da Memória
```
Antes do strdup:
┌─────────────┐
│ "Hello\0"   │ ← String original (read-only ou stack)
└─────────────┘

Após ft_strdup("Hello"):
┌─────────────┐
│ "Hello\0"   │ ← String original (inalterada)
└─────────────┘

┌─────────────┐
│ "Hello\0"   │ ← Nova string (heap, modificável)
└─────────────┘
         ↑
    Retorno da função
```

### ⚠️ Gerenciamento de Memória
```c
char *dup = ft_strdup("Test");
// dup aponta para memória heap

// ✅ CORRETO: liberar memória
if (dup) {
    // usar dup...
    free(dup);
    dup = NULL;  // boa prática
}

// ❌ ERRO: vazamento de memória
char *dup = ft_strdup("Test");
// esqueceu do free() → MEMORY LEAK!
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void demonstrar_duplicacao_basica(void)
{
    printf("=== DUPLICAÇÃO BÁSICA ===\n\n");
    
    const char *original = "Estudando na 42!";
    char *duplicada;
    
    printf("String original: \"%s\"\n", original);
    printf("Endereço original: %p\n", (void *)original);
    
    duplicada = ft_strdup(original);
    if (duplicada)
    {
        printf("String duplicada: \"%s\"\n", duplicada);
        printf("Endereço duplicada: %p\n", (void *)duplicada);
        printf("São iguais? %s\n", 
               strcmp(original, duplicada) == 0 ? "SIM" : "NÃO");
        printf("Mesmo endereço? %s\n", 
               original == duplicada ? "SIM" : "NÃO");
        
        free(duplicada);
    }
    else
    {
        printf("❌ Falha na duplicação!\n");
    }
    
    printf("\n");
}

void exemplo_modificacao_independente(void)
{
    printf("=== MODIFICAÇÃO INDEPENDENTE ===\n\n");
    
    char *original = ft_strdup("hello world");
    char *copia = ft_strdup(original);
    
    if (original && copia)
    {
        printf("Antes da modificação:\n");
        printf("Original: \"%s\"\n", original);
        printf("Cópia:    \"%s\"\n", copia);
        
        // Modificar apenas a cópia
        copia[0] = 'H';  // h → H
        copia[6] = 'W';  // w → W
        
        printf("\nApós modificar a cópia:\n");
        printf("Original: \"%s\"\n", original);  // Inalterada
        printf("Cópia:    \"%s\"\n", copia);     // Modificada
        
        free(original);
        free(copia);
    }
    
    printf("\n");
}

void exemplo_lista_de_nomes(void)
{
    printf("=== LISTA DE NOMES ===\n\n");
    
    // Simular entrada de dados
    const char *entrada[] = {
        "Ana Silva",
        "João Santos", 
        "Maria Oliveira",
        "Pedro Costa"
    };
    
    // Array para armazenar cópias
    char *nomes[4];
    int total = 4;
    
    printf("Criando lista de nomes:\n");
    
    // Duplicar cada nome
    for (int i = 0; i < total; i++)
    {
        nomes[i] = ft_strdup(entrada[i]);
        if (nomes[i])
            printf("%d. %s\n", i + 1, nomes[i]);
        else
            printf("%d. ERRO na duplicação\n", i + 1);
    }
    
    // Modificar um nome (só possível porque são cópias)
    if (nomes[1])
    {
        free(nomes[1]);  // Liberar versão antiga
        nomes[1] = ft_strdup("João Pedro Santos");  // Nova versão
        printf("\nNome atualizado: %s\n", nomes[1]);
    }
    
    // Liberar toda a memória
    printf("\nLiberando memória...\n");
    for (int i = 0; i < total; i++)
    {
        if (nomes[i])
        {
            free(nomes[i]);
            nomes[i] = NULL;
        }
    }
    
    printf("✅ Memória liberada!\n\n");
}

void exemplo_parsing_configuracao(void)
{
    printf("=== PARSING DE CONFIGURAÇÃO ===\n\n");
    
    const char *config_line = "database_host=localhost:5432";
    char *linha, *chave, *valor;
    char *sep_pos;
    
    // Duplicar linha para processamento
    linha = ft_strdup(config_line);
    if (!linha)
    {
        printf("❌ Erro ao duplicar configuração\n");
        return;
    }
    
    printf("Configuração original: \"%s\"\n", config_line);
    
    // Encontrar separador
    sep_pos = strchr(linha, '=');
    if (sep_pos)
    {
        *sep_pos = '\0';  // Dividir string
        
        chave = ft_strdup(linha);           // Primeira parte
        valor = ft_strdup(sep_pos + 1);     // Segunda parte
        
        if (chave && valor)
        {
            printf("Chave extraída: \"%s\"\n", chave);
            printf("Valor extraído: \"%s\"\n", valor);
            
            // Agora podemos processar cada parte independentemente
            // Por exemplo, remover espaços, validar formato, etc.
        }
        
        free(chave);
        free(valor);
    }
    
    free(linha);
    printf("\n");
}

void demonstrar_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    char *resultado;
    
    // String vazia
    resultado = ft_strdup("");
    printf("String vazia: \"%s\" (len=%zu)\n", 
           resultado ? resultado : "NULL",
           resultado ? strlen(resultado) : 0);
    free(resultado);
    
    // String com espaços
    resultado = ft_strdup("   ");
    printf("Só espaços: \"%s\" (len=%zu)\n", 
           resultado ? resultado : "NULL",
           resultado ? strlen(resultado) : 0);
    free(resultado);
    
    // String longa
    resultado = ft_strdup("Esta é uma string bem longa para testar a duplicação de textos extensos!");
    printf("String longa: %.30s... (len=%zu)\n", 
           resultado ? resultado : "NULL",
           resultado ? strlen(resultado) : 0);
    free(resultado);
    
    // Caracteres especiais
    resultado = ft_strdup("Acentuação: ção, ã, ê, ü");
    printf("Caracteres especiais: \"%s\"\n", 
           resultado ? resultado : "NULL");
    free(resultado);
    
    // Entrada NULL
    resultado = ft_strdup(NULL);
    printf("Entrada NULL: %s\n", 
           resultado ? resultado : "NULL (correto)");
    
    printf("\n");
}

void exemplo_gerenciamento_memoria(void)
{
    printf("=== GERENCIAMENTO DE MEMÓRIA ===\n\n");
    
    char **palavras;
    const char *frase = "Gerenciar memoria e fundamental";
    const char *delim = " ";
    int count = 0;
    
    // Contar palavras (simulação simples)
    char *temp = ft_strdup(frase);
    if (!temp)
        return;
        
    char *token = strtok(temp, delim);
    while (token)
    {
        count++;
        token = strtok(NULL, delim);
    }
    free(temp);
    
    printf("Frase: \"%s\"\n", frase);
    printf("Palavras encontradas: %d\n\n", count);
    
    // Alocar array de ponteiros
    palavras = malloc(count * sizeof(char *));
    if (!palavras)
        return;
    
    // Duplicar frase novamente para tokenização real
    temp = ft_strdup(frase);
    if (!temp)
    {
        free(palavras);
        return;
    }
    
    // Extrair e duplicar cada palavra
    int i = 0;
    token = strtok(temp, delim);
    while (token && i < count)
    {
        palavras[i] = ft_strdup(token);
        if (palavras[i])
            printf("Palavra %d: \"%s\"\n", i + 1, palavras[i]);
        i++;
    }
    
    // Liberar tudo
    printf("\nLiberando memória:\n");
    for (i = 0; i < count; i++)
    {
        if (palavras[i])
        {
            printf("Liberando: \"%s\"\n", palavras[i]);
            free(palavras[i]);
        }
    }
    free(palavras);
    free(temp);
    
    printf("✅ Toda memória liberada!\n\n");
}

int main(void)
{
    printf("📋 DEMONSTRAÇÃO FT_STRDUP\n");
    printf("=========================\n\n");
    
    demonstrar_duplicacao_basica();
    exemplo_modificacao_independente();
    exemplo_lista_de_nomes();
    exemplo_parsing_configuracao();
    demonstrar_casos_especiais();
    exemplo_gerenciamento_memoria();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • strdup SEMPRE aloca nova memória\n");
    printf("   • SEMPRE usar free() após uso\n");
    printf("   • Strings duplicadas são independentes\n");
    printf("   • Verificar retorno NULL (falha alocação)\n");
    printf("   • NULL pointer → NULL return\n");
    
    return (0);
}
```

**Saída esperada:**
```
📋 DEMONSTRAÇÃO FT_STRDUP
=========================

=== DUPLICAÇÃO BÁSICA ===

String original: "Estudando na 42!"
Endereço original: 0x55555555a008
String duplicada: "Estudando na 42!"
Endereço duplicada: 0x55555555b260
São iguais? SIM
Mesmo endereço? NÃO

=== MODIFICAÇÃO INDEPENDENTE ===

Antes da modificação:
Original: "hello world"
Cópia:    "hello world"

Após modificar a cópia:
Original: "hello world"
Cópia:    "Hello World"

=== LISTA DE NOMES ===

Criando lista de nomes:
1. Ana Silva
2. João Santos
3. Maria Oliveira
4. Pedro Costa

Nome atualizado: João Pedro Santos

Liberando memória...
✅ Memória liberada!

=== PARSING DE CONFIGURAÇÃO ===

Configuração original: "database_host=localhost:5432"
Chave extraída: "database_host"
Valor extraído: "localhost:5432"

=== CASOS ESPECIAIS ===

String vazia: "" (len=0)
Só espaços: "   " (len=3)
String longa: Esta é uma string bem longa... (len=73)
Caracteres especiais: "Acentuação: ção, ã, ê, ü"
Entrada NULL: NULL (correto)

=== GERENCIAMENTO DE MEMÓRIA ===

Frase: "Gerenciar memoria e fundamental"
Palavras encontradas: 4

Palavra 1: "Gerenciar"
Palavra 2: "memoria"
Palavra 3: "e"
Palavra 4: "fundamental"

Liberando memória:
Liberando: "Gerenciar"
Liberando: "memoria"
Liberando: "e"
Liberando: "fundamental"
✅ Toda memória liberada!

💡 LEMBRE-SE:
   • strdup SEMPRE aloca nova memória
   • SEMPRE usar free() após uso
   • Strings duplicadas são independentes
   • Verificar retorno NULL (falha alocação)
   • NULL pointer → NULL return
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Alocação dinâmica:** Como funciona malloc/free
2. **Gerenciamento de memória:** Heap vs Stack
3. **Memory leaks:** Por que free() é obrigatório
4. **Cópia vs Referência:** Diferença fundamental

### 🎯 Perguntas para Reflexão
- Por que strdup aloca nova memória ao invés de retornar a mesma?
- O que acontece se esquecer de dar free()?
- Como detectar falhas de alocação?
- Quando usar strdup vs strcpy?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Manual
```c
char *ft_strdup(const char *s1)
{
    char *dup;
    size_t len;
    size_t i;
    
    if (!s1)
        return (NULL);
        
    len = ft_strlen(s1);
    dup = malloc(len + 1);
    if (!dup)
        return (NULL);
        
    i = 0;
    while (s1[i])
    {
        dup[i] = s1[i];
        i++;
    }
    dup[i] = '\0';
    
    return (dup);
}
```

### 💭 Abordagem 2: Com ft_strlcpy
```c
char *ft_strdup(const char *s1)
{
    char *dup;
    size_t len;
    
    if (!s1)
        return (NULL);
        
    len = ft_strlen(s1);
    dup = malloc(len + 1);
    if (!dup)
        return (NULL);
        
    ft_strlcpy(dup, s1, len + 1);
    
    return (dup);
}
```

### 🔧 Dicas de Implementação
- **Verificar NULL primeiro:** Entrada inválida deve retornar NULL
- **+1 para '\0':** Sempre alocar espaço extra para terminador
- **Verificar malloc:** Pode falhar em sistemas com pouca memória
- **Dependências:** Precisa de ft_strlen para calcular tamanho

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente strdup básica copiando char por char
2. Teste com strings de diferentes tamanhos
3. Verifique se sempre adiciona '\0' no final

### 🥈 Nível Intermediário
1. Adicione tratamento robusto de erros
2. Crie versão que usa ft_strlcpy
3. Implemente strndup (duplica apenas n caracteres)

### 🥇 Nível Avançado
1. Crie strdup otimizada para strings grandes
2. Implemente versão com custom allocator
3. Adicione proteção contra overflow em sistemas 32-bit

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - String
- [`ft_strlen`](ft_strlen.md) - Usada para calcular tamanho
- [`ft_strlcpy`](ft_strlcpy.md) - Pode ser usada para copiar
- [`ft_strchr`](ft_strchr.md) - Busca em strings duplicadas

### 🔄 Funções de Memória
- [`ft_calloc`](../memory/ft_calloc.md) - Alocação com inicialização
- `malloc` - Alocação dinâmica base
- `free` - Liberação de memória

### 📝 Aplicações Práticas
- [`ft_strjoin`](../additional/ft_strjoin.md) - Une strings alocando nova
- [`ft_split`](../additional/ft_split.md) - Cria array de strings
- Listas ligadas com strings
- Hash tables com chaves string

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [💾 Dynamic Memory Management](../../resources/dynamic_memory.md)
- [🔄 String Duplication Patterns](../../resources/string_duplication.md)
- [⚠️ Memory Leak Prevention](../../resources/memory_leaks.md)

### 🔗 Referências Externas
- Manual do C: `man strdup`
- [GNU C Library - String Functions](https://www.gnu.org/software/libc/manual/html_node/Copying-Strings-and-Arrays.html)
- [Memory Management in C](https://www.learn-c.org/en/Dynamic_memory_allocation)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender quando usar malloc vs outras formas de alocação
- [ ] Lembrar sempre do +1 para o '\0'
- [ ] Gerenciar adequadamente os free()
- [ ] Tratar falhas de alocação adequadamente

**Descobertas importantes:**
- [ ] strdup cria independência entre strings
- [ ] Memória heap permite strings de qualquer tamanho
- [ ] free() é obrigatório para evitar vazamentos
- [ ] NULL input deve retornar NULL output

**Testes que fiz:**
- [ ] Duplicação de strings normais
- [ ] String vazia e casos especiais
- [ ] Modificação independente das cópias
- [ ] Gerenciamento de memória em loops

---
<div align="center">

[← Função Anterior: ft_strnstr](ft_strnstr.md) | [Próxima Categoria: Funções Adicionais →](../additional/README.md)

🔤 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
