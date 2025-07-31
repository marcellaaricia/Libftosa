# ft_memset

> 🧱 Preenche um bloco de memória com um valor específico

---

**Categoria:** [Funções de Memória](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_memset.c`

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
void *ft_memset(void *dest, int c, size_t len);
```

> 💡 **Observação:** Trabalha com memória bruta (bytes), não strings terminadas em `\0`.

---

## 📖 DESCRIÇÃO

A função `ft_memset` preenche uma região de memória com um valor específico, byte a byte. É uma das funções mais fundamentais para manipulação de memória.

### 🎯 Objetivo
Definir os `len` primeiros bytes de uma área de memória com o mesmo valor

### 🌍 Aplicações Reais
- **Inicialização:** Zerar arrays ou estruturas
- **Limpeza:** Zerar dados antes do uso
- **Segurança:** Limpar senhas da memória

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `dest` | `void *` | Ponteiro para área de memória a ser preenchida |
| `c`| `int` | Valor a ser usado (convertido para unsigned char) |
| `len` | `size_t` | Número de bytes a serem preenchidos |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| Sempre | `dest` | Ponteiro para o início da área preenchida |

### 📊 Exemplos de Comportamento

| Operação | Resultado | Explicação |
|----------|-----------|------------|
| `memset(arr, 0, 10)` | Zeros nos 10 primeiros bytes | Inicialização comum |
| `memset(arr, 'A', 5)` | `"AAAAA"` nos primeiros 5 bytes | Preenchimento com caractere |
| `memset(arr, 255, 3)` | Bytes com valor 255 | Preenchimento com valor específico |
| `memset(arr, 300, 1)` | Byte com valor 44 | 300 % 256 = 44 (conversão) |

---

## 💡 COMO ENTENDER O CONCEITO

### 🧠 Lógica fundamental
```
Para cada byte na região `dest` até `len`:
	byte = c
```

### 🔄 Processo Passo a Passo
```
1. Converter dest para unsigned char* (trabalhar byte a byte)
2. Converter c para unsigned char (apenas 8 bits válidos)
3. Para i de 0 até len-1:
   - dest[i] = c
4. Retornar ponteiro original
```
### ⚠️ Pontos Importantes
- **Conversão de tipo:** `int c` vira `unsigned char` (0-255)
- **Retorno útil:** Permite operações encadeadas

---

## 🧪 EXEMPLO DE USO

```c
#include "libft.h"
#include <stdio.h>
#include <string.h>

void imprimir_memoria(unsigned char *ptr, size_t len, const char *titulo)
{
    printf("%s: ", titulo);
    for (size_t i = 0; i < len; i++)
        printf("%3d ", ptr[i]);
    printf("\n");
}

int main(void)
{
    unsigned char buffer[10];
    
    printf("=== DEMONSTRAÇÃO ft_memset ===\n\n");
    
    // Estado inicial (lixo de memória)
    printf("🗑️  Estado inicial (valores aleatórios):\n");
    imprimir_memoria(buffer, 10, "Buffer");
    
    // Zeramento completo
    ft_memset(buffer, 0, 10);
    printf("\n🧹 Após ft_memset(buffer, 0, 10):\n");
    imprimir_memoria(buffer, 10, "Buffer");
    
    // Preenchimento parcial com 'A'
    ft_memset(buffer, 'A', 5);
    printf("\n🎨 Após ft_memset(buffer, 'A', 5):\n");
    imprimir_memoria(buffer, 10, "Buffer");
    printf("     Como string: \"%.10s\"\n", buffer);
    
    // Preenchimento com valor alto
    ft_memset(buffer + 5, 255, 3);
    printf("\n⚡ Após ft_memset(buffer+5, 255, 3):\n");
    imprimir_memoria(buffer, 10, "Buffer");
    
    // Demonstração de conversão de tipo
    printf("\n🔄 Demonstração de conversão:\n");
    ft_memset(buffer, 300, 2);  // 300 % 256 = 44
    printf("    ft_memset(buffer, 300, 2) → valor real: %d\n", buffer[0]);
    
    // Uso prático: inicializar array de inteiros
    printf("\n📊 Uso prático - Array de inteiros:\n");
    int numeros[5] = {1, 2, 3, 4, 5};
    printf("    Antes: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", numeros[i]);
    
    ft_memset(numeros, 0, sizeof(numeros));
    printf("\n    Após memset: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", numeros[i]);
    printf("\n");
    
    return (0);
}
```

---

## 🔧 CONCEITOS TÉCNICOS

### 💾 Por que `void *`?
```c
// Flexibilidade para qualquer tipo
int    *ints = malloc(sizeof(int) * 10);
char   *chars = malloc(100);
double *doubles = malloc(sizeof(double) * 5);

// Todos podem usar memset
ft_memset(ints, 0, sizeof(int) * 10);
ft_memset(chars, 'X', 100);
ft_memset(doubles, 0, sizeof(double) * 5);
```

### 🎭 Conversão de `int c`
```c
// Por que int e não char?
// Histórico: compatibilidade com EOF (-1)
// Na prática: apenas os 8 bits menos significativos são usados

int valor = 300;           // 100101100 (binário)
unsigned char real = 300;  // 01001100 (44 decimal)
```

### ⚠️ Cuidados Importantes
- **Tamanho correto:** Use `sizeof()` para arrays locais
- **Não funciona com:** Arrays de ponteiros para strings
- **Alinhamento:** Funciona byte a byte, sem otimizações de palavra

---

## 🆚 COMPARAÇÃO COM ALTERNATIVAS

| Método | Vantagem | Desvantagem |
|--------|----------|-------------|
| `ft_memset` | Rápido, padrão | Só um valor por vez |
| Loop manual | Flexível | Mais código, propenso a erros |
| `ft_bzero` | Específico para zeros | Só zera |
| `calloc` | Aloca + zera | Só para alocação |

---

<div align="center">

**[← Função Anterior: ft_bzero](ft_bzero.md)** | **[Próxima Função: ft_memcpy →](ft_memcpy.md)**

🧠 [Funções de Memória](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>