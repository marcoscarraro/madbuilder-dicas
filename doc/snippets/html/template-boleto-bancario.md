# 🧾 Template HTML – Boleto Bancário  
(MadBuilder / Adianti)

Este snippet apresenta um **template HTML completo de boleto bancário**, estruturado com **todos os campos normalmente exigidos pela FEBRABAN**, compatível com o **AdiantiHTMLDocumentParser**.

Ele foi pensado para:

- geração de PDF
- substituição automática de tags `{$variavel}`
- uso com dados vindos diretamente do banco
- relatórios financeiros e emissão de boletos via gateway

⚠️ **Este template mantém a estrutura exigida**, porém **os dados reais (linha digitável, código de barras, nosso número, etc.) devem ser fornecidos por um banco ou gateway homologado**.

---

## 🎯 Objetivo do template

- Servir como **base visual e estrutural de boleto bancário**
- Atender aos **campos obrigatórios definidos pela FEBRABAN**
- Permitir substituição dinâmica de dados via `setMaster()`
- Facilitar geração de PDF a partir de HTML
- Centralizar o layout do boleto fora do código PHP

---

## 🧱 Template HTML (estrutura FEBRABAN)

> ⚠️ **Altere com cuidado o HTML**  
> Ele contém:
> - Cabeçalho do beneficiário  
> - Linha digitável  
> - Dados do título  
> - Dados do pagador  
> - Instruções  
> - Recibo do pagador  
> - Ficha de compensação  
> - Área de código de barras  

