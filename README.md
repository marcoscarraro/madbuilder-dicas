# madbuilder-dicas
Anotações, lembretes, códigos do madbuilder.com.br

Aplicar uma máscara em um campo específico do formulário, adicionar dentro do onShow()
TEntry::changeMask(self::$formName, 'chave_acesso', 'AAAA-AAAA');

Fazer algum ajuste de CSS em algum elemento do formulário, adicionar dentro do onShow()
TPage::register_css('upperCaseChaveAcesso','input[name="chave_acesso"]{ text-transform: uppercase; }');
