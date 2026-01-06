# ⚡ Scripts jQuery – MadBuilder / Adianti

Este arquivo reúne snippets para **execução de scripts jQuery e JavaScript**
utilizando o **TScript** no **MadBuilder Framework** (baseado no Adianti).

Os exemplos são úteis para manipular DOM, estilos, eventos e interações dinâmicas
a partir do backend PHP.

---

## ▶️ Executar JavaScript com TScript

### Executar script simples
```php
TScript::create("alert('Script executado com sucesso');");
```

---

## 🧩 Manipulação de elementos DOM

### Alterar CSS de um elemento
```php
TScript::create("
    $('#{$idContainer}').css('margin', '0');
");
```

### Alterar CSS de múltiplos elementos
```php
TScript::create("
    $('#{$idContainer}').parent().css('margin', '0');
    $('#{$idContainer}').parent().parent().css('padding', 0);
    $('#{$idContainer} .tab-pane').css('padding', 0);
", true, 10);
```

---

## 🧲 Trabalhando com seletores

### Selecionar por ID
```php
TScript::create("$('#meu_elemento').hide();");
```

### Selecionar por classe
```php
TScript::create("$('.campo-obrigatorio').addClass('highlight');");
```

### Selecionar por atributo
```php
TScript::create("input[name='cpf']").val('');");
```

---

## 🧠 Eventos jQuery

### Executar script ao carregar página
```php
TScript::create("
    $(document).ready(function(){
        console.log('Página carregada');
    });
");
```

### Executar ação após delay
```php
TScript::create("
    setTimeout(function(){
        alert('Executado após 2 segundos');
    }, 2000);
");
```

---

## 🪟 Ações de navegação

### Abrir nova aba
```php
TScript::create("window.open('{$page}', '_blank');");
```

### Redirecionar página atual
```php
TScript::create("window.location.href = 'https://www.exemplo.com';");
```

---

## 🔄 Atualização dinâmica

### Recarregar página
```php
TScript::create("location.reload();");
```

---

## 🧪 Debug com JavaScript

### Exibir mensagem no console
```php
TScript::create("console.log('Debug JS ativo');");
```

### Exibir variável PHP no console
```php
TScript::create("console.log(" . json_encode($dados) . ");");
```

---

## ⚠️ Boas práticas

- Evite scripts muito longos no backend
- Prefira funções JS reutilizáveis quando possível
- Cuidado com seletores genéricos
- Use `console.log` apenas em desenvolvimento

---

## 📎 Observação final

Este arquivo cobre **execução e manipulação com jQuery via TScript**.  
Para carregamento dinâmico de páginas, veja:
- `carregamento-dinamico.md`