```html
<html charset="UTF-8">

<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="robots" content="noindex" />
</head>

<body style="background-color: #fff; margin-right: 0; font-family: 'Arial';">
    <div class="document" style="margin: auto auto; width: 190mm; height: auto; background-color: #fff;">
        <br />
        <table style="border-collapse: collapse; width: 100%; border-style: none; margin-left: auto; margin-right: auto; height: 1178px;">
            <tbody>
                <tr style="height: 200px;">
                    <td style="width: 100%; height: 200px; text-align: left; vertical-align: top;">
                        <table style="border-collapse: collapse; width: 100%; height: 188px; border-color: #000000; border-style: solid;" border="1">
                            <tbody>
                                <tr style="height: 22px;">
                                    <td style="width: 24.2532%; height: 88px; border-color: #000000; border-style: solid; text-align: center; vertical-align: middle;" rowspan="4"><img src="{$cobranca->escola->logotipo}" alt="logo" height="88px" /></td>
                                    <td style="width: 75.7468%; height: 22px; border-style: none; vertical-align: middle;"><span style="font-size: 10pt;"><strong>Razão Social:</strong> {$cobranca->escola->razao_social}</span></td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 75.7468%; height: 22px; border-style: none; vertical-align: middle;"><span style="font-size: 10pt;"><strong>Endereço:</strong> {$cobranca->escola->cep->logradouro}, {$cobranca->escola->endereco_numero}, {$cobranca->escola->endereco_complemento}, {$cobranca->escola->cep->bairro}, {$cobranca->escola->cep->cidade}-{$cobranca->escola->cep->uf}<br /></span></td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 75.7468%; height: 22px; border-style: none; vertical-align: middle;"><span style="font-size: 10pt;"><strong>E-mail:</strong> {$cobranca->escola->email_financeiro}</span></td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 75.7468%; height: 22px; border-style: none; vertical-align: middle;"><span style="font-size: 10pt;"><strong>Telefone:</strong> {$cobranca->escola->telefone_financeiro}</span></td>
                                </tr>
                                <tr style="height: 94px;">
                                    <td style="width: 100%; height: 100px; border-color: #000000; border-style: solid; vertical-align: top;" colspan="2">
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Curso: {$cobranca->matricula->curso->curso}</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Data de Início: {$cobranca->matricula->turma->data_inicio}</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Aluno: {$cobranca->matricula->cliente->nome}</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Matrícula: {$cobranca->matricula_id}</span></p>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                        <p style="margin: 1px 0;"> </p>
                    </td>
                </tr>
                <tr style="height: 330px;">
                    <td style="width: 100%; height: 330px; text-align: left; vertical-align: top;">
                        <table style="border-collapse: collapse; width: 100%; border-color: #000000; border-style: solid;" border="1" cellpadding="5">
                            <tbody>
                                <tr>
                                    <td style="width: 100%;" colspan="5">
                                        <table style="border-collapse: collapse; width: 100%; border-style: hidden; margin: -5px;" cellpadding="5">
                                            <tbody>
                                                <tr>
                                                    <td style="width: 20.3504%; border-right-style: solid; border-right-width: 1px; border-right-color: #000000; text-align: center; vertical-align: middle;"><img src="{$cobranca->tipo_pagamento->gateway_pagamento->logo_banco}" alt="logo do banco" height="40px" /></td>
                                                    <td style="width: 11.7561%; text-align: center; vertical-align: middle; border-right-style: solid; border-right-width: 1px; border-right-color: #000000;"><strong>{$cobranca->tipo_pagamento->gateway_pagamento->cod_banco}</strong></td>
                                                    <td style="width: 71.4909%; text-align: right; vertical-align: middle;">{$linha_digitavel}</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </td>
                                </tr>
                                <tr>
                                    <td style="width: 100%; border-color: #000000; border-style: solid; vertical-align: top;" colspan="5">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Beneficiário:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->escola->razao_social} - CNPJ {$cobranca->escola->cnpj}</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->escola->cep->logradouro}, {$cobranca->escola->endereco_complemento}, {$cobranca->escola->cep->bairro}, {$cobranca->escola->cep->cidade}-{$cobranca->escola->cep->uf}</span></p>
                                    </td>
                                </tr>
                                <tr>
                                    <td style="width: 18.8706%; border-color: #000000; border-style: solid; vertical-align: top;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Nº Documento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$numero_documento_remessa}</span></p>
                                    </td>
                                    <td style="width: 22.9957%; border-color: #000000; border-style: solid; vertical-align: top;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Nosso Número:</span></strong></p>
                                        <p style="margin: 1px 0;">{$nosso_numero}</p>
                                    </td>
                                    <td style="width: 20.1337%; border-color: #000000; border-style: solid; vertical-align: top;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Data de Processamento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$data_geracao}</span></p>
                                    </td>
                                    <td style="width: 18%; border-color: #000000; border-style: solid; vertical-align: top;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Vencimento:</span></strong></p>
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 12pt;">{$cobranca->data_vencimento}</span></strong></p>
                                    </td>
                                    <td style="width: 20%; border-color: #000000; border-style: solid; vertical-align: top;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Valor:</span></strong></p>
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 12pt;">{$valor_cobranca}</span></strong></p>
                                    </td>
                                </tr>
                                <tr>
                                    <td style="width: 100%; border-color: #000000; border-style: solid; vertical-align: top;" colspan="5">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Instruções:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Não aceitar após vencimento</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Não aceitar pagamento com Cheque</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Multa de {$cobranca->tipo_pagamento->juros_multa}% por atraso e juros de {$cobranca->tipo_pagamento->juros_atraso}% ao dia.</span></p>
                                    </td>
                                </tr>
                                <tr>
                                    <td style="width: 100%; vertical-align: top;" colspan="5">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Pagador:</span></strong></p>
                                        <p style="margin: 1px 0;">{$cobranca->cliente->nome} - {$cobranca->cliente->cpf_cnpj}</p>
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Endereço:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->cliente->cep->logradouro}, {$cobranca->cliente->endereco_numero} {$cobranca->cliente->endereco_complemento}, {$cobranca->cliente->cep->bairro}, {$cobranca->cliente->cep->cidade}-{$cobranca->cliente->cep->uf} CEP {$cobranca->cliente->cep->cep}</span></p>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                        <p style="margin: 1px 0px; text-align: right;"><span style="font-size: 8pt;">Autenticação mecânica</span></p>
                        <p style="margin: 1px 0px; text-align: right;"><span style="font-size: 8pt;">Recibo do pagador</span></p>
                    </td>
                </tr>
                <tr style="height: 35px;">
                    <td style="width: 100%; height: 35px; text-align: center; vertical-align: middle;">
                        <p style="margin: 1px 0px; text-align: right; border-top: 1px #000 dashed;"><span style="font-size: 8pt;">Corte aqui</span></p>
                    </td>
                </tr>
                <tr style="height: 340px;">
                    <td style="width: 100%; height: 340px; vertical-align: top;">
                        <table style="border-collapse: collapse; width: 100%; height: 354px; border-color: #000000; border-style: solid;" border="1" cellpadding="5">
                            <tbody>
                                <tr>
                                    <td style="width: 100%;" colspan="6">
                                        <table style="border-collapse: collapse; width: 100%; border-style: hidden; margin: -5px;" cellpadding="5">
                                            <tbody>
                                                <tr>
                                                    <td style="width: 20.3504%; border-right-style: solid; border-right-width: 1px; border-right-color: #000000; text-align: center; vertical-align: middle;"><img src="{$cobranca->tipo_pagamento->gateway_pagamento->logo_banco}" alt="logo do banco" height="40px" /></td>
                                                    <td style="width: 11.7561%; text-align: center; vertical-align: middle; border-right-style: solid; border-right-width: 1px; border-right-color: #000000;"><span style="font-size: 14pt;">{$cobranca->tipo_pagamento->gateway_pagamento->cod_banco}</span></td>
                                                    <td style="width: 71.4909%; text-align: right; vertical-align: middle;">{$linha_digitavel}</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="height: 22px; width: 76.5294%; vertical-align: top; border-color: #000000; border-style: solid;" colspan="5">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Local do pagamento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Pagável no banco</span></p>
                                    </td>
                                    <td style="width: 23.3286%; vertical-align: top; height: 22px; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Vencimento:</span></strong></p>
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 12pt;">{$cobranca->data_vencimento}</span></strong></p>
                                    </td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="height: 22px; width: 76.5294%; vertical-align: top; border-color: #000000; border-style: solid;" colspan="5">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Beneficiário:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->escola->razao_social} - CNPJ {$cobranca->escola->cnpj}</span></p>
                                    </td>
                                    <td style="width: 23.3286%; vertical-align: top; height: 22px; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Agência/Código Beneficiário:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->tipo_pagamento->gateway_pagamento->agencia_banco} / {$cobranca->tipo_pagamento->gateway_pagamento->conta_banco}</span></p>
                                    </td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 17.4965%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Data do Documento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$data_geracao}</span></p>
                                    </td>
                                    <td style="width: 14.7938%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Nº Documento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$numero_documento_remessa}</span></p>
                                    </td>
                                    <td style="width: 11.8066%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Espécie doc:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">DM</span></p>
                                    </td>
                                    <td style="width: 11.6643%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Aceite:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">N</span></p>
                                    </td>
                                    <td style="width: 20.7682%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Data de  Processamento:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$data_geracao}</span></p>
                                    </td>
                                    <td style="width: 23.3286%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Nosso número:</span></strong></p>
                                        <p style="margin: 1px 0;">{$nosso_numero}</p>
                                    </td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 17.4965%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Uso do Banco:</span></strong></p>
                                        <p style="margin: 1px 0;"> </p>
                                    </td>
                                    <td style="width: 14.7938%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Carteira:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->tipo_pagamento->gateway_pagamento->carteira_banco}</span></p>
                                    </td>
                                    <td style="width: 11.8066%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Moeda:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">R$</span></p>
                                    </td>
                                    <td style="width: 11.6643%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Quantidade:</span></strong></p>
                                        <p style="margin: 1px 0;"> </p>
                                    </td>
                                    <td style="width: 20.7682%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Valor:</span></strong></p>
                                        <p style="margin: 1px 0;"> </p>
                                    </td>
                                    <td style="width: 23.3286%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Valor do Documento:</span></strong></p>
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 12pt;">{$valor_cobranca}</span></strong></p>
                                    </td>
                                </tr>
                                <tr style="height: 22px;">
                                    <td style="width: 76.5294%; border-color: #000000; border-style: solid; vertical-align: top; height: 66px;" colspan="5" rowspan="3">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 10.6667px;">Instruções:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Não aceitar após vencimento</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Não aceitar pagamento com Cheque</span></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">Multa de {$cobranca->tipo_pagamento->juros_multa}% por atraso e juros de {$cobranca->tipo_pagamento->juros_atraso}% ao dia.</span></p>
                                    </td>
                                    <td style="width: 23.3286%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">(-) Descontos/Abatimento:</span></strong></p>
                                    </td>
                                </tr>
                                <tr style="height: 26px;">
                                    <td style="width: 23.3286%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">(+) Juros/Multa:</span></strong></p>
                                    </td>
                                </tr>
                                <tr style="height: 21px;">
                                    <td style="width: 23.3286%; height: 22px; vertical-align: top; border-color: #000000; border-style: solid;">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">(=) Valor Cobrado:</span></strong></p>
                                    </td>
                                </tr>
                                <tr style="height: 93px;">
                                    <td style="width: 99.858%; border-color: #000000; border-style: solid; vertical-align: top; height: 93px;" colspan="6">
                                        <p style="margin: 1px 0;"><strong><span style="font-size: 8pt;">Pagador:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->cliente->nome} - {$cobranca->cliente->cpf_cnpj}<br /></span><strong><span style="font-size: 8pt;">Endereço:</span></strong></p>
                                        <p style="margin: 1px 0;"><span style="font-size: 12pt;">{$cobranca->cliente->cep->logradouro}, {$cobranca->cliente->endereco_numero} {$cobranca->cliente->endereco_complemento}, {$cobranca->cliente->cep->bairro}, {$cobranca->cliente->cep->cidade}-{$cobranca->cliente->cep->uf} CEP {$cobranca->cliente->cep->cep}</span></p>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                        <table style="border-collapse: collapse; width: 100%;">
                            <tbody>
                                <tr>
                                    <td style="width: 76.0313%; height: 90px; text-align: center; vertical-align: middle;">{$codigoBarrasBoletoPNG}</td>
                                    <td style="width: 23.9687%; text-align: right; vertical-align: top;">
                                        <p style="margin: 1px 0px; text-align: right;"><span style="font-size: 8pt;">Autenticação mecânica</span></p>
                                        <p style="margin: 1px 0px; text-align: right;"><span style="font-size: 8pt;">Ficha de Compensação</span></p>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</body>

</html>
```

---

## 🔁 Uso com AdiantiHTMLDocumentParser

```php
$html = new AdiantiHTMLDocumentParser($arquivo_html);
$html->setMaster($boleto);
$html->process();
$html->saveAsPDF($destino, 'A4', 'portrait');
```

---

## 🏷️ Exemplo de tags utilizadas

```html
{$razao_social}
{$linha_digitavel}
{$valor}
{$data_vencimento}
{$codigo_barras}
```

📌 Todas as tags são resolvidas automaticamente pelo objeto passado no `setMaster()`.

---

## 💡 Boas práticas

- Gere o código de barras via lib própria
- Use QRCode quando aplicável
- Centralize regras financeiras no backend
- Use templates no banco para flexibilidade

---

## 📎 Observação final

Este snippet é uma **base genérica de layout de boleto**.

