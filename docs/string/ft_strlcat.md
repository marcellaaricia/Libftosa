# ft_strlcat

> 🔗 Concatena strings de forma segura (sem buffer overflow)

---

**Categoria:** [Funções de String](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_strlcat.c`

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
size_t ft_strlcat(char *dst, const char *src, size_t dstsize);
```

> 💡 **Observação:** Sempre garante terminação nula, mesmo em buffers pequenos.

---

## 📖 DESCRIÇÃO

A função `ft_strlcat` concatena a string `src` ao final da string `dst`, garantindo que o resultado não exceda `dstsize - 1` caracteres, sempre terminando com '\0'.

### 🎯 Objetivo
Fornecer concatenação segura de strings, prevenindo **buffer overflow** e garantindo sempre strings válidas.

### 🌍 Aplicações Reais
- **Construção de paths:** Juntar diretórios e arquivos
- **Mensagens dinâmicas:** Construir mensagens personalizadas
- **URLs e queries:** Montar URLs com parâmetros
- **Logs seguros:** Concatenar informações sem overflow

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `dst` | `char *` | String de destino (será modificada) |
| `src` | `const char *` | String de origem (será copiada) |
| `dstsize` | `size_t` | Tamanho total do buffer de destino |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Descrição |
|----------|---------|-----------|
| Sucesso total | `strlen(dst_inicial) + strlen(src)` | Comprimento que resultaria |
| Buffer insuficiente | `dstsize + strlen(src)` | Comprimento que tentou criar |

### 📊 Exemplos de Comportamento

| dst inicial | src | dstsize | Resultado dst | Retorno | Explicação |
|-------------|-----|---------|---------------|---------|------------|
| "Hello" | " World" | 20 | "Hello World" | 11 | Concatenação completa |
| "Hello" | " World" | 10 | "Hello Wo" | 11 | Truncado, mas seguro |
| "Hello" | " World" | 5 | "Hello" | 13 | Buffer muito pequeno |
| "" | "World" | 10 | "World" | 5 | Destino vazio |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧮 Algoritmo Básico
```c
// 1. Encontrar final de dst
// 2. Verificar se há espaço disponível
// 3. Copiar src até limite ou fim de src
// 4. Garantir terminação nula
// 5. Retornar tamanho que resultaria
```

### 🆚 Diferença do strcat
```c
// strcat: PERIGOSO - pode causar overflow
char dest[10] = "Hello";
strcat(dest, " World!!!"); // BOOM! Overflow!

// strlcat: SEGURO - limita o tamanho
char dest[10] = "Hello";
ft_strlcat(dest, " World!!!", 10); // "Hello Wor\0" - Seguro!
```

### 🔍 Casos Especiais
```c
char buf[10] = "Hello";

// Buffer exatamente no limite
ft_strlcat(buf, " World", 12); // "Hello World\0"

// Buffer insuficiente
ft_strlcat(buf, " World", 8); // "Hello W\0"

// dst já maior que dstsize
char full[5] = "Hello"; // dst já tem 5 chars
ft_strlcat(full, "X", 3); // Retorna 3 + 1 = 4
```

### 📏 Cálculo do Retorno
```
Se dst_len < dstsize:
    retorno = dst_len + src_len

Se dst_len >= dstsize:
    retorno = dstsize + src_len
    
💡 Isso permite detectar se houve truncamento:
   se (retorno >= dstsize) então houve truncamento
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>
#include <string.h>

void demonstrar_concatenacao_segura(void)
{
    printf("=== CONCATENAÇÃO SEGURA ===\n\n");
    
    char buffer[20];
    size_t resultado;
    
    // Exemplo 1: Concatenação normal
    strcpy(buffer, "Hello");
    printf("Inicial: \"%s\" (len=%zu)\n", buffer, strlen(buffer));
    
    resultado = ft_strlcat(buffer, " World", sizeof(buffer));
    printf("Após \" World\": \"%s\" (retorno=%zu)\n", buffer, resultado);
    
    if (resultado < sizeof(buffer))
        printf("✅ Concatenação completa!\n");
    else
        printf("⚠️  Houve truncamento!\n");
    
    printf("\n");
}

void exemplo_truncamento(void)
{
    printf("=== EXEMPLO DE TRUNCAMENTO ===\n\n");
    
    char pequeno[10];
    size_t resultado;
    
    strcpy(pequeno, "Hello");
    printf("Buffer pequeno [%zu]: \"%s\"\n", sizeof(pequeno), pequeno);
    
    resultado = ft_strlcat(pequeno, " Beautiful World", sizeof(pequeno));
    printf("Tentando adicionar \" Beautiful World\"...\n");
    printf("Resultado: \"%s\" (retorno=%zu)\n", pequeno, resultado);
    
    if (resultado >= sizeof(pequeno))
        printf("⚠️  Truncamento detectado! Precisaria de %zu bytes\n", resultado + 1);
    
    printf("\n");
}

void exemplo_construcao_path(void)
{
    printf("=== CONSTRUÇÃO DE PATH ===\n\n");
    
    char path[256];
    const char *base = "/home/user";
    const char *dirs[] = {"/documents", "/projects", "/42", "/libft", NULL};
    size_t resultado;
    int i = 0;
    
    // Inicializar path
    strcpy(path, base);
    printf("Base: \"%s\"\n", path);
    
    // Adicionar diretórios
    while (dirs[i])
    {
        resultado = ft_strlcat(path, dirs[i], sizeof(path));
        printf("+ \"%s\" → \"%s\"\n", dirs[i], path);
        
        if (resultado >= sizeof(path))
        {
            printf("❌ Path muito longo! Truncado.\n");
            break;
        }
        i++;
    }
    
    printf("Path final: \"%s\"\n\n", path);
}

void exemplo_mensagens_dinamicas(void)
{
    printf("=== MENSAGENS DINÂMICAS ===\n\n");
    
    char msg[100];
    char nome[] = "João";
    char idade[] = "25";
    char cidade[] = "São Paulo";
    size_t resultado;
    
    // Construir mensagem passo a passo
    strcpy(msg, "Olá, ");
    printf("1. \"%s\"\n", msg);
    
    resultado = ft_strlcat(msg, nome, sizeof(msg));
    printf("2. \"%s\" (ret=%zu)\n", msg, resultado);
    
    resultado = ft_strlcat(msg, "! Você tem ", sizeof(msg));
    printf("3. \"%s\" (ret=%zu)\n", msg, resultado);
    
    resultado = ft_strlcat(msg, idade, sizeof(msg));
    printf("4. \"%s\" (ret=%zu)\n", msg, resultado);
    
    resultado = ft_strlcat(msg, " anos e mora em ", sizeof(msg));
    printf("5. \"%s\" (ret=%zu)\n", msg, resultado);
    
    resultado = ft_strlcat(msg, cidade, sizeof(msg));
    printf("6. \"%s\" (ret=%zu)\n", msg, resultado);
    
    if (resultado < sizeof(msg))
        printf("✅ Mensagem completa!\n");
    
    printf("\n");
}

void demonstrar_casos_especiais(void)
{
    printf("=== CASOS ESPECIAIS ===\n\n");
    
    char buf[10];
    size_t resultado;
    
    // Caso 1: Destino vazio
    buf[0] = '\0';
    resultado = ft_strlcat(buf, "Hello", sizeof(buf));
    printf("Destino vazio + \"Hello\": \"%s\" (ret=%zu)\n", buf, resultado);
    
    // Caso 2: Fonte vazia
    strcpy(buf, "Hello");
    resultado = ft_strlcat(buf, "", sizeof(buf));
    printf("\"Hello\" + vazio: \"%s\" (ret=%zu)\n", buf, resultado);
    
    // Caso 3: Buffer size = 0
    strcpy(buf, "Test");
    resultado = ft_strlcat(buf, "X", 0);
    printf("dstsize=0: \"%s\" (ret=%zu)\n", buf, resultado);
    
    // Caso 4: dst já maior que dstsize
    strcpy(buf, "LongString");
    resultado = ft_strlcat(buf, "X", 5);
    printf("dst > dstsize: \"%s\" (ret=%zu)\n", buf, resultado);
    
    printf("\n");
}

int main(void)
{
    printf("🔗 DEMONSTRAÇÃO FT_STRLCAT\n");
    printf("==========================\n\n");
    
    demonstrar_concatenacao_segura();
    exemplo_truncamento();
    exemplo_construcao_path();
    exemplo_mensagens_dinamicas();
    demonstrar_casos_especiais();
    
    printf("💡 LEMBRE-SE:\n");
    printf("   • strlcat SEMPRE termina com '\\0'\n");
    printf("   • Retorno >= dstsize indica truncamento\n");
    printf("   • dstsize deve incluir espaço para '\\0'\n");
    printf("   • Mais seguro que strcat/strncat\n");
    
    return (0);
}
```

**Saída esperada:**
```
🔗 DEMONSTRAÇÃO FT_STRLCAT
==========================

=== CONCATENAÇÃO SEGURA ===

Inicial: "Hello" (len=5)
Após " World": "Hello World" (retorno=11)
✅ Concatenação completa!

=== EXEMPLO DE TRUNCAMENTO ===

Buffer pequeno [10]: "Hello"
Tentando adicionar " Beautiful World"...
Resultado: "Hello Bea" (retorno=21)
⚠️  Truncamento detectado! Precisaria de 22 bytes

=== CONSTRUÇÃO DE PATH ===

Base: "/home/user"
+ "/documents" → "/home/user/documents"
+ "/projects" → "/home/user/documents/projects"
+ "/42" → "/home/user/documents/projects/42"
+ "/libft" → "/home/user/documents/projects/42/libft"
Path final: "/home/user/documents/projects/42/libft"

=== MENSAGENS DINÂMICAS ===

1. "Olá, "
2. "Olá, João" (ret=9)
3. "Olá, João! Você tem " (ret=21)
4. "Olá, João! Você tem 25" (ret=23)
5. "Olá, João! Você tem 25 anos e mora em " (ret=39)
6. "Olá, João! Você tem 25 anos e mora em São Paulo" (ret=49)
✅ Mensagem completa!

=== CASOS ESPECIAIS ===

Destino vazio + "Hello": "Hello" (ret=5)
"Hello" + vazio: "Hello" (ret=5)
dstsize=0: "Test" (ret=1)
dst > dstsize: "LongString" (ret=6)

💡 LEMBRE-SE:
   • strlcat SEMPRE termina com '\0'
   • Retorno >= dstsize indica truncamento
   • dstsize deve incluir espaço para '\0'
   • Mais seguro que strcat/strncat
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Buffer overflow:** Por que funções inseguras são perigosas
2. **Terminação nula:** Importância do caractere '\0'
3. **Medição de strings:** Como ft_strlen funciona
4. **Limites de buffer:** Diferença entre tamanho e capacidade

### 🎯 Perguntas para Reflexão
- Por que strlcat é mais segura que strcat?
- Como detectar se houve truncamento?
- O que acontece se dst não estiver terminada com nul?
- Por que retornar o tamanho que resultaria?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Passo a Passo
```c
size_t ft_strlcat(char *dst, const char *src, size_t dstsize)
{
    size_t dst_len;
    size_t src_len;
    size_t i;
    
    // 1. Medir strings
    dst_len = ft_strlen(dst);
    src_len = ft_strlen(src);
    
    // 2. Verificar se dst já está no limite
    if (dst_len >= dstsize)
        return (dstsize + src_len);
    
    // 3. Copiar src até o limite
    i = 0;
    while (src[i] && (dst_len + i) < (dstsize - 1))
    {
        dst[dst_len + i] = src[i];
        i++;
    }
    
    // 4. Garantir terminação nula
    dst[dst_len + i] = '\0';
    
    // 5. Retornar tamanho total que resultaria
    return (dst_len + src_len);
}
```

### 💭 Abordagem 2: Com Ponteiros
```c
size_t ft_strlcat(char *dst, const char *src, size_t dstsize)
{
    char *d;
    const char *s;
    size_t n;
    size_t dst_len;
    
    d = dst;
    s = src;
    n = dstsize;
    
    // Encontrar final de dst
    while (n-- && *d)
        d++;
        
    dst_len = d - dst;
    n = dstsize - dst_len;
    
    if (n == 0)
        return (dst_len + ft_strlen(s));
        
    while (*s)
    {
        if (n > 1)
        {
            *d++ = *s;
            n--;
        }
        s++;
    }
    *d = '\0';
    
    return (dst_len + (s - src));
}
```

### 🔧 Dicas de Implementação
- **Medição first:** Sempre meça dst_len antes de modificar
- **Limite correto:** Reserve 1 posição para '\0'
- **Casos especiais:** Tratar dstsize = 0 ou dst >= dstsize
- **Dependências:** Use ft_strlen para medições

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente strlcat básica seguindo o algoritmo passo-a-passo
2. Teste com strings simples e diferentes tamanhos de buffer
3. Verifique se sempre termina com '\0'

### 🥈 Nível Intermediário
1. Adicione tratamento para todos os casos especiais
2. Otimize para reduzir chamadas de função
3. Compare performance com strcat/strncat

### 🥇 Nível Avançado
1. Implemente versão que funciona com strings não-terminadas
2. Crie strlcat para wide characters (wchar_t)
3. Adicione suporte a encoding UTF-8

---

## 🔗 FUNÇÕES RELACIONADAS

### 🧠 Mesma Categoria - String
- [`ft_strlen`](ft_strlen.md) - Usada para medir strings
- [`ft_strlcpy`](ft_strlcpy.md) - Cópia segura de strings
- [`ft_strncmp`](ft_strncmp.md) - Comparação limitada de strings

### 🔄 Funções de Concatenação
- `strcat` - Concatenação insegura (não use!)
- `strncat` - Concatenação limitada (complicada)
- `sprintf` - Formatação com concatenação

### 📝 Aplicações Práticas
- [`ft_strjoin`](../additional/ft_strjoin.md) - Junta strings alocando nova
- [`ft_split`](../additional/ft_split.md) - Divide strings em arrays

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [🛡️ Buffer Overflow Prevention](../../resources/buffer_security.md)
- [📏 String Length Management](../../resources/string_lengths.md)
- [🔗 Safe String Operations](../../resources/safe_strings.md)

### 🔗 Referências Externas
- Manual do C: `man strlcat`
- [OpenBSD strlcat](https://man.openbsd.org/strlcat.3)
- [Secure Programming - String Functions](https://wiki.sei.cmu.edu/confluence/display/c/STR07-C)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado

**Dificuldades encontradas:**
- [ ] Entender a diferença entre tamanho do buffer e comprimento da string
- [ ] Implementar a lógica de retorno correta
- [ ] Lidar com casos onde dst >= dstsize
- [ ] Garantir que sempre termina com '\0'

**Descobertas importantes:**
- [ ] strlcat é muito mais segura que strcat
- [ ] O valor de retorno permite detectar truncamento
- [ ] dstsize deve incluir espaço para o '\0'
- [ ] Casos especiais são fundamentais para robustez

**Testes que fiz:**
- [ ] Concatenação normal (buffer suficiente)
- [ ] Truncamento (buffer insuficiente)
- [ ] Casos especiais (dstsize=0, strings vazias)
- [ ] Strings já no limite do buffer

---
<div align="center">

[← Função Anterior: ft_strlcpy](ft_strlcpy.md) | [Próxima Função: ft_strnstr →](ft_strnstr.md)

🔤 [Funções de String](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
