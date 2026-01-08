# 🐘 PHP Tips and Tricks  
(Dicas práticas para escrever código PHP mais limpo e legível)

Este documento reúne **boas práticas, atalhos e padrões mentais**
para escrever **PHP mais simples, legível e fácil de manter**.

As dicas aqui não são regras absolutas, mas **melhorias progressivas**
que fazem muita diferença em projetos reais.

---

## ✅ Tip 1 — Evite `else` quando já existe `return`

Quando uma função usa `return`, **qualquer código abaixo dele não será executado**.
Isso torna muitos blocos `else` desnecessários.

### ❌ Exemplo redundante

```php
function output_gender(bool $user_is_male)
{
    if ($user_is_male) {
        return "User is male";
    } else {
        return "User is female";
    }
}
```

### ✅ Forma recomendada

```php
function output_gender(bool $user_is_male)
{
    if ($user_is_male) {
        return "User is male";
    }

    return "User is female";
}
```

📌 Benefícios:
- Menos indentação
- Código mais legível
- Fluxo mais claro

---

## ✅ Tip 2 — Trate primeiro o caso com MENOS código (Early Return)

Quando um `if/else` possui um bloco pequeno e outro grande,
**trate primeiro o bloco menor** e saia da função.

### ❌ Código inchado

```php
public function categoryWithPosts($category)
{
    $category = Category::find($category);

    if ($category) {
        $category->posts = $category->posts()->published()->get();
        return response(['data' => $category], 200);
    } else {
        return response(['error' => 'Category not found'], 404);
    }
}
```

### ✅ Código limpo (early return)

```php
public function categoryWithPosts($category)
{
    $category = Category::find($category);

    if (!$category) {
        return response(['error' => 'Category not found'], 404);
    }

    $category->posts = $category->posts()->published()->get();
    return response(['data' => $category], 200);
}
```

📌 Benefícios:
- Menos blocos aninhados
- Mais espaço para lógica principal
- Leitura natural de cima para baixo

---

## ✅ Tip 3 — Verificar múltiplos valores com `in_array`

Evite vários `||` quando estiver comparando com múltiplos valores.

### ❌ Exemplo verboso

```php
if ($item == 'candy' || $item == 'toy') {
    return true;
}

return false;
```

### ✅ Forma recomendada

```php
return in_array($item, ['candy', 'toy']);
```

📌 Ideal quando:
- Existem muitos valores possíveis
- A lista pode crescer no futuro

---

## ✅ Tip 4 — Use `??` (Null Coalescing Operator)

O operador `??` retorna o primeiro valor **que não seja null**.

### ❌ Forma tradicional

```php
$data = [
    'a' => 1,
    'b' => 2,
    'c' => null,
];

return $data['c'] ? $data['c'] : 'No data';
```

### ✅ Forma moderna

```php
return $data['c'] ?? 'No data';
```

### Exemplo real

```php
$user = getUserFromDb($user_id) ?? trigger_error('User id is invalid');
```

📌 Muito usado para:
- valores opcionais
- parâmetros
- arrays
- retorno de funções

---

## ✅ Tip 5 — Comparações mais comuns em PHP

```php
$a == $b    // Igual
$a != $b    // Diferente
$a < $b     // Menor que
$a <= $b    // Menor ou igual
$a > $b     // Maior que
$a >= $b    // Maior ou igual
```

📌 Sempre prefira `===` quando quiser comparar **valor e tipo**.

---

## ✅ Tip 6 — Operadores de atribuição úteis

### Incrementar valores

```php
$valorTotal += $contas['valor'];
```

### Concatenar strings

```php
$linha .= $conteudo;
```

Esses operadores deixam o código:
- mais curto
- mais legível
- menos repetitivo

---

## ✅ Tip 7 — Ordenação em SQL e Arrays

```php
ASC  // Ordem crescente
DESC // Ordem decrescente
```

📌 Use sempre de forma explícita em queries para evitar ambiguidades.

---

## 🧠 Dica mental importante

> Código é lido **mais vezes do que é escrito**.

Sempre que possível:
- reduza indentação
- elimine `else` desnecessários
- use retornos antecipados
- prefira clareza à “esperteza”

---

## 📎 Conclusão

Essas dicas:
- não quebram código
- não mudam performance negativamente
- aumentam muito a legibilidade

Aplique aos poucos e seu código PHP
vai ficar cada vez mais profissional.
