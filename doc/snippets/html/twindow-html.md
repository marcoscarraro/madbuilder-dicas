# 🪟 TWindow com conteúdo HTML  
(MadBuilder / Adianti)

Este snippet demonstra como **criar uma janela modal (`TWindow`)**
contendo **HTML livre**, utilizando `BElement`.

É ideal para:
- telas de ajuda
- listas de tags disponíveis
- instruções rápidas
- termos e avisos
- conteúdo informativo sem formulário

---

## 🎯 Cenário

Você precisa exibir um conteúdo HTML dentro de uma janela modal,
sem criar um formulário ou página dedicada.

---

## 🧱 Criando o conteúdo HTML

```php
$html_tags = 'conteúdo html';
```

📌 Pode ser:
- HTML montado em string
- HTML vindo de template
- HTML gerado dinamicamente

---

## 🧩 Criando o container HTML com BElement

```php
$element_tags = new BElement('div');
$element_tags->add($html_tags);
```

O `BElement` funciona como um **container genérico de HTML**.

---

## 🪟 Criando a TWindow

```php
$window = TWindow::create(
    'Tags disponíveis para o contrato',
    0.8,
    650
);
```

### Parâmetros:
- **Título da janela**
- **Largura** (percentual da tela)
- **Altura** (em pixels)

---

## ➕ Adicionando o conteúdo na janela

```php
$window->add($element_tags);
$window->show();
```

---

## 🧠 Exemplo completo

```php
$html_tags = 'conteúdo html';

$element_tags = new BElement('div');
$element_tags->add($html_tags);

$window = TWindow::create('Tags disponíveis para o contrato', 0.8, 650);
$window->add($element_tags);
$window->show();
```

---

## ⚠️ Observações importantes

- `BElement` aceita qualquer HTML válido
- Não é necessário usar `BootstrapFormBuilder`
- Ideal para conteúdo somente leitura
- Pode ser combinado com `THtmlRenderer`

---

## 💡 Boas práticas

- Use para conteúdos auxiliares
- Evite lógica de negócio dentro da window
- Prefira HTML limpo e organizado
- Para formulários, utilize `BootstrapFormBuilder`

---

## 📎 Observação final

Este snippet cobre **TWindow com HTML puro**.  
Para janelas com formulário, veja:
- `twindow-formulario.md`
- `belement-html.md`
