# 🎨 Estilos Inline – CSS (MadBuilder / Adianti)

Este arquivo reúne snippets para **aplicação de estilos CSS inline**
em componentes, campos e elementos do **MadBuilder / Adianti**.

Os estilos inline são úteis para ajustes pontuais e rápidos,
especialmente quando o estilo se aplica a apenas um elemento específico.

---

## ✏️ Aplicar estilo inline diretamente

### Definir estilo em componente
```php
$text31->style = 'float: right !important;';
```

---

## 🧩 Estilizar campos de formulário

### Alterar alinhamento de campo
```php
$entry->style = 'text-align: right;';
```

### Definir largura fixa
```php
$entry->style = 'width: 200px;';
```

---

## 🧱 Estilos em containers

### Estilizar `div` ou container
```php
$container->style = 'padding: 0; margin: 0;';
```

---

## 🧬 Combinar múltiplos estilos

```php
$label->style = 'font-weight: bold; color: #ff0000; font-size: 14px;';
```

---

## ⚠️ Observações importantes

- Estilos inline devem ser aplicados **antes de montar a página**
- Use `!important` apenas quando necessário
- Evite excesso de CSS inline para manter organização
- Prefira CSS global quando o estilo for reutilizável

---

## 📎 Observação final

Este arquivo cobre **aplicação de CSS inline**.  
Para estilos reutilizáveis e globais, veja:
- `estilos-globais.md`
