# 🧩 Snippets – Model (Atributos e Métodos Customizados)

Este arquivo contém **padrões e exemplos** para criação de **atributos virtuais**
e **métodos customizados dentro de Models** no **MadBuilder / Adianti**.

Esses métodos são ideais para:
- cálculos dinâmicos
- valores derivados do banco
- regras reutilizáveis no sistema

---

## 📌 Método customizado na Model

### Exemplo: gerar próxima matrícula automaticamente

Este método busca a **última matrícula registrada** e retorna:
- o próximo número disponível
- ou `1000` caso ainda não exista nenhuma matrícula

### Implementação na Model

```php
public function get_proxima_matricula()
{
    $conn = TTransaction::get();

    $stmt = $conn->prepare('SELECT MAX(matricula) AS ultima FROM matriculas');
    $stmt->execute();
    $row = $stmt->fetch();

    if (!empty($row['ultima'])) {
        return $row['ultima'] + 1;
    }

    return 1000;
}
```

📌 Observações importantes:
- O método **não abre transação**
- Assume que a transação já está aberta
- Segue o padrão de **atributo virtual** (`get_nome_do_atributo`)

---

## 🧠 Convenção de nome (Adianti)

Quando o método segue o padrão:

```php
get_nome_do_atributo()
```

Ele pode ser acessado como **propriedade**, sem parênteses:

```php
$model->nome_do_atributo;
```

---

## ▶️ Como utilizar o método na prática

### Exemplo: preencher campo automaticamente antes do form

Uso típico dentro de:
- `onBeforeAddFieldsToForm`
- `onEdit`
- `onShow`

### Código de uso

```php
// <onBeforeAddFieldsToForm>

// Abre a transação
TTransaction::open(MAIN_DATABASE);

// Instancia a model
$proxima_matricula = new Matriculas();

// Usa o atributo virtual
$matricula->setValue($proxima_matricula->proxima_matricula);

// Fecha a transação
TTransaction::close();
```

📌 Pontos importantes:
- A transação **sempre** deve ser aberta antes
- O método da Model apenas executa a lógica
- Mantém o padrão de separação de responsabilidades

---

## ✅ Boas práticas para métodos em Models

- ✔ Não abrir ou fechar transações dentro da Model
- ✔ Usar métodos pequenos e objetivos
- ✔ Retornar valores simples (int, string, bool, array)
- ✔ Evitar regras de negócio complexas (use Service)
- ✔ Nomear métodos de forma clara e semântica

---

## ⚠️ Atenção

Se o método precisar:
- múltiplas queries
- regras complexas
- integrações externas

👉 **Use uma classe Service**, e não a Model.

---

## 📎 Quando usar este padrão

Use métodos na Model quando:
- o dado depende diretamente da tabela
- o cálculo é simples
- o valor é reutilizável em vários pontos do sistema

Esse padrão mantém o código:
- limpo
- reutilizável
- consistente entre projetos
