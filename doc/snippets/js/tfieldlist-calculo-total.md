# ➕ Cálculo de valores em TFieldList via JavaScript (MadBuilder / Adianti)

Este snippet demonstra um **padrão completo para calcular valores totais**
(soma, duração, preço, quantidade, etc.) a partir de um **TFieldList**
utilizando **JavaScript + AJAX**, integrado ao backend PHP.

O exemplo abaixo calcula a **duração total de disciplinas** selecionadas
em um `TFieldList`.

---

## 🎯 Cenário de uso

- TFieldList com múltiplas linhas
- Cada linha possui um `select`
- O valor total é calculado dinamicamente
- Recalcula automaticamente ao:
  - carregar a página
  - alterar um campo
  - adicionar/remover linhas

---

## 🧠 Estratégia adotada

1. Coletar os valores do TFieldList via JS
2. Enviar todos os IDs em **uma única requisição AJAX**
3. Calcular no backend
4. Retornar JSON
5. Atualizar o campo total no formulário

---

## ⚙️ Código JavaScript (Frontend)

> Inserir no evento `<onAfterPageCreation>`

```php
TScript::create("

    function calcularDuracaoTotal() {
        var disciplinas = [];

        /* Coleta todas as disciplinas selecionadas */
        $('select[name=\"grade_curricular_cursos_disciplinas_id[]\"]').each(function() {
            var id = $(this).val();
            if (id && id !== '') {
                disciplinas.push(id);
            }
        });

        /* Se não houver disciplinas, zera o campo */
        if (disciplinas.length === 0) {
            $('input[name=\"duracao_total_curso\"]').val('00:00');
            return;
        }

        /* Feedback visual */
        $('input[name=\"duracao_total_curso\"]').val('Calculando...');

        /* Requisição AJAX única */
        $.ajax({
            url: 'engine.php?class=CursosForm&method=getDuracaoTotalDisciplinas&static=1',
            type: 'POST',
            dataType: 'json',
            data: {
                disciplinas: disciplinas
            },
            success: function(response) {
                if (response && response.total !== undefined) {
                    $('input[name=\"duracao_total_curso\"]').val(
                        parseFloat(response.total).toFixed(2)
                    );
                } else {
                    $('input[name=\"duracao_total_curso\"]').val('00:00');
                }
            },
            error: function() {
                $('input[name=\"duracao_total_curso\"]').val('00:00');
            }
        });
    }

    /* Recalcula ao carregar a página */
    $(document).ready(function() {
        var duracaoTotal = $('input[name=\"duracao_total_curso\"]').val();
        if (!duracaoTotal || duracaoTotal == '' || duracaoTotal == '0' || duracaoTotal == '00:00') {
            setTimeout(function() {
                calcularDuracaoTotal();
            }, 300);
        }
    });

    /* Recalcula ao alterar qualquer disciplina */
    $(document).on('change', 'select[name=\"grade_curricular_cursos_disciplinas_id[]\"]', function() {
        setTimeout(function() {
            calcularDuracaoTotal();
        }, 300);
    });

    /* Recalcula ao adicionar ou remover linhas do TFieldList */
    $(document).on(
        'click',
        '[onclick*=\"ttable_remove_row\"], [onclick*=\"ttable_clone_previous_row\"]',
        function() {
            setTimeout(function() {
                calcularDuracaoTotal();
            }, 300);
        }
    );

");
```

---

## 🧩 Código PHP (Backend)

> Método responsável por calcular o total e retornar JSON

```php
public static function getDuracaoTotalDisciplinas($param = null)
{
    try
    {
        TTransaction::open(MAIN_DATABASE);

        $total = 0;

        foreach ($param['disciplinas'] as $id) {
            $disciplina = new Disciplinas($id);
            $total += ($disciplina->duracao / 60);
        }

        TTransaction::close();

        echo json_encode(['total' => $total]);
    }
    catch (Exception $e)
    {
        new TMessage('error', $e->getMessage());
    }
}
```

---

## 🔁 Adaptações comuns

Este padrão pode ser facilmente adaptado para:

- 💰 Soma de valores financeiros
- ⏱️ Cálculo de tempo total
- 📦 Quantidade de itens
- 📊 Média ou subtotal
- 🧾 Totais por tipo ou categoria

Basta ajustar:
- o campo coletado no JS
- o cálculo no PHP

---

## 📎 Boas práticas

- Evite múltiplas requisições (use **uma única chamada AJAX**)
- Sempre trate erro no frontend
- Dê feedback visual ao usuário
- Recalcule após clone/remove de linhas
- Use `static=1` para chamadas via `engine.php`

Este snippet é ideal para **TFieldList dinâmicos**.
