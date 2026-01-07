# 🧩 THtmlRenderer – Múltiplas Seções no Mesmo Template  
(MadBuilder / Adianti)

Este snippet demonstra como **unificar duas ou mais seções**
de um **mesmo arquivo HTML**
utilizando o `THtmlRenderer`.

Esse padrão é muito útil quando:
- o template possui `<style>` e `<html>` separados
- você precisa ativar múltiplas seções no mesmo retorno
- o conteúdo final será injetado via JavaScript

---

## 🎯 Cenário

- Um único arquivo HTML (`status_pagamento.html`)
- Contém múltiplas seções:
  - `css`
  - `pendente`
- Ambas precisam aparecer **juntas** no resultado final

👉 O segredo está na **concatenação dos `getContents()`**.

---

## 📄 Estrutura do template HTML (exemplo)

```html
<!-- {css} -->
<style>
.status-pendente {
    color: orange;
}
</style>

<!-- {/css} -->

<!-- {pendente} -->
<div class="status-pendente">
    <p>Pagamento ID: {pagamento_id}</p>
    <p>Valor: R$ {valor}</p>
    <p>Data: {data_atual}</p>
</div>
<!-- {/pendente} -->
```

---

## 🧩 Carregar múltiplas seções do mesmo arquivo

### Etapa 1 – Renderer da seção CSS
```php
$css_status = new THtmlRenderer('app/resources/status_pagamento.html');
$css_status->enableSection('css');
```

---

### Etapa 2 – Renderer da seção HTML
```php
$html_status = new THtmlRenderer('app/resources/status_pagamento.html');
$html_status->enableSection('pendente', [
    'pagamento_id' => $criaVenda['pagamento_id'],
    'valor'        => $criaVenda['valor'],
    'data_atual'   => date('d/m/Y H:i')
]);
```

---

## 🔗 Unificar os conteúdos

Aqui está o **ponto-chave**:  
👉 concatenar os resultados dos dois renderers.

```php
$html_status =
    $css_status->getContents()
    . $html_status->getContents();
```

---

## 🧹 Limpar conteúdo anterior (opcional)

```php
TScript::create("$('#html_resumo_compra').empty();");
```

---

## 📥 Injetar HTML final na tela

```php
TScript::create("
    $('#html_status_pagamento').html(`{$html_status}`);
");
```

---

## ⚠️ Observações importantes

- Cada seção precisa de um `THtmlRenderer` separado
- `enableSection()` **não acumula seções**
- Sempre use `getContents()` para concatenar
- Ideal para banners, status, resumos e componentes visuais

---

## 🧠 Boas práticas

- Centralize CSS dentro da seção `{css}`
- Evite lógica de negócio no HTML
- Use nomes de seção claros
- Documente quando o template tiver múltiplas seções

---

## 📎 Observação final

Este arquivo documenta um **padrão avançado de uso do THtmlRenderer**.

Veja também:
- `belement-html.md`
- `scripts-jquery.md`
- `carregamento-dinamico.md`
