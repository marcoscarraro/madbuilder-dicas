# 👁️ Mostrar / Ocultar Campos Dinamicamente – MadBuilder / Adianti

Snippet usado para **mostrar ou ocultar linhas de campos de formulário dinamicamente**  
com base em uma seleção feita pelo usuário (ex: tipo de promoção, tipo de cliente, tipo de pagamento, etc).

Este padrão é muito útil para:

- Simplificar formulários
- Melhorar UX
- Exibir apenas campos relevantes
- Reduzir erros de preenchimento

A técnica utiliza **jQuery + TScript** para localizar o campo e ocultar a **linha completa do formulário**.

---

# 🧠 Conceito

No MadBuilder / Adianti, os campos ficam dentro de uma estrutura HTML semelhante a:

```
.row
   label
   input
```

Portanto usamos:

```
$("[name='campo']").closest('.row').hide()
```

ou

```
$("[name='campo']").closest('.row').show()
```

para ocultar ou mostrar **a linha inteira do campo**.

---

# 📌 Exemplo real – Tipo de promoção

```php
public static function onChangeTipoPromocao($param = null) 
{
    try 
    {
        // Oculta todos os campos primeiro

        TScript::create("$('[name=\"quantidade\"]').closest('.row').hide()");
        TScript::create("$('[name=\"idade_inicial\"]').closest('.row').hide()");
        TScript::create("$('[name=\"idade_final\"]').closest('.row').hide()");
        TScript::create("$('[name=\"cidade\"]').closest('.row').hide()");
        TScript::create("$('[name=\"estado\"]').closest('.row').hide()");
        TScript::create("$('[name=\"cupom\"]').closest('.row').hide()");
        TScript::create("$('[name=\"data_inicio\"]').closest('.row').hide()");
        TScript::create("$('[name=\"data_fim\"]').closest('.row').hide()");
        TScript::create("$('[name=\"desconto\"]').closest('.row').hide()");
        TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').hide()");
        TScript::create("$('[name=\"ativo\"]').closest('.row').hide()");

        switch ($param['tipo_promocao']) 
        {
            case 1:

                TScript::create("$('[name=\"quantidade\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;

            case 2:

                TScript::create("$('[name=\"idade_inicial\"]').closest('.row').show()");
                TScript::create("$('[name=\"idade_final\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;

            case 3:

                TScript::create("$('[name=\"cidade\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;

            case 4:

                TScript::create("$('[name=\"estado\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;

            case 5:

                TScript::create("$('[name=\"data_inicio\"]').closest('.row').show()");
                TScript::create("$('[name=\"data_fim\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;

            case 6:

                TScript::create("$('[name=\"cupom\"]').closest('.row').show()");
                TScript::create("$('[name=\"desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"tipo_desconto\"]').closest('.row').show()");
                TScript::create("$('[name=\"ativo\"]').closest('.row').show()");

            break;
        }
    }
    catch (Exception $e) 
    {
        new TMessage('error', $e->getMessage());    
    }
}
```

---

# 📌 Exemplo simplificado – Tipo de pessoa

```php
public static function onChangeTipoPessoa($param)
{
    try
    {
        TScript::create("$('[name=\"cpf\"]').closest('.row').hide()");
        TScript::create("$('[name=\"cnpj\"]').closest('.row').hide()");

        if ($param['tipo_pessoa'] == 'F')
        {
            TScript::create("$('[name=\"cpf\"]').closest('.row').show()");
        }
        else
        {
            TScript::create("$('[name=\"cnpj\"]').closest('.row').show()");
        }
    }
    catch (Exception $e)
    {
        new TMessage('error', $e->getMessage());
    }
}
```

---

# 📌 Exemplo – Tipo de pagamento

```php
public static function onChangeFormaPagamento($param)
{
    try
    {
        TScript::create("$('[name=\"parcelas\"]').closest('.row').hide()");
        TScript::create("$('[name=\"bandeira_cartao\"]').closest('.row').hide()");

        if ($param['forma_pagamento'] == 'cartao')
        {
            TScript::create("$('[name=\"parcelas\"]').closest('.row').show()");
            TScript::create("$('[name=\"bandeira_cartao\"]').closest('.row').show()");
        }
    }
    catch (Exception $e)
    {
        new TMessage('error', $e->getMessage());
    }
}
```

---

# 📌 Exemplo – Exibir campos avançados

```php
public static function onToggleAvancado($param)
{
    try
    {
        if (!empty($param['modo_avancado']))
        {
            TScript::create("$('.campo-avancado').closest('.row').show()");
        }
        else
        {
            TScript::create("$('.campo-avancado').closest('.row').hide()");
        }
    }
    catch (Exception $e)
    {
        new TMessage('error', $e->getMessage());
    }
}
```

Campos no formulário:

```
<input name="campo_extra" class="campo-avancado">
```

---

# 📌 Exemplo – Ocultar múltiplos campos com array

Forma mais limpa quando existem muitos campos.

```php
public static function hideFields($fields)
{
    foreach ($fields as $field)
    {
        TScript::create("$('[name=\"{$field}\"]').closest('.row').hide()");
    }
}
```

Uso:

```php
self::hideFields([
    'cpf',
    'cnpj',
    'inscricao_estadual',
    'razao_social'
]);
```

---

# 🧠 Boas práticas

✔ Sempre **ocultar todos os campos primeiro**  
✔ Depois mostrar apenas os necessários  
✔ Use `closest('.row')` para ocultar a linha completa  
✔ Evite lógica JS complexa no PHP  
✔ Sempre tratar exceções com `TMessage`

---

# 🎯 Benefícios deste padrão

- Formulários mais limpos  
- UX muito melhor  
- Menos erros de preenchimento  
- Lógica de interface centralizada  
- Fácil manutenção

---

# 📎 Dica avançada

Se usar muito esse padrão, crie helpers:

```
showField('nome_campo')
hideField('nome_campo')
```

Isso reduz drasticamente o código repetido.

---
```
