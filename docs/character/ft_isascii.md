# ft_isascii

> 🔢 Verifica se um valor é um caractere ASCII válido

**Categoria:** [Funções de Caractere](../../README.md#-funções-de-caractere)  
**Repositório:** [Libftosa](../../README.md)
**Arquivo:** `ft_isascii.c`

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
int ft_isascii(int c);
```

> 💡 **Observação:** Diferente das outras funções, esta trabalha com **valores numéricos** diretamente, não caracteres.

---

## 📖 DESCRIÇÃO

A função `ft_isascii` determina se um valor está dentro do conjunto de caracteres ASCII válidos (0-127).

### 🎯 Objetivo
Identificar se o valor de entrada está no intervalo ASCII padrão:
- **Intervalo válido:** 0 a 127 (inclusive)
- **Total de caracteres:** 128 valores possíveis
- **Inclui:** Caracteres de controle, imprimíveis e DEL

### 🌍 Aplicações Reais
- **Validação de entrada:** Verificar se dados estão em ASCII puro
- **Limpeza de dados:** Filtrar caracteres estendidos
- **Compatibilidade:** Garantir compatibilidade com sistemas antigos
- **Encoding:** Verificar se texto é ASCII antes de conversões
- **Protocolos de rede:** Validar headers HTTP, emails

---

## 🎯 PARÂMETROS

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `c` | `int` | Valor a ser analisado (pode ser qualquer inteiro) |

---

## ↩️ VALOR DE RETORNO

| Condição | Retorno | Significado |
|----------|---------|-------------|
| É ASCII válido | `≠ 0` | Verdadeiro (valor entre 0-127) |
| Não é ASCII | `0` | Falso (valor fora do range) |

### 📊 Exemplos de Comportamento

| Entrada | Decimal | Retorno | Explicação |
|---------|---------|---------|------------|
| `'A'` | 65 | `≠ 0` | Letra maiúscula |
| `'a'` | 97 | `≠ 0` | Letra minúscula |
| `'0'` | 48 | `≠ 0` | Dígito |
| `'\n'` | 10 | `≠ 0` | Caractere de controle |
| `'\0'` | 0 | `≠ 0` | Null terminator |
| `127` | 127 | `≠ 0` | DEL (último ASCII) |
| `128` | 128 | `0` | Primeiro caractere estendido |
| `200` | 200 | `0` | Caractere estendido |
| `-1` | -1 | `0` | Valor negativo |

---

## 💡 COMO ENTENDER O CONCEITO

### 🔢 Fundamentos do ASCII
```
ASCII (American Standard Code for Information Interchange):
- Criado em 1963
- 7 bits = 2^7 = 128 caracteres possíveis
- Valores: 0 a 127 (decimal)

Distribuição:
0-31:   Caracteres de controle (não imprimíveis)
32:     Espaço
33-126: Caracteres imprimíveis
127:    DEL (delete)
```

### 🧠 Lógica Conceitual
```
Para ser ASCII válido, um valor deve estar em:
Intervalo único [0-127]

Implementação: verificar UM único intervalo
Mais simples que ft_isalpha (sem múltiplos ranges)
```

### ⚡ Diferença das Outras Funções
- **ft_isalpha/isdigit:** Trabalham com caracteres específicos
- **ft_isascii:** Trabalha com **qualquer valor** no range ASCII
- **Mais abrangente:** Inclui controle + imprimíveis + DEL
- **Base fundamental:** Outras funções assumem entrada ASCII

---

## 🧪 EXEMPLO DE USO EDUCATIVO

```c
#include "libft.h"  // Ou <ctype.h> para isascii original
#include <stdio.h>

int main(void)
{
    int exemplos[] = {0, 32, 65, 97, 127, 128, 200, -1, 255};
    int i = 0;
    
    printf("=== TESTANDO CONCEITO ===\n");
    printf("Valor | Char | ft_isascii | Resultado\n");
    printf("------|------|------------|----------\n");
    
    while (i < 9)
    {
        int c = exemplos[i];
        int resultado = ft_isascii(c);  // Sua implementação
        
        printf(" %3d  |", c);
        
        // Mostra caractere se for imprimível
        if (c >= 32 && c <= 126)
            printf("  '%c' |", c);
        else if (c == 0)
            printf(" '\\0' |");
        else if (c == 10)
            printf(" '\\n' |");
        else
            printf("  ?   |");
        
        printf("     %d      | %s\n", 
               resultado ? 1 : 0,
               resultado ? "✅ É ASCII" : "❌ Não é ASCII");
        
        i++;
    }
    
    return (0);
}
```

**Saída esperada:**
```
=== TESTANDO CONCEITO ===
Valor | Char | ft_isascii | Resultado
------|------|------------|----------
  0   | '\0' |     1      | ✅ É ASCII
 32   |  ' ' |     1      | ✅ É ASCII
 65   |  'A' |     1      | ✅ É ASCII
 97   |  'a' |     1      | ✅ É ASCII
127   |  ?   |     1      | ✅ É ASCII
128   |  ?   |     0      | ❌ Não é ASCII
200   |  ?   |     0      | ❌ Não é ASCII
 -1   |  ?   |     0      | ❌ Não é ASCII
255   |  ?   |     0      | ❌ Não é ASCII
```

---

## 📚 CONCEITOS PARA ESTUDAR

### 🔍 Antes de Implementar
1. **História do ASCII:** Por que 128 caracteres e não mais?
2. **Diferença entre ASCII e Unicode:** Limitações do ASCII
3. **Caracteres de controle:** O que são e para que servem
4. **ASCII estendido:** Por que valores 128-255 não são "ASCII puro"

### 🎯 Perguntas para Reflexão
- Por que ASCII vai até 127 e não 128?
- Qual a diferença entre ASCII e UTF-8?
- Por que -1 não é considerado ASCII?
- Como ft_isascii se relaciona com as outras funções?

---

## 🛠️ ESTRATÉGIAS DE IMPLEMENTAÇÃO

### 💭 Abordagem 1: Pensamento Lógico
```
Se (valor está entre 0 e 127):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 2: Matemática Direta
```
Se (0 ≤ c ≤ 127):
    retorna verdadeiro
Senão:
    retorna falso
```

### 💭 Abordagem 3: Expressão Booleana
```
resultado = (c >= 0) && (c <= 127)
retorna resultado
```

### 🔧 Dicas de Implementação
- Use comparação direta com **números** (0, 127)
- **NÃO use aspas** ('0', '127' são inválidos)
- Uma única condição com operador `&&`
- Considere valores negativos (devem retornar falso)
- Mais simples que ft_isalpha (só um intervalo)

---

## 🎓 EXERCÍCIOS PARA PRATICAR

### 🥉 Nível Iniciante
1. Implemente `ft_isascii` seguindo os conceitos acima
2. Teste com valores extremos (0, 127, 128, -1)
3. Compare com a função original `isascii`
4. Entenda por que caracteres estendidos não são ASCII

### 🥈 Nível Intermediário
1. Crie uma função que conta caracteres ASCII em string
2. Implemente um filtro que remove caracteres não-ASCII
3. Crie um validador de texto "ASCII puro"
4. Combine ft_isascii com outras funções para validação completa

### 🥇 Nível Avançado
1. Implemente um conversor ASCII ↔ Unicode básico
2. Crie um detector de encoding (ASCII vs UTF-8)
3. Desenvolva um sanitizador de dados para protocolos antigos
4. Implemente análise estatística de caracteres em arquivo

---

## 🔗 FUNÇÕES RELACIONADAS

### 🔤 Mesma Categoria
- [`ft_isalpha`](ft_isalpha.md) - Verifica se é letra (subconjunto de ASCII)
- [`ft_isdigit`](ft_isdigit.md) - Verifica se é dígito (subconjunto de ASCII)
- [`ft_isalnum`](ft_isalnum.md) - Verifica se é alfanumérico (subconjunto de ASCII)
- [`ft_isprint`](ft_isprint.md) - Verifica se é imprimível (subconjunto de ASCII)

### 🔄 Funções que Dependem de ASCII
- Todas as funções de caractere assumem entrada ASCII válida
- `ft_toupper` e `ft_tolower` trabalham apenas com ASCII

---

## 📊 CURIOSIDADES TÉCNICAS

### 🔢 Matemática do ASCII
```
ASCII = 7 bits = 2^7 = 128 possibilidades
Range: 0 a 127 (not 1 a 128!)

Por que 7 bits?
- Computadores antigos: 8 bits total
- 7 bits para dados + 1 bit para paridade
- Padrão ficou para sempre!
```

### 🌍 ASCII vs Unicode
```
ASCII:    128 caracteres (inglês básico)
Unicode:  1,112,064 caracteres (mundo todo!)

UTF-8 é compatível com ASCII:
- Primeiros 128 valores = ASCII idêntico
- Valores 128+ = sequências multi-byte
```

### 🎯 Hierarquia das Verificações
```
ft_isascii (0-127)
    └── ft_isprint (32-126)
            ├── ft_isalnum
            │   ├── ft_isalpha (A-Z, a-z)
            │   └── ft_isdigit (0-9)
            └── símbolos (!@#$%...)
    └── controle (0-31, 127)
```

---

## 📖 MATERIAL DE APOIO

### 📚 Recursos Didáticos
- [📊 Tabela ASCII Completa](../../resources/ascii_table.md)
- [🌍 História do ASCII](../../resources/ascii_history.md)
- [🔤 ASCII vs Unicode](../../resources/encoding_comparison.md)
- [🎯 Caracteres de Controle](../../resources/control_characters.md)

### 🔗 Referências Externas
- Manual do C: `man isascii`
- [ASCII Table Reference](https://www.asciitable.com/)
- [Unicode vs ASCII](https://home.unicode.org/)

---

## ✍️ NOTAS PESSOAIS

### 📝 Meu Processo de Aprendizado
> **Documenta aqui suas descobertas, dificuldades e insights durante a implementação!**

**Dificuldades encontradas:**
- [ ] Entender por que é 0-127 e não 1-128
- [ ] Diferença entre caracteres e valores numéricos
- [ ] Por que usar números sem aspas (0 vs '0')
- [ ] Relação com outras funções de caractere

**Descobertas importantes:**
- [ ] ASCII é base para todas as outras verificações
- [ ] Por que valores negativos não são válidos
- [ ] Diferença entre ASCII e caracteres estendidos
- [ ] Como essa função é mais fundamental que as outras

**Testes que fiz:**
- [ ] Teste com valor 0 (null terminator)
- [ ] Teste com valor 127 (DEL)
- [ ] Teste com valores negativos
- [ ] Teste com valores acima de 127

**Reflexões:**
- [ ] Por que computadores antigos limitaram ASCII a 7 bits?
- [ ] Como ASCII influencia programação moderna?
- [ ] Quando usar ft_isascii vs outras verificações?

---
<div align="center">
  
**[← Anterior: ft_isalnum](ft_isalnum.md)** | **[Próxima: ft_isprint →](ft_isprint.md)**

**🔤 [Funções de Caractere](../../README.md#-funções-de-caractere)** | **[📚 Voltar ao Índice](../../README.md)**

---

**🛡️ Material Educativo - Libftosa**  
*Desenvolvendo conceitos, não copiando soluções*

</div>
