# ft_isdigit

> 🔢 Verifica se um caractere é um dígito numérico

**Categoria:** [Funções de Caractere](../../README.md#-funções-de-caractere)  
**Repositório:** [Libftosa](../../README.md)
**Arquivo:** `ft_isdigit.c`

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
int ft_isdigit(int c);
```

> 💡 **Observação:** O parâmetro é `int` por compatibilidade, mas trabalha com caracteres.

---

## 📖 DESCRIÇÃO

A função `ft_isdigit` determina se um caractere representa um dígito decimal (0-9).

### 🎯 Objetivo
Identificar se o caractere de entrada pertence ao conjunto:
- **Dígitos decimais:** 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 (ASCII 48-57)

### 🌍 Aplicações Reais
- **Validação de entrada:** Verificar se usuário digitou apenas números
- **Parsing numérico:** Identificar dígitos em strings mistas
- **Calculadoras:** Validar entrada de operandos
- **Formulários:** Verificar campos como CEP, telefone, CPF

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Caractere a ser analisado (valor ASCII) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| É dígito | `≠ 0` | Verdadeiro (valor não-zero) |
| Não é dígito | `0` | Falso |

### 📊 Exemplos de Comportamento

| Entrada | ASCII | Retorno | Explicação |
|---------|-------|---------|------------|
| `'0'` | 48 | `≠ 0` | Dígito zero |
| `'5'` | 53 | `≠ 0` | Dígito cinco |
| `'9'` | 57 | `≠ 0` | Dígito nove |
| `'A'` | 65 | `0` | Letra maiúscula |
| `'a'` | 97 | `0` | Letra minúscula |
| `' '` | 32 | `0` | Espaço em branco |
| `'@'` | 64 | `0` | Símbolo especial |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔢 Fundamentos da Tabela ASCII
```
Valores dos dígitos:
'0' = 48, '1' = 49, '2' = 50, ..., '9' = 57

Padrão sequencial: cada dígito tem valor ASCII consecutivo
'1' - '0' = 49 - 48 = 1
'2' - '0' = 50 - 48 = 2
```

### 🧠 Lógica Conceitual
```
Para ser dígito, um caractere deve estar em:
Intervalo único [48-57] que corresponde a ['0'-'9']

Implementação: verificar UM único intervalo
```

### ⚡ Diferença do ft_isalpha
- **ft_isalpha:** Dois intervalos (maiúsculas E minúsculas)
- **ft_isdigit:** Um intervalo (apenas dígitos 0-9)
- **Simplicidade:** Menos comparações = mais eficiente

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"  // Ou <ctype.h> para isdigit original
#include <stdio.h>

int main(void)
{
    char exemplos[] = {'0', '5', '9', 'A', 'a', '@', ' ', '\n'};
    int i = 0;
    
    printf("=== TESTANDO CONCEITO ===\n");
    printf("Char | ASCII | ft_isdigit | Resultado\n");
    printf("-----|-------|------------|----------\n");
    
    while (i < 8)
    {
        char c = exemplos[i];
        int resultado = ft_isdigit(c);  // Sua implementação
        
        // Tratamento de caracteres especiais para exibição
        if (c == ' ')
            printf("'⎵'  |");
        else if (c == '\n')
            printf("'\\n' |");
        else
            printf("'%c'  |", c);
        
        printf("  %3d  |     %d      | %s\n", 
               c, 
               resultado ? 1 : 0,
               resultado ? "✅ É dígito" : "❌ Não é dígito");
        
        i++;
    }
    
    return (0);
}
```

**Saída esperada:**
```
=== TESTANDO CONCEITO ===
Char | ASCII | ft_isdigit | Resultado
-----|-------|------------|----------
'0'  |   48  |     1      | ✅ É dígito
'5'  |   53  |     1      | ✅ É dígito
'9'  |   57  |     1      | ✅ É dígito
'A'  |   65  |     0      | ❌ Não é dígito
'a'  |   97  |     0      | ❌ Não é dígito
'@'  |   64  |     0      | ❌ Não é dígito
'⎵'  |   32  |     0      | ❌ Não é dígito
'\n' |   10  |     0      | ❌ Não é dígito
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Intervalo ASCII:** Entenda que dígitos são sequenciais (48-57)
2. **Comparação simples:** Apenas um intervalo para verificar
3. **Eficiência:** Menos operações que ft_isalpha
4. **Conversão:** Diferença entre caractere dígito e valor numérico

### 🎯 Perguntas para Reflexão
- Por que '0' não é igual a 0 (número)?
- Como converter caractere dígito para valor numérico?
- Qual a diferença entre '5' e 5?
- Por que a verificação é mais simples que ft_isalpha?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Pensamento Lógico
```
Se (caractere está entre '0' e '9'):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 2: Matemática ASCII
```
Se (48 ≤ c ≤ 57):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 3: Expressão Booleana
```
resultado = (c >= '0') && (c <= '9')
retorna resultado
```

### 🔧 Dicas de Implementação
- Use comparação direta com caracteres (`'0'`, `'9'`)
- Uma única condição com operador `&&`
- Return direto da expressão booleana
- Mais simples que ft_isalpha (só um intervalo)

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente `ft_isdigit` seguindo os conceitos acima
2. Teste com diferentes caracteres
3. Compare com a função original `isdigit`
4. Entenda a diferença entre '5' e 5

### 🥈 Nível Intermediário
1. Crie uma função que conta dígitos em uma string
2. Implemente um validador de números (só dígitos)
3. Crie uma função que extrai apenas dígitos de uma string
4. Combine ft_isdigit com ft_isalpha para criar ft_isalnum

### 🥇 Nível Avançado
1. Implemente um parser que separa números de texto
2. Crie um validador de CPF/CNPJ usando ft_isdigit
3. Desenvolva uma calculadora simples que valida entrada
4. Implemente conversão de string para número usando ft_isdigit

---

## 🔗 FUNÇÕES RELACIONADAS

### 🔤 Mesma Categoria
- [`ft_isalpha`](ft_isalpha.md) - Verifica se é letra (A-Z, a-z)
- [`ft_isalnum`](ft_isalnum.md) - Verifica se é alfanumérico (letra OU dígito)
- [`ft_isascii`](ft_isascii.md) - Verifica se é caractere ASCII (0-127)
- [`ft_isprint`](ft_isprint.md) - Verifica se é caractere imprimível

### 🔄 Funções Complementares
- [`ft_atoi`](../additional/ft_atoi.md) - Converte string para inteiro
- [`ft_itoa`](../additional/ft_itoa.md) - Converte inteiro para string

---

## 🧮 CURIOSIDADES MATEMÁTICAS

### 🔢 Conversão Caractere → Número
```c
// Para converter '5' para 5:
char c = '5';
int numero = c - '0';  // 53 - 48 = 5
```

### 📊 Comparação de Eficiência
```
ft_isdigit:  1 comparação (c >= '0' && c <= '9')
ft_isalpha:  2 comparações ((c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z'))

ft_isdigit é mais eficiente! ⚡
```

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [📊 Tabela ASCII Completa](../../resources/ascii_table.md)
- [🧠 Operadores Lógicos em C](../../resources/logical_operators.md)
- [🔢 Conversões de Tipo](../../resources/type_conversion.md)

### 🔗 Referências Externas
- Manual do C: `man isdigit`
- [ASCII Table Reference](https://www.asciitable.com/)
- [C Reference - Character Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado
> **Documenta aqui suas descobertas, dificuldades e insights durante a implementação!**

**Dificuldades encontradas:**
- [ ] Entender diferença entre '5' e 5
- [ ] Lembrar valores ASCII dos dígitos
- [ ] Comparar com ft_isalpha (diferenças)
- [ ] Implementar sem condicionais extras

**Descobertas importantes:**
- [ ] Por que ft_isdigit é mais simples que ft_isalpha
- [ ] Relação entre caractere e valor numérico
- [ ] Importância da sequência ASCII dos dígitos
- [ ] Como usar para validação de entrada

**Testes que fiz:**
- [ ] Teste com todos os dígitos (0-9)
- [ ] Teste com letras
- [ ] Teste com símbolos
- [ ] Teste com caracteres especiais

---

**🔤 [← Funções de Caractere](../../README.md#-funções-de-caractere)** | **[📚 Voltar ao Índice](../../README.md)** | **[Anterior: ft_isalpha ←](ft_isalpha.md)** | **[Próxima: ft_isalnum →](ft_isalnum.md)**

---

<div align="center">

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
