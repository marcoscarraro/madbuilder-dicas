# 📚 MadBuilder – Dicas & Snippets

Este repositório reúne **snippets práticos, exemplos reais e boas práticas**
para desenvolvimento com **MadBuilder Framework**, baseado no **Adianti 7.5**.

O objetivo é servir como **referência rápida**, focada em soluções do dia a dia,
evitando exemplos genéricos e priorizando código usado em produção.

---

## Sumário / Índice (detalhado)

- [README.md](README.md) — Índice principal.

#### PHP (doc/snippets/php/)
- [formularios.md](doc/snippets/php/formularios.md) — Snippets para criação e tratamento de formulários, TForm, envio e recebimento de dados.
- [banco-de-dados.md](doc/snippets/php/banco-de-dados.md) — Boas práticas com TTransaction, Active Record, exemplos de consultas e relacionamentos.
- [mensagens-e-redirecionamento.md](doc/snippets/php/mensagens-e-redirecionamento.md) — Uso de TMessage, TToast, redirecionamentos e notificações ao usuário.
- [validacoes.md](doc/snippets/php/validacoes.md) — Validadores nativos (TRequiredValidator, TRequiredListValidator) e exemplos de validação personalizada.
- [debug.md](doc/snippets/php/debug.md) — Ferramentas e funções de debug (mad_dump, md, mdd) e dicas para debugar no MadBuilder.
- [sessao-e-ambiente.md](doc/snippets/php/sessao-e-ambiente.md) — Uso de TSession, gerenciamento de ambientes e variáveis de configuração.
- [belement-html.md](doc/snippets/php/belement-html.md) — Exemplo de uso de BElement com HTML, renderização dinâmica e templates embutidos.
- [listagens-e-kanban.md](doc/snippets/php/listagens-e-kanban.md) — Filtros em listagens e Kanban, aplicar filtros dinâmicos e refresh de componentes.
- [tcombo-para-switch.md](doc/snippets/php/tcombo-para-switch.md) — Como converter um TCombo para um controle tipo switch.
- [tdaterange-opcoes.md](doc/snippets/php/tdaterange-opcoes.md) — Configurações e limitações do TDateRange.
- [upload-tratamento-arquivos.md](doc/snippets/php/upload-tratamento-arquivos.md) — Exemplo de upload, validação e tratamento de arquivos enviados.
- [qrcode.md](doc/snippets/php/qrcode.md) — Geração de QRCode e integrações úteis.
- [filtro-default-listagem.md](doc/snippets/php/filtro-default-listagem.md) - Aplicar filtro default em uma listagem sem confliar com os filtros disponíveis na listagem.

#### JavaScript / jQuery (doc/snippets/js/)
- [scripts-jquery.md](doc/snippets/js/scripts-jquery.md) — Exemplos de uso de jQuery em views MadBuilder, manipulação do DOM e eventos.
- [funcoes-nativas-js.md](doc/snippets/js/funcoes-nativas-js.md) — Funções JS recomendadas para trabalhar com Adianti/MadBuilder (atalhos e helpers).
- [carregamento-dinamico.md](doc/snippets/js/carregamento-dinamico.md) — Técnicas de carregamento dinâmico (jQuery .load, __adianti_load_page) e atualizações parciais.
- [redirecionamento.md](doc/snippets/js/redirecionamento.md) — Estratégias de redirecionamento via JS, timeouts e abertura de novas janelas.

#### CSS (doc/snippets/css/)
- [estilos-inline.md](doc/snippets/css/estilos-inline.md) — Como aplicar estilos inline a elementos específicos via TPage::register_css e propriedades de componentes.
- [estilos-globais.md](doc/snippets/css/estilos-globais.md) — Padrões e classes globais para o layout, sticky headers e ajustes de responsividade.

#### SQL (doc/snippets/sql/)
- [consultas-e-filtros.md](doc/snippets/sql/consultas-e-filtros.md) — Exemplos de queries, filtros com Active Record e boas práticas.
- [logs-sql.md](doc/snippets/sql/logs-sql.md) — Habilitar logs de SQL com TTransaction::setLogger e análise de queries.
- [model-atributos-virtuais.md](doc/snippets/sql/model-atributos-virtuais.md) — Como criar e usar atributos virtuais em models.
- [update-com-where.md](doc/snippets/sql/update-com-where.md) - Update em massa em registros usando where.

#### HTML / Templates (doc/snippets/html/)
- [htmlrenderer-multiplas-secoes.md](doc/snippets/html/htmlrenderer-multiplas-secoes.md) — Uso de HTMLRenderer para múltiplas seções dentro de um template.
- [modal-com-formulario.md](doc/snippets/html/modal-com-formulario.md) — Inserir formulários dentro de modais e submeter via JS/PHP.
- [twindow-html.md](doc/snippets/html/twindow-html.md) — Exemplo de criação de TWindow que renderiza HTML dinâmico.
- [template-html-banco-dados.md](doc/snippets/html/template-html-banco-dados.md) — Armazenar templates HTML no banco e renderizar nas views.

#### Infra / Ambiente (doc/snippets/infra/)
- [ios-http2-nginx.md](doc/snippets/infra/ios-http2-nginx.md) — Ajustes de Nginx / HTTP2 para compatibilidade com iOS e resolução de erros de conexão.
- [single-page-sem-parametros.md](doc/snippets/infra/single-page-sem-parametros.md) — Estratégias para aplicações single-page sem exibir parâmetros na URL.

---

## 🔗 Documentação Oficial

- MadBuilder:  
  👉 https://docs-fw.madbuilder.com.br/

- Adianti Framework:  
  👉 https://www.adianti.com.br/framework

---

## 📄 Licença

Este repositório é mantido para **estudo, referência e produtividade**.

O uso em produção deve respeitar a licença do **MadBuilder Framework** e
dependências associadas.

---

💡 *Dica:*  
Se você trabalha com MadBuilder no dia a dia, este repositório foi feito
para ser seu **atalho mental**.
