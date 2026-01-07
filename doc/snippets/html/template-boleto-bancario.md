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
<!DOCTYPE html>
<html charset="UTF-8">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="robots" content="noindex" />
    <style>
        @page {
            size: A4;
            margin: 0;
        }
        body {
            background-color: #fff;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }
        .document {
            width: 210mm;
            min-height: 297mm;
            margin: 0 auto;
            background-color: #fff;
            padding: 10mm;
        }
        table {
            border-collapse: collapse;
            width: 100%;
        }
        .bordered {
            border: 1px solid #000;
        }
        .cell-bordered {
            border: 1px solid #000;
            padding: 3px 5px;
            vertical-align: top;
        }
        .label {
            font-size: 8pt;
            font-weight: bold;
        }
        .value {
            font-size: 12pt;
        }
        .value-bold {
            font-size: 12pt;
            font-weight: bold;
        }
        .text-center {
            text-align: center;
        }
        .text-right {
            text-align: right;
        }
        .text-small {
            font-size: 8pt;
        }
        .linha-corte {
            border-top: 1px dashed #000;
            margin: 10px 0;
            padding-top: 5px;
            text-align: right;
            font-size: 8pt;
        }
        .codigo-barras-container {
            background-color: #fff;
            padding: 5px 0;
            text-align: center;
        }
        .header-logo {
            width: 24%;
            text-align: center;
            vertical-align: middle;
            padding: 5px;
        }
        .header-info {
            width: 76%;
            padding: 5px;
            vertical-align: middle;
        }
        .banco-header {
            border-right: 1px solid #000;
        }
        .aviso-seguranca {
            background-color: #ffffcc;
            padding: 5px;
            margin: 10px 0;
            border: 1px solid #ffcc00;
            font-size: 9pt;
        }
        p {
            margin: 2px 0;
        }
    </style>
</head>
<body>
    <div class="document">
        
        <!-- RECIBO DO PAGADOR (Parte Superior) -->
        <table class="bordered">
            <tr>
                <td class="cell-bordered header-logo" rowspan="4">
                    <img src="{$cobranca->escola->logotipo}" alt="Logo" style="max-height: 88px; max-width: 100%;" />
                </td>
                <td class="cell-bordered header-info">
                    <span class="label">Razão Social:</span> <span class="value">{$cobranca->escola->razao_social}</span>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered header-info">
                    <span class="label">Endereço:</span> <span class="value">{$cobranca->escola->cep->logradouro}, {$cobranca->escola->endereco_numero}, {$cobranca->escola->endereco_complemento}, {$cobranca->escola->cep->bairro}, {$cobranca->escola->cep->cidade}-{$cobranca->escola->cep->uf}</span>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered header-info">
                    <span class="label">E-mail:</span> <span class="value">{$cobranca->escola->email_financeiro}</span>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered header-info">
                    <span class="label">Telefone:</span> <span class="value">{$cobranca->escola->telefone_financeiro}</span>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="2" style="padding: 8px 5px;">
                    <p><span class="value">Curso: {$cobranca->matricula->curso->curso}</span></p>
                    <p><span class="value">Data de Início: {$cobranca->matricula->turma->data_inicio}</span></p>
                    <p><span class="value">Aluno: {$cobranca->matricula->cliente->nome}</span></p>
                    <p><span class="value">Matrícula: {$cobranca->matricula_id}</span></p>
                </td>
            </tr>
        </table>

        <br />

        <!-- Recibo do Pagador - Dados do Boleto -->
        <table class="bordered">
            <tr>
                <td class="cell-bordered" colspan="5">
                    <table style="border: none; margin: -3px;">
                        <tr>
                            <td style="width: 20%; border-right: 1px solid #000; text-align: center; padding: 5px;">
                                <img src="{$cobranca->tipo_pagamento->gateway_pagamento->logo_banco}" alt="Banco" style="max-height: 40px;" />
                            </td>
                            <td style="width: 12%; border-right: 1px solid #000; text-align: center; vertical-align: middle; padding: 5px;">
                                <span style="font-size: 14pt; font-weight: bold;">{$cobranca->tipo_pagamento->gateway_pagamento->cod_banco}</span>
                            </td>
                            <td style="width: 68%; text-align: right; vertical-align: middle; padding: 5px;">
                                <span class="value">{$linha_digitavel}</span>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5">
                    <p><span class="label">Beneficiário:</span></p>
                    <p><span class="value">{$cobranca->escola->razao_social} - CNPJ {$cobranca->escola->cnpj}</span></p>
                    <p><span class="value">{$cobranca->escola->cep->logradouro}, {$cobranca->escola->endereco_complemento}, {$cobranca->escola->cep->bairro}, {$cobranca->escola->cep->cidade}-{$cobranca->escola->cep->uf}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 19%;">
                    <p><span class="label">Nº Documento:</span></p>
                    <p><span class="value">{$numero_documento_remessa}</span></p>
                </td>
                <td class="cell-bordered" style="width: 23%;">
                    <p><span class="label">Nosso Número:</span></p>
                    <p><span class="value">{$nosso_numero}</span></p>
                </td>
                <td class="cell-bordered" style="width: 20%;">
                    <p><span class="label">Data de Processamento:</span></p>
                    <p><span class="value">{$data_geracao}</span></p>
                </td>
                <td class="cell-bordered" style="width: 18%;">
                    <p><span class="label">Vencimento:</span></p>
                    <p><span class="value-bold">{$cobranca->data_vencimento}</span></p>
                </td>
                <td class="cell-bordered" style="width: 20%;">
                    <p><span class="label">Valor:</span></p>
                    <p><span class="value-bold">{$valor_cobranca}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5" style="height: 60px;">
                    <p><span class="label">Instruções:</span></p>
                    <p><span class="value">Não aceitar após vencimento</span></p>
                    <p><span class="value">Não aceitar pagamento com Cheque</span></p>
                    <p><span class="value">Multa de {$cobranca->tipo_pagamento->juros_multa}% por atraso e juros de {$cobranca->tipo_pagamento->juros_atraso}% ao dia.</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5">
                    <p><span class="label">Pagador:</span></p>
                    <p><span class="value">{$cobranca->cliente->nome} - {$cobranca->cliente->cpf_cnpj}</span></p>
                    <p><span class="label">Endereço:</span></p>
                    <p><span class="value">{$cobranca->cliente->cep->logradouro}, {$cobranca->cliente->endereco_numero} {$cobranca->cliente->endereco_complemento}, {$cobranca->cliente->cep->bairro}, {$cobranca->cliente->cep->cidade}-{$cobranca->cliente->cep->uf} CEP {$cobranca->cliente->cep->cep}</span></p>
                </td>
            </tr>
        </table>

        <p class="text-right text-small">Autenticação mecânica - Recibo do pagador</p>

        <!-- Linha de Corte -->
        <div class="linha-corte">
            Corte aqui
        </div>

        <!-- Aviso de Segurança -->
        <div class="aviso-seguranca">
            <strong>⚠ IMPORTANTE:</strong> Não dobre, não amasse e não rasure este boleto. Isso pode dificultar a leitura do código de barras.
        </div>

        <!-- FICHA DE COMPENSAÇÃO (Parte Inferior) -->
        <table class="bordered">
            <tr>
                <td class="cell-bordered" colspan="6">
                    <table style="border: none; margin: -3px;">
                        <tr>
                            <td style="width: 20%; border-right: 1px solid #000; text-align: center; padding: 5px;">
                                <img src="{$cobranca->tipo_pagamento->gateway_pagamento->logo_banco}" alt="Banco" style="max-height: 40px;" />
                            </td>
                            <td style="width: 12%; border-right: 1px solid #000; text-align: center; vertical-align: middle; padding: 5px;">
                                <span style="font-size: 18pt; font-weight: bold;">{$cobranca->tipo_pagamento->gateway_pagamento->cod_banco}</span>
                            </td>
                            <td style="width: 68%; text-align: right; vertical-align: middle; padding: 5px;">
                                <span class="value">{$linha_digitavel}</span>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5" style="width: 77%;">
                    <p><span class="label">Local de Pagamento:</span></p>
                    <p><span class="value">Pagável em qualquer banco até o vencimento</span></p>
                </td>
                <td class="cell-bordered" style="width: 23%;">
                    <p><span class="label">Vencimento:</span></p>
                    <p><span class="value-bold">{$cobranca->data_vencimento}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5" style="width: 77%;">
                    <p><span class="label">Beneficiário:</span></p>
                    <p><span class="value">{$cobranca->escola->razao_social} - CNPJ {$cobranca->escola->cnpj}</span></p>
                </td>
                <td class="cell-bordered" style="width: 23%;">
                    <p><span class="label">Agência/Código Beneficiário:</span></p>
                    <p><span class="value">{$cobranca->tipo_pagamento->gateway_pagamento->agencia_banco} / {$cobranca->tipo_pagamento->gateway_pagamento->conta_banco}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 17.5%;">
                    <p><span class="label">Data do Documento:</span></p>
                    <p><span class="value">{$data_documento}</span></p>
                </td>
                <td class="cell-bordered" style="width: 14.8%;">
                    <p><span class="label">Nº Documento:</span></p>
                    <p><span class="value">{$numero_documento_remessa}</span></p>
                </td>
                <td class="cell-bordered" style="width: 11.8%;">
                    <p><span class="label">Espécie Doc.:</span></p>
                    <p><span class="value">DM</span></p>
                </td>
                <td class="cell-bordered" style="width: 11.7%;">
                    <p><span class="label">Aceite:</span></p>
                    <p><span class="value">N</span></p>
                </td>
                <td class="cell-bordered" style="width: 20.7%;">
                    <p><span class="label">Data de Processamento:</span></p>
                    <p><span class="value">{$data_geracao}</span></p>
                </td>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">Nosso Número:</span></p>
                    <p><span class="value">{$nosso_numero}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 17.5%;">
                    <p><span class="label">Uso do Banco:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
                <td class="cell-bordered" style="width: 14.8%;">
                    <p><span class="label">Carteira:</span></p>
                    <p><span class="value">{$cobranca->tipo_pagamento->gateway_pagamento->carteira_banco}</span></p>
                </td>
                <td class="cell-bordered" style="width: 11.8%;">
                    <p><span class="label">Espécie:</span></p>
                    <p><span class="value">R$</span></p>
                </td>
                <td class="cell-bordered" style="width: 11.7%;">
                    <p><span class="label">Quantidade:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
                <td class="cell-bordered" style="width: 20.7%;">
                    <p><span class="label">Valor Unitário:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">Valor do Documento:</span></p>
                    <p><span class="value-bold">{$valor_cobranca}</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="5" rowspan="6" style="width: 77%; vertical-align: top;">
                    <p><span class="label">Instruções de Responsabilidade do Beneficiário:</span></p>
                    <p><span class="value">Não aceitar após vencimento</span></p>
                    <p><span class="value">Não aceitar pagamento com Cheque</span></p>
                    <p><span class="value">Multa de {$cobranca->tipo_pagamento->juros_multa}% por atraso e juros de {$cobranca->tipo_pagamento->juros_atraso}% ao dia.</span></p>
                    <p><span class="value">Após o vencimento cobrar multa de R$ {$valor_multa}</span></p>
                    <p><span class="value">Protesto automático após {$dias_protesto} dias do vencimento</span></p>
                </td>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">(-) Desconto/Abatimento:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">(-) Outras Deduções:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">(+) Mora/Multa:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">(+) Outros Acréscimos:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 23.5%;">
                    <p><span class="label">(=) Valor Cobrado:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" style="width: 23.5%; background-color: #f0f0f0;">
                    <p><span class="label" style="font-size: 9pt;">Código de Baixa:</span></p>
                    <p><span class="value">&nbsp;</span></p>
                </td>
            </tr>
            <tr>
                <td class="cell-bordered" colspan="6" style="height: 80px; vertical-align: top;">
                    <p><span class="label">Pagador:</span></p>
                    <p><span class="value">{$cobranca->cliente->nome} - CPF/CNPJ: {$cobranca->cliente->cpf_cnpj}</span></p>
                    <p><span class="label">Endereço:</span></p>
                    <p><span class="value">{$cobranca->cliente->cep->logradouro}, {$cobranca->cliente->endereco_numero} {$cobranca->cliente->endereco_complemento}</span></p>
                    <p><span class="value">{$cobranca->cliente->cep->bairro}, {$cobranca->cliente->cep->cidade}-{$cobranca->cliente->cep->uf} CEP {$cobranca->cliente->cep->cep}</span></p>
                    <p><span class="label">Sacador/Avalista:</span></p>
                </td>
            </tr>
        </table>

        <!-- Código de Barras -->
        <table style="margin-top: 5px;">
            <tr>
                <td style="width: 70%; text-align: center; vertical-align: middle; padding: 10px; background-color: #fff;">
                    <div class="codigo-barras-container">
                        {$codigoBarrasBoletoPNG}
                    </div>
                </td>
                <td style="width: 30%; text-align: right; vertical-align: bottom; padding: 5px;">
                    <p class="text-small">Autenticação mecânica</p>
                    <p class="text-small"><strong>Ficha de Compensação</strong></p>
                </td>
            </tr>
        </table>

        <div style="margin-top: 10px; padding: 5px; background-color: #f9f9f9; border: 1px solid #ddd; font-size: 8pt;">
            <p><strong>Atenção:</strong> Este documento possui código de barras para facilitar o pagamento. Mantenha-o em bom estado.</p>
            <p>SAC: {$cobranca->escola->telefone_financeiro} | Ouvidoria: (se aplicável)</p>
        </div>

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

