# 🧩 Model – Atributos Virtuais (get_*)  
(MadBuilder / Adianti)

Este snippet demonstra como **criar atributos virtuais em Models**
no Adianti/MadBuilder utilizando métodos `get_`.

Com isso, é possível acessar **dados formatados ou modificados**
como se fossem **colunas reais do banco**.

---

## 🎯 Conceito

No Active Record do Adianti:

- `$pessoa->nome_completo` → coluna real do banco
- `$pessoa->nome_completo_html` → atributo virtual

O atributo virtual é criado automaticamente quando existe um método:

```php
get_nome_completo_html()
```

E pode ser usado:
- em código PHP
- em `TDBCombo`
- em templates HTML
- em grids e listas

---

## 🧱 Criar o método no Model

Edite a **model da tabela** e adicione o método desejado.

### Exemplo: `TipoIngresso`

```php
public function get_tipo_ingresso_html()
{
    return "{$this->nome}<br>
            {$this->descricao}<br>
            Quantidade: {$this->quantidade} Ingressos |
            Valor Total: {$this->preco}";
}
```

📌 Esse método **não existe no banco**, mas passa a ser acessível como:

```php
$ingresso->tipo_ingresso_html;
```

---

## 🎛️ Usar atributo virtual no `TDBCombo`

No `TDBCombo`, basta informar o atributo virtual
entre chaves `{}`.

```php
$combo = new TDBCombo(
    'tipo_ingresso_id',
    'database',
    'TipoIngresso',
    'id',
    '{tipo_ingresso_html}',
    'nome asc'
);
```

---

## 🎨 Ajustar CSS para exibição correta

Como o conteúdo possui múltiplas linhas (`<br>`),
é necessário ajustar o CSS para permitir altura dinâmica.

### Arquivo
```text
builder_user_custom_css.css
```

### CSS
```css
[page_name="ComprarIngressoPublicForm"]
.select2-container--default
.select2-selection--single {
    min-height: var(--field-height) !important;
    height: auto !important;
}
```

---

## ⚠️ Observações importantes

- O método deve começar com `get_`
- O nome após `get_` vira o atributo virtual
- Pode conter HTML
- Não substitui validações de backend
- Ideal para melhorar UX sem duplicar dados

---

## 🧠 Boas práticas

- Use atributos virtuais apenas para apresentação
- Evite lógica de negócio complexa no model
- Documente atributos virtuais usados em formulários
- Padronize nomes (`*_html`, `*_label`, etc.)

---

## 📎 Observação final

Este arquivo documenta um **recurso avançado e elegante**
do Active Record no MadBuilder / Adianti.

Veja também:
- `formularios.md`
- `componentes-especiais.md`
- `estilos-globais.md`
