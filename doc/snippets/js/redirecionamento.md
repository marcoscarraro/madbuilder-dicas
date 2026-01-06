# ➡️ Redirecionamento – JavaScript / jQuery (MadBuilder)

Este arquivo reúne snippets para **redirecionamento de páginas**
utilizando **JavaScript, jQuery e TScript** no **MadBuilder / Adianti**.

Use estes exemplos para controlar navegação, abertura de novas páginas
e fluxos automáticos da interface.

---

## 🌐 Redirecionar página atual

### Redirecionamento simples
```php
TScript::create("window.location.href = 'https://www.exemplo.com';");
```

---

## 🔁 Substituir URL (sem histórico)

```php
TScript::create("window.location.replace('https://www.exemplo.com');");
```

---

## 🪟 Abrir nova aba ou janela

### Abrir nova aba
```php
TScript::create("window.open('https://www.exemplo.com', '_blank');");
```

### Abrir nova janela com configurações
```php
TScript::create("
    window.open(
        'https://www.exemplo.com',
        '_blank',
        'width=1200,height=800'
    );
");
```

---

## ⏱️ Redirecionamento com delay

```php
TScript::create("
    setTimeout(function(){
        window.location.href = 'https://www.exemplo.com';
    }, 3000);
");
```

---

## 🔄 Redirecionar após ação específica

### Redirecionar após salvar registro
```php
TScript::create("
    setTimeout(function(){
        __adianti_load_page(
            'engine.php?class=HomeView&method=onShow'
        );
    }, 1000);
");
```

---

## 🔁 Recarregar página

### Recarregar página atual
```php
TScript::create("location.reload();");
```

---

## 🧠 Redirecionamento condicional

```php
TScript::create("
    if ({$status} === 'ok') {
        window.location.href = 'sucesso.php';
    } else {
        window.location.href = 'erro.php';
    }
");
```

---

## ⚠️ Boas práticas

- Informe o usuário antes de redirecionar
- Evite redirecionamentos inesperados
- Prefira `replace` quando não quiser histórico
- Use delay apenas quando necessário

---

## 📎 Observação final

Este arquivo cobre **redirecionamentos via JavaScript/jQuery**.  
Para mensagens ao usuário, veja:
- `mensagens-e-redirecionamento.md`
