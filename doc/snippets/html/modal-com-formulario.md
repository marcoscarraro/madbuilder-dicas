# 🪟 Modal com Formulário (TWindow + Form)  
(MadBuilder / Adianti)

Este snippet demonstra como **criar um modal (`TWindow`)**
contendo um **formulário funcional dentro do corpo**,
utilizando **BootstrapFormBuilder** e componentes de banco.

Esse padrão é ideal para:
- seleções rápidas
- ações auxiliares
- confirmações com dados
- formulários sem sair da tela principal

---

## 🎯 Cenário

- Abrir um modal
- Exibir um formulário dentro dele
- Submeter dados normalmente para um método PHP
- Manter a navegação fluida

---

## 🧱 Criar o formulário do modal

```php
// Cria o formulário que vai dentro do modal
$form2 = new BootstrapFormBuilder('form_modal');
//$form2->setFormTitle('Selecionar Categoria');
```

---

## 🧩 Criar componentes do formulário

### Exemplo com `TDBCombo`

```php
$criteria_portaria_selecionada2 = new TCriteria();

$portaria_selecionada2 = new TDBCombo(
    'portaria_selecionada2',
    'bilheteria',
    'Portaria',
    'id',
    '{nome}',
    'nome asc',
    $criteria_portaria_selecionada2
);

$portaria_selecionada2->setSize('100%');
```

---

## ➕ Inserir campos no formulário

```php
$form2->addFields([$portaria_selecionada2]);
```

---

## 💾 Adicionar ação ao formulário

```php
$action2 = new TAction(['ControlePortariaForm', 'processaLeitura']);
$form2->addAction('Salvar', $action2, 'fa:save green');
```

📌 O submit funciona exatamente como um formulário normal.

---

## 🪟 Criar a janela modal (`TWindow`)

```php
$window2 = TWindow::create('Portaria', 0.3, null);
```

### Configurações recomendadas
```php
$window2->removePadding();
$window2->disableEscape();
$window2->disableScrolling();
```

---

## 📥 Inserir o formulário no modal

```php
$window2->add($form2);
```

---

## ▶️ Exibir o modal

```php
$window2->show();
```

---

## ⚠️ Observações importantes

- O formulário mantém validações e ciclo normal
- O modal não recarrega a página
- Ideal para ações auxiliares
- Pode conter qualquer componente (`TEntry`, `TCombo`, `TDBCombo`, etc.)

---

## 🧠 Boas práticas

- Use modais para ações rápidas
- Evite formulários longos em modal
- Nomeie formulários de forma única
- Controle bem ações de submit

---

## 📎 Observação final

Este arquivo documenta o padrão de **modal com formulário funcional**
no MadBuilder / Adianti.

Veja também:
- `formularios.md`
- `componentes-especiais.md`
- `scripts-jquery.md`
