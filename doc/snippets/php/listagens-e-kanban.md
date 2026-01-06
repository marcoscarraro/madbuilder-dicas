# 📋 Listagens e Kanban – MadBuilder / Adianti

Este arquivo reúne snippets para **aplicação de filtros**
em **Listagens, Datagrids e Kanban**
no **MadBuilder Framework** (baseado no Adianti).

Esses filtros normalmente são aplicados via:
- parâmetros (`$param`)
- sessão
- ações de reload

---

## 🔍 Aplicar filtro em Listagem

### Filtrar dados via parâmetro
```php
$criteria = new TCriteria;
$criteria->add(new TFilter('status', '=', $param['status']));
```

---

## ♻️ Aplicar filtro usando sessão

### Salvar filtro em sessão
```php
TSession::setValue('kanban_status', $param['status']);
```

### Recuperar filtro da sessão
```php
$status = TSession::getValue('kanban_status');
```

---

## 🧩 Filtro em Kanban

### Aplicar filtro no carregamento
```php
$criteria = new TCriteria;

if ($status = TSession::getValue('kanban_status')) {
    $criteria->add(new TFilter('status', '=', $status));
}
```

---

## 🔄 Recarregar Kanban com filtro

```php
__adianti_load_page(
    'engine.php?class=AtividadesKanbanView&method=onReload'
);
```

---

## 🧠 Boas práticas

- Prefira filtros via `TCriteria`
- Evite lógica pesada no `onShow`
- Use sessão para manter estado do filtro
- Limpe filtros quando necessário

---

## 📎 Observação final

Este arquivo cobre **filtros em Listagens e Kanban**.  
Veja também:
- `banco-de-dados.md`
- `carregamento-dinamico.md`
