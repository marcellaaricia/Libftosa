# ft_isprint

> 🖨️ Verifica se um caractere é imprimível (printável)

---

**Categoria:** [Funções de Caractere](./README.md)

**Repositório:** [Libftosa](../../README.md)

**Arquivo:** `ft_isprint.c`

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
int ft_isprint(int c);
```

> 💡 **Observação:** O parâmetro é `int` por compatibilidade, mas a lógica se aplica a caracteres (`char`).

---

## 📖 DESCRIÇÃO

A função `ft_isprint` verifica se um caractere é imprimível — ou seja, se ele pode aparecer visualmente em tela.

### 🎯 Objetivo
Identificar se o caractere de entrada está no intervalo:
- **ASCII 32 a 126**, que inclui letras, números, pontuações e símbolos

### 🌍 Aplicações Reais
- **Renderização de texto:** Evitar caracteres de controle
- **Validação de entrada:** Bloquear caracteres invisíveis ou especiais
- **Sanitização:** Filtrar apenas caracteres visíveis

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Caractere a ser analisado (valor ASCII) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| É imprimível (ASCII 32–126) | `≠ 0` | Verdadeiro |
| Não é imprimível | `0` | Falso |

### 📊 Exemplos de Comportamento

| Entrada | ASCII | Retorno | Explicação |
|---------|-------|---------|------------|
| `'A'` | 65 | `≠ 0` | Letra visível |
| `' '` | 32 | `≠ 0` | Espaço (é imprimível!) |
| `'~'` | 126 | `≠ 0` | Último caractere imprimível |
| `'
'` | 10 | `0` | Caractere de controle |
| `127` | 127 | `0` | DEL (não imprimível) |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔢 Fundamentos da Tabela ASCII
```
Intervalo dos imprimíveis:
ASCII 32 (espaço) até 126 ('~')

Fora disso: controle ou caracteres não visíveis
```

### 🧠 Lógica Conceitual
```
Se o caractere está entre 32 e 126 (inclusive):
    é imprimível
Senão:
    não é imprimível
```

### ⚡ Expressão Lógica Simples
```c
return (c >= 32 && c <= 126);
```

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"
#include <stdio.h>

int main(void)
{
    char exemplos[] = {'A', 'z', '5', '0', '@', ' ', '!', '\n', 127, '~'};
    int i = 0;

    printf("=== TESTANDO CONCEITO ===\n");
    printf("Char | ASCII | ft_isprint | Resultado\n");
    printf("-----|-------|------------|----------\n");

    while (i < 10)
    {
        char c = exemplos[i];
        int resultado = ft_isprint(c);

        if (c == ' ')
            printf("'⎵'  |");
        else if (c == '\n')
            printf("'\\n' |");
        else if (c == 127)
            printf("'DEL'|");
        else
            printf("'%c'  |", c);

        printf("  %3d  |     %d      | %s\n", 
               c, 
               resultado ? 1 : 0,
               resultado ? "✅ Imprimível" : "❌ Não imprimível");

        i++;
    }

    return (0);
}
```

---

<div align="center">

**[← Função Anterior: ft_isascii](ft_isascii.md)** | **[Próxima Função: ft_toupper →](ft_toupper.md)**



🔤 [Funções de Caractere](./README.md) | [📚 Voltar ao Índice](../../README.md)

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
