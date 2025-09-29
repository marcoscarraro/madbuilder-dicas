# madbuilder-dicas
Anotações, lembretes, códigos do madbuilder.com.br

Aplicar uma máscara em um campo específico do formulário, adicionar dentro do onShow()
TEntry::changeMask(self::$formName, 'chave_acesso', 'AAAA-AAAA');

Fazer algum ajuste de CSS em algum elemento do formulário, adicionar dentro do onShow()
TPage::register_css('upperCaseChaveAcesso','input[name="chave_acesso"]{ text-transform: uppercase; }');


Fazer ajuste de CSS global
$class = _CLASS_;
$css = "
    div[page-name='{$class}'] .card-header.panel-heading {
        position: sticky;
        top: 0;
        z-index: 1000;
    }
";
parent::register_css("my_" . _CLASS_, $css);



Executar algum script jquery
TScript::create("  window.open('{$page}', '_blank'); ");
TScript::create("
            $('#{$idContainer}').parent().css('margin', '0');
            $('#{$idContainer}').parent().parent().css('padding', 0);
            $('#{$idContainer} .tab-pane').css('padding', 0);
        ", true, 10);

Fazer o post de um formulário 
TApplication::postData('form_interaction', 'FormInteractionsView', 'onView');

Carregar algum conteúdo de alguma página do sistema via jquery
TScript::create("__adianti_load_page('engine.php?class=ProcessoForm&method=onReloadPoloProcessual&static=1');");
TScript::create( "$('#content_polo_processual').load('engine.php?class=ProcessoForm&method=onReloadPoloProcessual #content_polo_processual');");

Relacionamento de objetos com o banco de dados
Exemplo: Telefone tem uma coluna com o ID da Pessoa, logo eu posso acessar as propriedades das pessoas a partir do telefone
$telefone->pessoa->nome_completo;
A relação só ocorre quando a chave estrangeira estiver na tabela, por exemplo eu não consigo acessar o telefone da pessoa iniciando pelo objeto de pessoa. $pessoa->telefone->numero;
