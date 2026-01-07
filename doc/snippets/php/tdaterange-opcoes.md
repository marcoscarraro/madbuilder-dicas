# 📅 TDateRange – Limitar intervalo de seleção  
(MadBuilder / Adianti)

Este snippet demonstra como **modificar o comportamento do componente `TDateRange`**
para **limitar o intervalo de datas selecionáveis**
utilizando opções lidas posteriormente pelo JavaScript do framework.

Esse padrão é ideal para:
- reservas
- agendamentos
- períodos de utilização
- regras de negócio baseadas em datas

---

## 🎯 Cenário

É necessário:
- definir uma **data mínima** para seleção
- definir uma **data máxima** para seleção
- impedir que o usuário selecione datas fora do intervalo permitido

---

## ⚙️ Configuração via `setOption()`

As opções `startDate` e `endDate` são interpretadas
pelo JavaScript do `TDateRange`.

⚠️ Devem ser definidas **antes da renderização do formulário**.

---

## 🧩 Exemplo prático

```php
//<onBeforeAddFieldsToForm>

// Data mínima = hoje
$data_utilizacao->setOption('startDate', date('d/m/Y'));

// Data máxima = 30 dias após hoje
$data_utilizacao->setOption(
    'endDate',
    date('d/m/Y', strtotime('+30 days'))
);
```

---

## 🧠 Como funciona internamente

- `setOption()` injeta metadados no campo
- O JavaScript do Adianti lê essas opções
- O calendário é renderizado respeitando o intervalo definido
- O usuário não consegue selecionar datas inválidas

---

## ⚠️ Observações importantes

- O formato da data deve ser compatível com o componente (`d/m/Y`)
- O controle é **visual**, não substitui validação no backend
- Sempre valide novamente no `onSave`

---

## 🧠 Boas práticas

- Combine com validação server-side
- Centralize regras de datas quando possível
- Evite lógica complexa diretamente na view
- Documente claramente o motivo do limite

---

## 📎 Observação final

Este arquivo documenta um **comportamento avançado do TDateRange**.

Veja também:
- `formularios.md`
- `validacoes.md`
- `componentes-especiais.md`
