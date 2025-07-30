# ft_isalnum

> 🔤🔢 Verifica se um caractere é alfanumérico (letra ou dígito)

**Categoria:** [Funções de Caractere](../../README.md#-funções-de-caractere)  
**Repositório:** [Libftosa](../../README.md)
**Arquivo:** `ft_isalnum.c`

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
int ft_isalnum(int c);
```

> 💡 **Observação:** Combina a funcionalidade de `ft_isalpha` e `ft_isdigit`.

---

## 📖 DESCRIÇÃO

A função `ft_isalnum` determina se um caractere é alfanumérico, ou seja, se é uma letra (A-Z, a-z) OU um dígito (0-9).

### 🎯 Objetivo
Identificar se o caractere de entrada pertence a QUALQUER um dos conjuntos:
- **Letras maiúsculas:** A, B, C, ..., Z (ASCII 65-90)
- **Letras minúsculas:** a, b, c, ..., z (ASCII 97-122)
- **Dígitos decimais:** 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 (ASCII 48-57)

### 🌍 Aplicações Reais
- **Validação de usernames:** Aceitar apenas letras e números
- **Parsing de identificadores:** Validar nomes de variáveis
- **Formulários web:** Campos que só aceitam alfanuméricos
- **Senhas básicas:** Validação de caracteres permitidos
- **Códigos de produto:** SKUs, códigos de barras

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Caractere a ser analisado (valor ASCII) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| É alfanumérico | `≠ 0` | Verdadeiro (valor não-zero) |
| Não é alfanumérico | `0` | Falso |

### 📊 Exemplos de Comportamento

| Entrada | ASCII | Retorno | Explicação |
|---------|-------|---------|------------|
| `'A'` | 65 | `≠ 0` | Letra maiúscula |
| `'z'` | 122 | `≠ 0` | Letra minúscula |
| `'5'` | 53 | `≠ 0` | Dígito |
| `'0'` | 48 | `≠ 0` | Dígito zero |
| `'@'` | 64 | `0` | Símbolo especial |
| `' '` | 32 | `0` | Espaço em branco |
| `'!'` | 33 | `0` | Pontuação |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔢 Fundamentos da Tabela ASCII
```
Intervalos aceitos:
'A'-'Z': 65-90   (26 caracteres)
'a'-'z': 97-122  (26 caracteres)  
'0'-'9': 48-57   (10 caracteres)

Total: 62 caracteres alfanuméricos
```

### 🧠 Lógica Conceitual
```
Para ser alfanumérico, um caractere deve estar em:
QUALQUER UM dos três intervalos:
- [65-90]  OU  [97-122]  OU  [48-57]

Implementação: usar operador OU (||) entre as condições
```

### ⚡ Elegância da Implementação
```c
// Versão direta (3 comparações)
return ((c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z') || (c >= '0' && c <= '9'));

// Versão modular (reutiliza funções)
return (ft_isalpha(c) || ft_isdigit(c));
```

**A segunda é mais elegante e mantível!** ✨

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"  // Ou <ctype.h> para isalnum original
#include <stdio.h>

int main(void)
{
    char exemplos[] = {'A', 'z', '5', '0', '@', ' ', '!', '\n'};
    int i = 0;
    
    printf("=== TESTANDO CONCEITO ===\n");
    printf("Char | ASCII | ft_isalnum | Resultado\n");
    printf("-----|-------|------------|----------\n");
    
    while (i < 8)
    {
        char c = exemplos[i];
        int resultado = ft_isalnum(c);  // Sua implementação
        
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
               resultado ? "✅ É alfanumérico" : "❌ Não é alfanumérico");
        
        i++;
    }
    
    return (0);
}
```

**Saída esperada:**
```
=== TESTANDO CONCEITO ===
Char | ASCII | ft_isalnum | Resultado
-----|-------|------------|----------
'A'  |   65  |     1      | ✅ É alfanumérico
'z'  |  122  |     1      | ✅ É alfanumérico
'5'  |   53  |     1      | ✅ É alfanumérico
'0'  |   48  |     1      | ✅ É alfanumérico
'@'  |   64  |     0      | ❌ Não é alfanumérico
'⎵'  |   32  |     0      | ❌ Não é alfanumérico
'!'  |   33  |     0      | ❌ Não é alfanumérico
'\n' |   10  |     0      | ❌ Não é alfanumérico
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **Reutilização de código:** Como usar funções já criadas
2. **Operador OR (||):** Como combinar múltiplas condições
3. **Modularidade:** Vantagens de quebrar problemas em partes menores
4. **Manutenibilidade:** Por que a versão modular é melhor

### 🎯 Perguntas para Reflexão
- Por que `ft_isalpha(c) || ft_isdigit(c)` é melhor que escrever tudo de novo?
- O que acontece se você corrigir um bug em ft_isalpha?
- Como o operador || funciona (short-circuit evaluation)?
- Qual versão é mais fácil de entender para outro programador?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Implementação Direta
```
Se (c é maiúscula) OU (c é minúscula) OU (c é dígito):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 2: Implementação Modular (Recomendada)
```
Se ft_isalpha(c) OU ft_isdigit(c):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 3: Expressão Booleana
```
resultado = ft_isalpha(c) || ft_isdigit(c)
retorna resultado
```

### 🔧 Dicas de Implementação
- **Prefira reutilização** ao invés de reescrever
- **Use operador ||** para combinar condições
- **Mantenha consistência** com funções relacionadas  
- **Pense na manutenibilidade** do código

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente `ft_isalnum` das duas formas (direta e modular)
2. Compare performance das duas versões
3. Teste com diferentes tipos de caracteres
4. Entenda por que a versão modular é preferível

### 🥈 Nível Intermediário
1. Crie um validador de username (só alfanuméricos)
2. Implemente um contador de caracteres alfanuméricos em string
3. Crie uma função que remove caracteres não-alfanuméricos
4. Desenvolva um gerador de senhas alfanuméricas

### 🥇 Nível Avançado
1. Implemente um parser que separa alfanuméricos de símbolos
2. Crie um validador de código de produto (formato específico)
3. Desenvolva um sistema de limpeza de dados de entrada
4. Implemente um analisador léxico básico usando ft_isalnum

---

## 🔗 FUNÇÕES RELACIONADAS

### 🔤 Funções Base (Dependências)
- [`ft_isalpha`](ft_isalpha.md) - Verifica se é letra (A-Z, a-z)
- [`ft_isdigit`](ft_isdigit.md) - Verifica se é dígito (0-9)

### 🔤 Mesma Categoria
- [`ft_isascii`](ft_isascii.md) - Verifica se é caractere ASCII (0-127)
- [`ft_isprint`](ft_isprint.md) - Verifica se é caractere imprimível
- [`ft_toupper`](ft_toupper.md) - Converte para maiúscula
- [`ft_tolower`](ft_tolower.md) - Converte para minúscula

---

## 🧮 CURIOSIDADES TÉCNICAS

### 🔢 Estatísticas ASCII
```
Total de caracteres ASCII: 128 (0-127)
Caracteres alfanuméricos: 62 (48%)
- Maiúsculas: 26 caracteres
- Minúsculas: 26 caracteres  
- Dígitos: 10 caracteres
```

### 📊 Comparação de Performance
```
Versão direta:    3 comparações sempre
Versão modular:   1-2 comparações (short-circuit)

Em ft_isalpha(c) || ft_isdigit(c):
- Se for letra: só executa ft_isalpha
- Se não for letra: executa ambas as funções
```

### 🎯 Vantagens da Modularidade
- **Manutenção:** Corrige em um lugar, funciona em todos
- **Legibilidade:** Código mais claro e expressivo
- **Testabilidade:** Cada função pode ser testada isoladamente
- **Reutilização:** Não repete código desnecessário

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [📊 Tabela ASCII Completa](../../resources/ascii_table.md)
- [🧠 Operadores Lógicos em C](../../resources/logical_operators.md)
- [🔧 Modularidade em Programação](../../resources/modularity.md)
- [⚡ Short-circuit Evaluation](../../resources/short_circuit.md)

### 🔗 Referências Externas
- Manual do C: `man isalnum`
- [ASCII Table Reference](https://www.asciitable.com/)
- [C Reference - Character Functions](https://en.cppreference.com/w/c/string/byte)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado
> **Documenta aqui suas descobertas, dificuldades e insights durante a implementação!**

**Dificuldades encontradas:**
- [ ] Decidir entre implementação direta vs modular
- [ ] Entender o conceito de "alfanumérico"
- [ ] Lembrar de todos os intervalos ASCII
- [ ] Usar corretamente o operador ||

**Descobertas importantes:**
- [ ] Vantagens da reutilização de código
- [ ] Como o short-circuit funciona na prática
- [ ] Por que modularidade facilita manutenção
- [ ] Relação entre ft_isalpha, ft_isdigit e ft_isalnum

**Testes que fiz:**
- [ ] Teste com todas as letras (A-Z, a-z)
- [ ] Teste com todos os dígitos (0-9)
- [ ] Teste com símbolos especiais
- [ ] Teste com caracteres de controle

**Reflexões:**
- [ ] Qual implementação escolhi e por quê?
- [ ] Como posso aplicar modularidade em outras funções?
- [ ] Que outros caracteres são considerados "válidos" em diferentes contextos?

---

**🔤 [← Funções de Caractere](../../README.md#-funções-de-caractere)** | **[📚 Voltar ao Índice](../../README.md)** | **[Anterior: ft_isdigit ←](ft_isdigit.md)** | **[Próxima: ft_isascii →](ft_isascii.md)**

---

<div align="center">

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
