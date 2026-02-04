# 🧩 Snippet – Filtro Default em Listagens (Adianti / MadBuilder)

Este snippet define o **padrão recomendado** para aplicar **filtros default** em listagens, garantindo que:

- O filtro seja aplicado **apenas na primeira carga da tela**
- **Não interfira** quando o usuário realizar buscas manuais
- O comportamento seja previsível e reutilizável em LIST, CARDLIST e DATAGRID

---

## 🎯 Objetivo

Aplicar automaticamente um filtro inicial (ex.: `status = 'A'`) ao abrir a listagem,  
permitindo que o usuário **substitua ou remova** esse filtro ao utilizar a busca.

Exemplo:
- Primeira abertura: mostrar apenas **registros ativos**
- Após busca: respeitar **somente** os filtros informados pelo usuário

---

## 📍 Onde aplicar o filtro default

O filtro default deve ser definido **no construtor da página (`__construct`)**,  
logo após a criação dos campos de filtro.

⚠️ **Nunca aplicar filtro default no `onReload()`**  
⚠️ **Nunca misturar filtro default com lógica do `onSearch()`**

---

## 🧠 Estratégia

- Armazenar o filtro default em **Session**
- Usar a estrutura padrão do Adianti (`TCriteria` / `TFilter`)
- Limpar os filtros da sessão quando o usuário realizar uma busca

---

## 🧪 Implementação

### 🔹 Definição do filtro default (no `__construct`)

```php
//<onAfterFieldsCreation>

$filters = [];
$filters[] = new TFilter('status', '=', 'A'); // filtro default

TSession::setValue(__CLASS__ . '_filters', $filters);

//</onAfterFieldsCreation>
```

📌 Esse filtro será aplicado **somente na carga inicial** da listagem.

---

## 🔍 Busca do usuário (`onSearch`)

No método `onSearch`, o padrão correto é:

1. Limpar filtros anteriores da sessão  
2. Aplicar apenas os filtros informados pelo usuário  
3. Regravar os filtros na sessão  

```php
TSession::setValue(__CLASS__ . '_filter_data', null);
TSession::setValue(__CLASS__ . '_filters', null);

$filters = [];

if (!empty($data->status)) {
    $filters[] = new TFilter('status', '=', $data->status);
}

TSession::setValue(__CLASS__ . '_filters', $filters);
```

📌 A partir desse momento, o **filtro default deixa de existir**  
e a listagem passa a respeitar exclusivamente a busca do usuário.

---

## 🔄 Aplicação dos filtros no `onReload`

```php
if ($filters = TSession::getValue(__CLASS__ . '_filters')) {
    foreach ($filters as $filter) {
        $criteria->add($filter);
    }
}
```

📌 Código genérico e reutilizável, independente da origem do filtro.

---

## ✅ Benefícios do padrão

- ✔ Filtro default aplicado corretamente  
- ✔ Busca do usuário nunca é sobrescrita  
- ✔ Código limpo e previsível  
- ✔ Compatível com LIST, CARDLIST e DATAGRID  
- ✔ Fácil manutenção e extensão  

---

## 🚫 O que NÃO fazer

- ❌ Aplicar filtro default diretamente no `onReload`  
- ❌ Recriar filtro default dentro do `onSearch`  
- ❌ Misturar regra de negócio com lógica de busca  
- ❌ Forçar filtros invisíveis após o usuário pesquisar  

---

## 🏁 Conclusão

> **Filtro default é comportamento inicial, não regra fixa.**

Esse padrão garante UX correta e evita efeitos colaterais em listagens complexas.

📌 **Padrão recomendado para projetos MadBuilder / Adianti**
