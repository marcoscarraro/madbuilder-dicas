# 🔧 Transformers de Coluna – MadBuilder / Adianti

Este arquivo reúne **snippets de transformers** usados para **formatar ou transformar valores de colunas** em:

- **TDataGrid**
- **Relatórios**
- **Listagens HTML**
- **CardView**

Transformers permitem alterar **visual, texto ou comportamento** do valor exibido sem alterar os dados no banco.

---

# 🧠 Estrutura padrão de um Transformer

Todo transformer segue a seguinte assinatura:

```php
function($value, $object, $row)
{
    // código aqui
}
```

### Parâmetros disponíveis

| Parâmetro | Descrição |
|---|---|
| `$value` | Valor da coluna |
| `$object` | Objeto completo da linha (registro do banco) |
| `$row` | Objeto da linha da tabela |

### Exemplos de acesso ao objeto

```php
$object->id
$object->nome
$object->estado->nome
$object->cidade->nome
$object->status_id
```

---

# 🎨 Cor HEX → Cor visual

Exibe um pequeno bloco com a cor cadastrada.

```php
function($value, $object, $row)
{
    return "<div style='width:40px;height:20px;border-radius:4px;background:{$value};'></div>";
}
```

---

# 💳 Status de pagamento com cor dinâmica

Usa relacionamento da tabela para obter **nome e cor do status**.

```php
function($value, $object, $row)
{
    return "<span class='label label-default' style='color:#fff;background-color:{$object->status_pagamento->cor}'>
            {$object->status_pagamento->nome}
            </span>";
}
```

---

# 🚪 Status da portaria

Transforma código em texto legível.

```php
function($value, $object, $row)
{
    switch ($value)
    {
        case 'A':
            return 'Aberta';

        case 'F':
            return 'Fechada';

        default:
            return $value;
    }
}
```

---

# 📋 Presença / Falta com estilo visual

```php
function($value, $object, $row)
{
    if ($value == 'P')
    {
        return '<div style="background:#e0f2fe;color:#0369a1;padding:4px 10px;border-radius:20px;font-weight:bold;">Presença</div>';
    }
    elseif ($value == 'F')
    {
        return '<div style="background:#fee2e2;color:#b91c1c;padding:4px 10px;border-radius:20px;font-weight:bold;">Falta</div>';
    }

    return $value;
}
```

---

# ✍️ Status de assinatura de contrato

```php
function($value, $object, $row)
{
    switch ($value)
    {
        case 'S':
            return '<span class="badge bg-success">Assinado</span>';

        case 'N':
            return '<span class="badge bg-danger">Pendente</span>';

        default:
            return '<span class="badge bg-secondary">Pendente</span>';
    }
}
```

---

# ✂️ Truncar texto longo

Limita o tamanho exibido.

```php
function($value, $object, $row)
{
    return substr($value, 0, 50);
}
```

Versão com reticências:

```php
function($value, $object, $row)
{
    return strlen($value) > 50
        ? substr($value,0,50) . '...'
        : $value;
}
```

---

# 🧾 Status de nota fiscal com TElement

Exemplo usando **componente visual do Adianti**.

```php
function($value, $object, $row)
{
    $div = new TElement('span');
    $div->class = 'label label-default';
    $div->style = "background-color: {$object->nota_fiscal_status->cor}; color:#fff";
    $div->add($object->nota_fiscal_status->nome);

    return $div;
}
```

---

# ⚠️ Tooltip para erro

Mostra erro ao passar mouse.

```php
function($value, $object, $row)
{
    if ($value == 'erro')
    {
        return "<span class='text-danger'
                data-toggle='tooltip'
                data-placement='top'
                title='{$object->erro}'>
                Erro
                </span>";
    }

    return "<span class='text-success'>Sucesso</span>";
}
```

---

# ✅ Sim / Não com label

Aceita múltiplos formatos de valor.

```php
function($value, $object, $row)
{
    if ($value === 'T' || $value === true || $value === 't' ||
        $value === 'S' || $value === 'Y' || $value === 'y')
    {
        return '<span class="label label-success">Sim</span>';
    }
    elseif ($value === 'F' || $value === false || $value === 'f' ||
            $value === 'N' || $value === 'n')
    {
        return '<span class="label label-danger">Não</span>';
    }

    return '';
}
```

---

# 📄 nl2br (quebra de linha automática)

```php
function($value, $object, $row)
{
    if ($value)
    {
        return nl2br($value);
    }

    return $value;
}
```

---

# 👤 Nome do solicitante ou atendente

Retorna usuário dependendo da origem.

```php
function($value, $object, $row)
{
    return $object->cliente_id
        ? $object->cliente->system_user->name
        : $object->atendente->system_user->name;
}
```

---

# 👤 Usuário ativo / inativo

```php
function($value, $object, $row)
{
    $class = ($value == 'N') ? 'danger' : 'success';
    $label = ($value == 'N') ? _t('No') : _t('Yes');

    $div = new TElement('span');
    $div->class = "label label-{$class}";
    $div->style = "text-shadow:none;font-size:12px;font-weight:lighter";
    $div->add($label);

    return $div;
}
```

---

# 📅 Mês / Ano (YYYYMM → jan/25)

```php
function($value, $object, $row)
{
    if (empty($value))
    {
        return '';
    }

    $meses = [
        1 => 'jan',
        2 => 'fev',
        3 => 'mar',
        4 => 'abr',
        5 => 'mai',
        6 => 'jun',
        7 => 'jul',
        8 => 'ago',
        9 => 'set',
        10 => 'out',
        11 => 'nov',
        12 => 'dez',
    ];

    return $meses[(int)substr($value, 4, 2)] . '/' . substr($value,2, 2);
}
```

---

# 💰 Formatar valor monetário

```php
function($value, $object, $row)
{
    return 'R$ ' . number_format($value, 2, ',', '.');
}
```

---

# 📞 Formatar telefone

```php
function($value, $object, $row)
{
    if (strlen($value) == 11)
    {
        return preg_replace("/(\d{2})(\d{5})(\d{4})/", "($1) $2-$3", $value);
    }

    if (strlen($value) == 10)
    {
        return preg_replace("/(\d{2})(\d{4})(\d{4})/", "($1) $2-$3", $value);
    }

    return $value;
}
```

---

# 📊 Barra de progresso simples

Muito útil para dashboards.

```php
function($value, $object, $row)
{
    $percent = min(100, (int)$value);

    return "
        <div style='background:#e5e7eb;border-radius:4px;height:8px;width:100px'>
            <div style='background:#22c55e;height:8px;border-radius:4px;width:{$percent}%'></div>
        </div>
    ";
}
```

---

# 🧠 Boas práticas

✔ Sempre retornar **HTML válido**  
✔ Evitar lógica pesada no transformer  
✔ Usar transformer apenas para **apresentação**  
✔ Para lógica complexa prefira **ViewModel ou Service**

---

# 📎 Observação importante

Transformers são executados **para cada linha da listagem**.

Portanto:

- Evite queries dentro do transformer
- Evite processamento pesado
- Prefira usar dados já carregados no `$object`

---

# 🎯 Objetivo deste arquivo

Criar um **repositório padrão de transformers reutilizáveis** para projetos **MadBuilder / Adianti**, garantindo:

- consistência visual
- melhor UX
- menos código duplicado
- desenvolvimento mais rápido
