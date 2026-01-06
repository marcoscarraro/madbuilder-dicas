# 🔄 Carregamento Dinâmico – MadBuilder / Adianti

Este arquivo reúne snippets para **carregamento dinâmico de páginas, views e conteúdos**
utilizando **jQuery**, **TScript** e funções nativas do **Adianti/MadBuilder**.

Esses recursos são amplamente usados para:
- atualização parcial de telas
- reload automático
- carregamento sem recarregar a página inteira

---

## ⚙️ Carregar página dinamicamente (Adianti)

### Carregar página com `__adianti_load_page`
```php
TScript::create(
    "__adianti_load_page(
        'engine.php?class=ProcessoForm&method=onReloadPoloProcessual&static=1'
    );"
);
```

---

## 🧩 Carregar conteúdo específico (jQuery)

### Recarregar apenas um container
```php
TScript::create("
    $('#content_polo_processual').load(
        'engine.php?class=ProcessoForm&method=onReloadPoloProcessual #content_polo_processual'
    );
");
```

---

## ⏱️ Carregamento com delay

### Executar reload após tempo definido
```php
TScript::create("
    setTimeout(function(){
        __adianti_load_page(
            'engine.php?class=AtividadesKanbanView&method=onShow'
        );
    }, 2000);
");
```

---

## 🔁 Atualização automática (intervalo)

### Recarregar página em intervalo contínuo
```php
TScript::create("
    setInterval(function(){
        __adianti_load_page(
            'engine.php?class=DashboardView&method=onReload'
        );
    }, 10000);
");
```

---

## 📊 Atualizar componentes específicos

### Recarregar Datagrid
```php
TScript::create("
    $(document).ready(function(){
        window.setTimeout(function(){
            __adianti_load_page(
                'engine.php?class=BateriaSimpleList&method=onRefresh&target_container=b66f15f3f5c62f'
            );
        }, 5000);
    });
");
```

---

## ♻️ Atualizar view atual

```php
TScript::create("
    __adianti_load_page(
        'engine.php?class=' + Adianti.currentClass + '&method=onShow'
    );
");
```

---

## 🧠 Boas práticas

- Prefira carregamento parcial para melhor performance
- Evite `setInterval` sem necessidade
- Use `target_container` para atualizar áreas específicas
- Atenção ao uso excessivo de reload automático

---

## 📎 Observação final

Este arquivo cobre **carregamento dinâmico de conteúdo**.  
Para scripts gerais, veja:
- `scripts-jquery.md`
- `redirecionamento.md`
