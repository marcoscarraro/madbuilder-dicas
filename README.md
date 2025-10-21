# MadBuilder Dicas e Anotações  

📚 Documentação oficial: [docs-fw.madbuilder.com.br](http://docs-fw.madbuilder.com.br/)  

Este repositório contém **exemplos, lembretes e truques práticos** para o uso do **MadBuilder Framework** (baseado no Adianti).  
A ideia é facilitar o dia a dia com snippets prontos para formulários, banco de dados, CSS, jQuery, mensagens e debugging.  

---

## Blocos de Personalização do MadBuilder  

O MadBuilder gera arquivos de **Formulário** e **Listagem (Datagrid)** com blocos especiais de comentário.  
Esses blocos servem para inserir código sem que ele seja sobrescrito quando o Builder regenerar o arquivo.  

## Blocos do Form

### ```<fileHeader>```
Usado no topo do arquivo. Serve para imports, namespaces, require_once e includes extras.

### ```<classProperties>```
Fica dentro da classe, antes dos métodos. Permite declarar variáveis e propriedades personalizadas.

### ```<onBeginPageCreation>```
Executado no início da criação da página. Bom lugar para inicializações, verificações de permissão e configurações globais.

### ```<onBeforeAddFieldsToForm>```
Chamado antes de adicionar os campos ao formulário. Útil para manipular dinamicamente campos ou aplicar configurações globais.

### ```<onAfterFieldsCreation>```
Executado após todos os campos do formulário serem criados. Ideal para máscaras, validações, CSS e eventos JS.

### ```<onAfterPageCreation>```
Executado no final da criação da página. Usado para ajustes finais de layout, CSS global ou scripts extras.

### ```<onSave>```
Contém o código da ação Salvar (gerado automaticamente). Dentro dele existem sub-blocos como beforeStoreAutoCode, afterStoreAutoCode, fieldList-..., messageAutoCode, etc. Esses pontos permitem inserir código antes e depois da gravação no banco.

### ```<onEdit>```
Contém o código da ação Editar (gerado automaticamente). Possui sub-blocos específicos para carregar dados mestre-detalhe (fieldList-...).

### ```<onFormClear>```
Executado quando o formulário é limpo (onClear). Bom para resetar variáveis adicionais ou limpar sessões.

### ```<onShow>```
Executado quando o formulário é exibido. Bom lugar para scripts ou carregamentos dinâmicos.

### ```<userCustomFunctions>```
Fica no final da classe. Usado para declarar funções personalizadas criadas pelo desenvolvedor.

---

## Blocos do List (Datagrid)

### ```<fileHeader>```
Usado no topo do arquivo. Serve para imports, namespaces, require_once.

### ```<classProperties>```
Dentro da classe, antes dos métodos. Para propriedades adicionais.

### ```<onBeginPageCreation>```
Executado no início da criação da página. Usado para setup inicial, permissões ou configurações globais.

### ```<onBeforeAddFieldsToForm>```
Executado antes de adicionar os campos do filtro.

### ```<onAfterFieldsCreation>```
Executado após a criação dos campos de filtro. Bom para aplicar máscaras, validações ou eventos.

### ```<onBeforeColumnsCreation>```
Chamado antes de criar as colunas da grid.

### ```<onAfterColumnsCreation>```
Chamado depois de criar as colunas da grid.

### ```<onAfterActionsCreation>```
Executado após criar as ações principais (editar, excluir).

### ```<onAfterHeaderActionsCreation>```
Executado após criar as ações de cabeçalho (cadastrar, exportar, etc).

### ```<onAfterPageCreation>```
Chamado no final da criação da página.

### ```onDelete```
Contém o código da ação Excluir (gerado automaticamente).

### ```<onBeforeDatagridSearch>```
Executado antes de aplicar os filtros de busca.

### ```<onDatagridSearch>```
Executado durante a aplicação da busca.

### ```<onBeforeDatagridLoad>```
Executado antes de carregar os registros no banco.

### ```<onBeforeDatagridAddItem>```
Executado antes de adicionar cada item (linha) na grid.

### ```<onAfterDatagridAddItem>```
Executado após adicionar cada item na grid. Bom para estilizar linhas, adicionar ícones, badges, etc.

### ```<onBeforeDatagridTransactionClose>```
Executado antes de fechar a transação de carregamento.

### ```<onShow>```
Executado quando a listagem é exibida.

### ```<userCustomFunctions>```
Fica no final da classe. Usado para criar funções personalizadas do desenvolvedor.

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
Redirecionamento com timeout:
```php
TScript::create("setTimeout(() => {window.location.replace('".DevUtils::domainApp()."showpdf.php?pdf=".$param['pdfContratoAssinado']."');});");
```
Recarregar página ou conteúdo:
```php
TScript::create("location.reload();");
```
Recarregar conteúdo específico de um datagrid:
```php
TScript::create("$(document).ready(function(){
  window.setTimeout(function(){
    __adianti_load_page('engine.php?class=BateriaSimpleList&method=onRefresh&target_container=b66f15f3f5c62f');
  }, 5000);
});");
```
Recarregar outra view:
```php
TScript::create("$(document).ready(function(){
  window.setTimeout(function(){
    __adianti_load_page('engine.php?class=AtividadesKanbanView&method=onShow');
  }, 0);
});");
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

### Exibir mensagem usando Toasts
```php
TToast::show('success', "Registro apagado", 'topRight', 'far:check-circle');
TToast::show('success', "Registro salvo", 'topRight', 'far:check-circle');
```


### Redirecionar para outra página
```php
AdiantiCoreApplication::loadPage('NomeDaClasseView');
TApplication::gotoPage('ContratoAssinadoForm', 'onShow', ['pdf' => 'testeabc.pdf']);

```

### Sessões
```php
TSession::setValue('usuario_logado', $usuario);
$usuario = TSession::getValue('usuario_logado');
TSession::freeSession(); // limpar sessão
```

### Valores do formulário vêm pelo $param como array associativo
```php
$param['curso_id'];
```

### Enviar dados para formulário
```php
$objetoFormulario = new stdClass();
$objetoFormulario->data_termino = $dataAula->format('d/m/Y');
TForm::sendData(self::$formName, $objetoFormulario);
```

### Interromper script e exibir erro:
```php
throw new Exception('O instrutor não está disponível neste período');
```

### Formatar valor monetário brasileiro:
```php
number_format($valorCurso->cursoTurmaValor($param['lead_negociacao_lead_turma_id']), 2, ',', '.');
```

### Converter valor monetário padrão US para salvar no banco (double):
```php
(double) str_replace(',', '.', str_replace('.', '', $param['valor_pago']));
```

### Fechar janela:
```php
TWindow::closeWindow();
```

### Fechar cortina lateral:
```php
TScript::create("Template.closeRightPanel()");
```

### Redirecionar para um site externo
```php
TScript::create('window.location.href = "https://www.trisistemas.com.br/";');
```

### Validações
```php
(new TRequiredValidator)->validate('Forma de Pagamento', $param['tipo_pagamento_id']);
(new TRequiredValidator)->validate('Parcelas', $param['parcelas']);
(new TRequiredValidator)->validate('Data dos Vencimentos', $param['data_vencimentos']);
```

### 🐞 Debug
O MadBuilder possui funções de debug nativas:
```php
mad_dump($variavel); // imprime formatado e continua execução
md($variavel);       // atalho para mad_dump
mdd($variavel);      // imprime formatado e interrompe execução
```

---

## Interações com o Banco de Dados

### 📌 Logs de SQL
Mostra em tela os logs do SQL. Colocar logo abaixo do `TTransaction::open(self::$database);`
```php
TTransaction::setLogger(new TLoggerSTD());
```

### Abrir/fechar conexão com o banco:
```php
TTransaction::open(self::$database); // Inicia conexão
TTransaction::close();               // Fecha conexão
TTransaction::open(MAIN_DATABASE);   // Conecta em outro banco sem usar nome específico
```

### Gravar registro:
```php
TTransaction::open(self::$database);
$objetoConteudoAula = new TurmaConteudos();
$objetoConteudoAula->turma = "Turma XPTO";
$objetoConteudoAula->store();
TTransaction::close();
```

### Atualizar registro (informando ID):
```php
TTransaction::open(self::$database);
$objetoConteudoAula = new TurmaConteudos();
$objetoConteudoAula->id = 1;
$objetoConteudoAula->turma = "Turma XPTO Atualizada";
$objetoConteudoAula->store();
TTransaction::close();
```

### Procurar por chave primária (ID):
```php
Curso::find($cursoId);
```

### Procurar por outra coluna:
```php
Calendario::where('data_calendario','=', $dataAula->format('Y/m/d'))->load();
```

### Especificar as colunas
```php
Calendario::select('data,ano')where('data_calendario','=', $dataAula->format('Y/m/d'))->load();
```

### Opções adicionais:
```php
->load() // Carrega todos os registros
->first() // Carrega o primeiro registro
->last() // Carrega o último registro
->orderBy('ordem', 'asc') // Ordena por coluna
```

### Contagem de registros
```php
$matriculados = Matricula::where('turma_id','=',$data->turma_id)->count();
```


---
## Lembretes de sintaxe / operadores
```bash
$a == $b    // Igual
$a != $b    // Diferente
$a < $b     // Menor que
$a <= $b    // Menor ou igual
$a > $b     // Maior que
$a >= $b    // Maior ou igual
$valortotal += $contas["valor"]; // Incrementa
$linha .= $conteudo; // Concatena
ASC // Ordem crescente
DESC // Ordem decrescente
```
