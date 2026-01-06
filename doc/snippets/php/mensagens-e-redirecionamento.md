# 💬 Mensagens e Redirecionamento – MadBuilder / Adianti

Este arquivo reúne snippets para **exibir mensagens ao usuário**, **toasts**,  
**redirecionamentos internos e externos**, além de ações comuns de navegação
no **MadBuilder Framework** (baseado no Adianti).

---

## 📢 Exibir mensagens para o usuário

### Mensagem simples (modal)
```php
new TMessage('info', 'Operação realizada com sucesso!');
new TMessage('error', 'Ocorreu um erro durante o processo.');
new TMessage('warning', 'Atenção! Verifique os dados informados.');
```

---

## 🔔 Exibir mensagens usando Toast

### Toast de sucesso
```php
TToast::show('success', 'Registro salvo com sucesso', 'topRight', 'far:check-circle');
```

### Toast de erro
```php
TToast::show('error', 'Erro ao salvar registro', 'topRight', 'far:times-circle');
```

### Toast de aviso
```php
TToast::show('warning', 'Verifique os campos obrigatórios', 'topRight', 'far:exclamation-triangle');
```

---

## 🔀 Redirecionamento interno (entre páginas)

### Redirecionar para outra página (classe)
```php
AdiantiCoreApplication::loadPage('NomeDaClasseView');
```

### Redirecionar chamando método e parâmetros
```php
TApplication::gotoPage(
    'ContratoAssinadoForm',
    'onShow',
    ['pdf' => 'testeabc.pdf']
);
```

---

## 🌐 Redirecionamento para site externo

```php
TScript::create('window.location.href = "https://www.trisistemas.com.br/";');
```

---

## ⏱️ Redirecionamento com delay (timeout)

```php
TScript::create("
    setTimeout(() => {
        window.location.replace('showpdf.php?pdf={$param['pdfContratoAssinado']}')
    }, 2000);
");
```

---

## 🔄 Recarregar página ou conteúdo

### Recarregar página inteira
```php
TScript::create('location.reload();');
```

### Recarregar conteúdo dinamicamente (Adianti)
```php
TScript::create(
    "__adianti_load_page(
        'engine.php?class=ProcessoForm&method=onReloadPoloProcessual&static=1'
    );"
);
```

---

## ♻️ Recarregar conteúdo específico com jQuery

### Atualizar container específico
```php
TScript::create("
    $('#content_polo_processual').load(
        'engine.php?class=ProcessoForm&method=onReloadPoloProcessual #content_polo_processual'
    );
");
```

---

## 🔁 Atualização automática (timeout)

### Recarregar datagrid após tempo definido
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

### Recarregar outra view automaticamente
```php
TScript::create("
    $(document).ready(function(){
        window.setTimeout(function(){
            __adianti_load_page(
                'engine.php?class=AtividadesKanbanView&method=onShow'
            );
        }, 0);
    });
");
```

---

## 🪟 Ações de interface

### Abrir página em nova aba
```php
TScript::create("window.open('{$page}', '_blank');");
```

### Fechar janela
```php
TWindow::closeWindow();
```

### Fechar painel lateral (cortina)
```php
TScript::create("Template.closeRightPanel()");
```

---

## 📎 Observações importantes

- Prefira **Toast** para feedback rápido
- Use **TMessage** para mensagens críticas ou bloqueantes
- Evite redirecionar sem informar o usuário
- Redirecionamentos com `TScript` permitem maior controle da interface

Este arquivo cobre **mensagens e navegação**.  
Veja também:
- `formularios.md`
- `validacoes.md`
- `debug.md`
