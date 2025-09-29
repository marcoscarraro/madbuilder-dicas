# MadBuilder Dicas e Anotações  

📚 Documentação oficial: [docs-fw.madbuilder.com.br](http://docs-fw.madbuilder.com.br/)  

Este repositório contém **exemplos, lembretes e truques práticos** para o uso do **MadBuilder Framework** (baseado no Adianti).  
A ideia é facilitar o dia a dia com snippets prontos para formulários, banco de dados, CSS, jQuery, mensagens e debugging.  

---

### Aplicar máscara em um campo específico  
```php
TEntry::changeMask(self::$formName, 'chave_acesso', 'AAAA-AAAA');
```

### Alterar estilo (CSS) de um campo específico
```php
TPage::register_css(
    'upperCaseChaveAcesso',
    'input[name="chave_acesso"]{ text-transform: uppercase; }'
);
```

### Alterar estilo (CSS) global da página
```php
$class = _CLASS_;
$css = "
    div[page-name='{$class}'] .card-header.panel-heading {
        position: sticky;
        top: 0;
        z-index: 1000;
    }
";
parent::register_css("my_" . _CLASS_, $css);
```

### Executar script com jQuery usando TScript
```php
TScript::create("window.open('{$page}', '_blank');");
```
```php
TScript::create("
    $('#{$idContainer}').parent().css('margin', '0');
    $('#{$idContainer}').parent().parent().css('padding', 0);
    $('#{$idContainer} .tab-pane').css('padding', 0);
", true, 10);
```

### Carregar conteúdo dinamicamente via jQuery
```php
TScript::create("__adianti_load_page('engine.php?class=ProcessoForm&method=onReloadPoloProcessual&static=1');");
```
```php
TScript::create("
    $('#content_polo_processual').load(
        'engine.php?class=ProcessoForm&method=onReloadPoloProcessual #content_polo_processual'
    );
");
```

### Fazer post de formulário via TApplication
```php
//TApplication::postData($formName, $class, $method = NULL, $parameters = NULL);
TApplication::postData('form_interaction', 'FormInteractionsView', 'onView');
```

### Relacionamento de Objetos
Se Telefone tem uma coluna com o id_pessoa:
```php
echo $telefone->pessoa->nome_completo;
```
⚠️ A relação só ocorre se a chave estrangeira existir na tabela.
Exemplo: não é possível acessar os telefones a partir de Pessoa se a FK estiver apenas em Telefone:
```php
// Isso NÃO funciona
echo $pessoa->telefone->numero;
```

### Exibir mensagem para o usuário
```php
new TMessage('info', 'Operação realizada com sucesso!');
new TMessage('error', 'Ocorreu um erro durante o processo.');
```

### Redirecionar para outra página
```php
AdiantiCoreApplication::loadPage('NomeDaClasseView');
```

### Sessões
```php
TSession::setValue('usuario_logado', $usuario);
$usuario = TSession::getValue('usuario_logado');
TSession::freeSession(); // limpar sessão
```

### 🐞 Debug
O MadBuilder possui funções de debug nativas:
```php
mad_dump($variavel); // imprime formatado e continua execução
md($variavel);       // atalho para mad_dump
mdd($variavel);      // imprime formatado e interrompe execução
```
