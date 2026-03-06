# 🔌 AJAX e Funções JS – MadBuilder / Adianti (Padrão Oficial)

Este arquivo reúne **snippets reais e úteis** para chamadas JavaScript utilizando os **helpers nativos do Adianti / MadBuilder**.

Essas funções permitem:

- Executar métodos PHP
- Atualizar partes da tela
- Enviar dados via AJAX
- Mostrar feedback visual
- Controlar navegação
- Executar tarefas em background

Tudo **sem recarregar a página**.

---

# 🔄 POST em background (sem exibir retorno)

Este padrão é muito usado para:

- polling
- consulta de status
- verificação de pagamentos
- execução silenciosa

## Exemplo: polling em background

```php
$formName = self::$formName;

TScript::create("
    window.mp_polling = setInterval(function() {
        __adianti_post_exec(
            'class=VendaIngressoPublicEtapa4form&method=onConsultaPagamento',
            $('#{$formName}').serialize(),
            null,
            null,
            true
        );
    }, {$interval});
");
```

### Observações

- Os dados do formulário são enviados via **POST**
- Nada é exibido em tela
- Executa totalmente em **background**
- Ideal para tarefas recorrentes

---

# 📥 Executar método AJAX com retorno automático em tela

Executa um método PHP e **renderiza automaticamente o retorno na tela**.

Isso é controlado pelo último parâmetro `true`.

```php
TScript::create("
    __adianti_ajax_exec(
        'class=VendaIngressoPublicEtapa2form&method=onPopularTfield',
        null,
        true
    );
");
```

### Quando usar

- Atualizar campos
- Popular componentes
- Recarregar partes da tela
- Executar lógica sem submit

---

# 📤 Submit de formulário (padrão Adianti)

Executa o submit do formulário usando o fluxo padrão do framework.

```php
$formName = self::$formName;

TScript::create("
    __adianti_post_data(
        '{$formName}',
        'class=VendaIngressoPublicEtapa2form&method=onPopularTfield'
    );
");
```

### Vantagens

- Respeita validações
- Usa o ciclo padrão do formulário
- Ideal para ações que dependem do estado do form

---

# 🌐 Buscar página ou endpoint via GET

Executa requisição GET para uma classe do Adianti.

Útil para:

- carregar conteúdo parcial
- consumir endpoints internos
- montar componentes dinâmicos

```javascript
__adianti_get_page(
    'class=ProdutoService&method=getLista',
    function(data){
        console.log(data);
    }
);
```

Com parâmetros:

```javascript
__adianti_get_page(
    'class=ProdutoService&method=getLista',
    function(data){
        console.log(data);
    },
    { categoria: 10 }
);
```

---

# 🔍 Lookup AJAX (busca dinâmica)

Muito usado em campos como:

- autocomplete
- busca por código
- relacionamento entre tabelas

```javascript
__adianti_ajax_lookup(
    'class=ClienteService&method=onSearch',
    document.getElementById('cliente_id')
);
```

O framework envia automaticamente:

```
key = valor digitado
ajax_lookup = 1
```

---

# 🔎 Lookup com envio de formulário

Versão mais completa do lookup.

Envia:

- formulário
- campo
- valor digitado
- metadados do campo

```javascript
__adianti_post_lookup(
    'form_cliente',
    'class=ClienteService&method=onSearch',
    'cliente_id'
);
```

Muito usado em:

- `TDBSeekButton`
- relacionamentos entre entidades

---

# ⏳ Bloquear interface (Loading)

Exibe loading na interface.

```javascript
__adianti_block_ui('Processando...');
```

Resultado:

- spinner
- bloqueio de interação
- mensagem personalizada

---

# 🔓 Desbloquear interface

Remove o bloqueio de UI.

```javascript
__adianti_unblock_ui();
```

Forçando desbloqueio:

```javascript
__adianti_unblock_ui(true);
```

---

# 🔔 Toast Notification

Mostra notificações rápidas na tela.

```javascript
__adianti_show_toast(
    'success',
    'Registro salvo com sucesso',
    'topRight',
    'fas fa-check'
);
```

Tipos comuns:

```
success
error
warning
info
```

---

# ❌ Exibir erro padrão

Mostra mensagem de erro.

```javascript
__adianti_error(
    'Erro',
    'Falha ao processar a requisição'
);
```

---

# 💬 Mensagem informativa

Exibe mensagem simples.

```javascript
__adianti_message(
    'Aviso',
    'Operação concluída'
);
```

---

# ❓ Diálogo de confirmação

Usado para ações críticas.

Exemplo:

- excluir
- cancelar
- finalizar operação

```javascript
__adianti_question(
    'Confirmação',
    'Deseja excluir este registro?',
    function(){
        console.log('SIM');
    },
    function(){
        console.log('NÃO');
    },
    'Sim',
    'Cancelar'
);
```

---

# 📥 Download de arquivo

Permite baixar arquivos gerados no backend.

```javascript
__adianti_download_file('tmp/relatorio.pdf');
```

Definindo nome do arquivo:

```javascript
__adianti_download_file(
    'tmp/relatorio.pdf',
    'relatorio_vendas'
);
```

---

# 🌍 Abrir página em nova aba

```javascript
__adianti_open_page(
    'index.php?class=RelatorioVendas'
);
```

---

# 🔁 Redirecionar página

Redirecionamento simples.

```javascript
__adianti_goto_page(
    'index.php?class=Dashboard'
);
```

---

# ⚠️ Diferença entre os métodos principais

| Método | Uso principal |
|------|-------------|
| `__adianti_post_exec` | POST em background |
| `__adianti_ajax_exec` | Executar método com retorno visual |
| `__adianti_post_data` | Submit padrão de formulário |
| `__adianti_get_page` | Buscar conteúdo via GET |
| `__adianti_ajax_lookup` | Lookup dinâmico |
| `__adianti_post_lookup` | Lookup com formulário |

---

# 🧠 Boas práticas

- Prefira sempre os **helpers nativos do Adianti**
- Use **polling com cuidado**
- Sempre limpe `setInterval`
- Evite múltiplas chamadas AJAX simultâneas
- Bloqueie UI em operações longas
- Use toast para feedback rápido

---

# 📎 Observação final

Este arquivo documenta o **padrão oficial de JavaScript no MadBuilder / Adianti**.

Veja também:

```
carregamento-dinamico.md
redirecionamento.md
formularios.md
html-renderer.md
```

Esses arquivos complementam o uso das funções JS dentro do framework.

--- 

🚀 **Objetivo deste documento**

Padronizar o uso de **AJAX, feedback visual e navegação dinâmica**
nos projetos baseados em **MadBuilder / Adianti**.

Isso reduz:

- código duplicado
- inconsistências
- erros de implementação
- tempo de desenvolvimento.
