# 🎨 Estilos Globais – CSS (MadBuilder / Adianti)

Este arquivo reúne snippets para **definição de estilos CSS globais**
aplicados a páginas, formulários e componentes no **MadBuilder / Adianti**.

Estilos globais são ideais quando o mesmo comportamento visual
precisa ser reutilizado em vários pontos da aplicação.

---

## 🌍 Registrar CSS global na página

### Registrar CSS com `TPage::register_css`
```php
$class = _CLASS_;
$css = "
    div[page-name='{$class}'] .card-header.panel-heading {
        position: sticky;
        top: 0;
        z-index: 1000;
    }
";
parent::register_css('sticky_header_' . _CLASS_, $css);
```

---

## 🧱 Criar classes CSS reutilizáveis

### Definir classe CSS global
```php
TPage::register_css(
    'row-full-width',
    '.row-full-width .fb-inline-field-container {
        width: 100% !important;
        float: none !important;
        display: block !important;
    }'
);
```

### Aplicar classe CSS em uma row
```php
$row3->class = 'row-full-width';
```

---

## 🧩 Estilizar campos específicos

### Transformar texto em uppercase
```php
TPage::register_css(
    'uppercase-field',
    'input[name="chave_acesso"] { text-transform: uppercase; }'
);
```

---

## 🎯 Estilos baseados na página atual

```php
$class = _CLASS_;
TPage::register_css(
    'page_specific_style',
    "div[page-name='{$class}'] .panel-body { padding-top: 0; }"
);
```

---

## 🧠 Boas práticas

- Use nomes de classes claros e reutilizáveis
- Evite CSS duplicado
- Centralize estilos globais quando possível
- Utilize `page-name` para evitar conflitos entre telas

---

## 📎 Observação final

Este arquivo cobre **CSS**
