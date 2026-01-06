# 🔌 AJAX – MadBuilder / Adianti (Padrão Oficial)

Este arquivo reúne snippets reais para **chamadas AJAX**
utilizando os **helpers nativos do Adianti / MadBuilder**:

- `__adianti_post_exec`
- `__adianti_ajax_exec`
- `__adianti_post_data`

Esses métodos permitem executar ações no backend
**sem recarregar a página**, mantendo o fluxo da aplicação.

---

## 🔄 POST em background (sem exibir retorno)

Este padrão é muito usado para:
- polling
- consulta de status
- verificação de pagamentos
- execução silenciosa

### Exemplo: polling em background
```php
$formName = self::$formName;

TScript::create("
    window.mp_polling = setInterval(function() {
        __adianti_post_exec(
            'class=VendaIngressoPublicEtapa4form&method=onConsultaPagamento',
            $('#{$formName}').serialize(),
            null,
            null,
            true
        );
    }, {$interval});
");
```

### Observações
- Os dados do formulário são enviados via `POST`
- Nada é exibido em tela
- Executa totalmente em background
- Ideal para tarefas recorrentes

---

## 📥 Executar método AJAX com retorno automático em tela

Este padrão executa um método PHP
e **renderiza automaticamente o retorno na tela**.

Isso é controlado pelo último parâmetro `true`.

```php
TScript::create("
    __adianti_ajax_exec(
        'class=VendaIngressoPublicEtapa2form&method=onPopularTfield',
        null,
        true
    );
");
```

### Quando usar
- Atualizar campos
- Popular componentes
- Recarregar partes da tela
- Executar lógica sem submit

---

## 📤 Submit de formulário (padrão Adianti)

Executa o submit do formulário usando o fluxo padrão do framework.

```php
$formName = self::$formName;

TScript::create("
    __adianti_post_data(
        '{$formName}',
        'class=VendaIngressoPublicEtapa2form&method=onPopularTfield'
    );
");
```

### Vantagens
- Respeita validações
- Usa o ciclo padrão do formulário
- Ideal para ações que dependem do estado do form

---

## ⚠️ Diferença entre os métodos

| Método | Uso principal |
|------|-------------|
| `__adianti_post_exec` | POST em background |
| `__adianti_ajax_exec` | Executar método com retorno visual |
| `__adianti_post_data` | Submit padrão de formulário |

---

## 🧠 Boas práticas

- Prefira métodos nativos do Adianti
- Use polling com cuidado (limpe o `setInterval`)
- Evite múltiplas chamadas simultâneas
- Documente ações executadas em background

---

## 📎 Observação final

Este arquivo documenta o **padrão correto de AJAX**
no MadBuilder / Adianti.

Veja também:
- `carregamento-dinamico.md`
- `redirecionamento.md`
- `formularios.md`
