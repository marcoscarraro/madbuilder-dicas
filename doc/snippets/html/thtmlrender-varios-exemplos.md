# 🧩 SNIPPET COMPLETO – THtmlRenderer (Adianti)

Guia prático e completo de utilização do **THtmlRenderer**, seguindo o padrão oficial de snippets do projeto.

---

# 1️⃣ Uso Básico – Renderizar uma Section

## 📄 Template HTML

```html
<!--[main]-->
<h1>{{titulo}}</h1>
<p>{{descricao}}</p>
<!--[/main]-->
```

## 🧠 PHP

```php
$html = new THtmlRenderer('app/resources/template.html');

$replace = [];
$replace['titulo']    = 'Dashboard';
$replace['descricao'] = 'Bem-vindo ao sistema';

$html->enableSection('main', $replace);

return $html->getContents();
```

✔ Substitui `{{titulo}}` e `{{descricao}}`

---

# 2️⃣ Substituição Simples de Variável

## 📄 Template

```html
<span>{{nome}}</span>
```

## 🧠 PHP

```php
$replace['nome'] = 'Marcos';
```

## ✅ Resultado

```html
<span>Marcos</span>
```

⚠️ Por padrão o valor passa por `htmlspecialchars()` (proteção contra XSS).

---

# 3️⃣ Renderizando HTML (Escape Automático)

## ❌ Problema

```php
$replace['conteudo'] = '<strong>Teste</strong>';
```

Resultado:

```html
&lt;strong&gt;Teste&lt;/strong&gt;
```

O renderer protege contra XSS automaticamente.

---

## ✅ Como Resolver (RAW)

### ✔ Opção 1 – Prefixo RAW:

```php
$replace['conteudo'] = 'RAW:' . '<strong>Teste</strong>';
```

### ✔ Opção 2 – Usar `|raw` no template

```html
{{conteudo|raw}}
```

---

# 4️⃣ Section Repetível (LOOP Manual)

## 📄 Template

```html
<!--[main]-->
<ul>
    <!--[item]-->
    <li>{{nome}} - {{email}}</li>
    <!--[/item]-->
</ul>
<!--[/main]-->
```

## 🧠 PHP

```php
$html = new THtmlRenderer('template.html');

$usuarios = [
    ['nome' => 'Ana',   'email' => 'ana@email.com'],
    ['nome' => 'Lucas', 'email' => 'lucas@email.com'],
];

$html->enableSection('item', $usuarios, TRUE);
$html->enableSection('main');

return $html->getContents();
```

✔ O terceiro parâmetro `TRUE` ativa repetição.

---

# 5️⃣ Section Repetível Dentro da Main (Automática)

Se a section estiver dentro da `main` e você passar o array assim:

```php
$replace['item'] = $usuarios;

$html->enableSection('main', $replace);
```

O próprio renderer detecta e repete automaticamente.

---

# 6️⃣ Usando Objetos

```php
$replace['usuario'] = $obj;
```

## 📄 Template

```html
{{usuario->nome}}
{{usuario->email}}
```

O renderer acessa automaticamente as propriedades públicas do objeto.

---

# 7️⃣ Renderizando Widgets (Objetos com show())

```php
$replace['form'] = $form;
```

Se o objeto possuir método `show()`, o renderer executa automaticamente:

```php
$form->show();
```

✔ Injeta o HTML no template  
✔ Internamente já trata como RAW  

---

# 8️⃣ Renderizando CSS Separadamente (Padrão Recomendado)

Em cenários onde o template possui uma section exclusiva para CSS, é recomendado renderizar o CSS **uma única vez** e depois renderizar apenas a section dinâmica necessária.

## 📄 Template (Exemplo)

```html
<!--[css]-->
<style>
.status-pago { color: green; }
.status-pendente { color: orange; }
</style>
<!--[/css]-->

<!--[pago]-->
<div class="status-pago">Pagamento aprovado</div>
<!--[/pago]-->

<!--[pendente]-->
<div class="status-pendente">Pagamento pendente</div>
<!--[/pendente]-->
```

---

## 🧠 PHP – Renderizando CSS uma vez

```php
// Renderiza CSS (uma vez)
$renderer = new THtmlRenderer('app/resources/status_pagamento.html');
$renderer->enableSection('css');
$css = $renderer->getContents();
```

---

## 🧠 PHP – Renderizando a Section Dinâmica

```php
// Renderiza a section correspondente ao status
$renderer = new THtmlRenderer('app/resources/status_pagamento.html');
$renderer->enableSection($status, $data);
$html = $renderer->getContents();

return $css . $html;
```

✔ Evita duplicação de CSS  
✔ Mantém organização do template  
✔ Ideal para cards dinâmicos, status, badges e blocos reutilizáveis  

---

# 9️⃣ Alterando Conteúdo de BElement em Tempo de Execução

Se for necessário alterar o conteúdo de um `BElement`, **é obrigatório limpar o conteúdo anterior**, senão o sistema irá incrementar.

```php
$this->banner_comprar_ingresso->children = [];
$this->banner_comprar_ingresso->add('Novo conteúdo');
```

---

## ⚠️ Atenção: Métodos Estáticos

Se o método for estático:

```php
$self = new self($param);
$self->banner_comprar_ingresso->children = [];
$self->banner_comprar_ingresso->add('Novo conteúdo');
```

---

# 🔟 Desabilitar uma Section

```php
$html->disableSection('main');
```

---

# 📌 Fluxo Padrão Correto

```php
$html = new THtmlRenderer('arquivo.html');

$html->enableSection('main', $replace);

return $html->getContents();
```

---

# 🧠 Regras de Ouro do THtmlRenderer

✔ Sempre feche sections:

```html
<!--[main]-->
<!--[/main]-->
```

✔ HTML aparecendo como texto?  
→ Provavelmente faltou `RAW:`  

✔ LOOP?  
→ Use array + `enableSection` com `TRUE` ou estrutura automática  

✔ Para HTML dinâmico (componentes, cards, agenda etc)  
→ Use `RAW:`  

❗ Nunca use `RAW` com dados diretos do usuário sem validação  

---

# 📌 Estrutura Interna Importante

O renderer:

- Lê o template linha por linha  
- Detecta sections com:

```html
<!--[section]-->
<!--[/section]-->
```

- Aplica `htmlspecialchars()` por padrão  
- Permite bypass com `RAW:` ou `|raw`  
- Detecta arrays e faz repetição automática  
- Executa `show()` se o objeto tiver esse método  

---

# 🎯 Conclusão

O **THtmlRenderer** é:

✔ Seguro por padrão  
✔ Poderoso com sections  
✔ Muito flexível com loops  
✔ Extremamente organizado quando separado por CSS + blocos dinâmicos  
✔ Fácil de quebrar se esquecer `RAW` 😅  

Agora você tem o manual prático completo dele.
