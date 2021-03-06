---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitir que somente determinados caracteres em uma caixa de texto (VB) | Microsoft Docs
author: wenz
description: "Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impede que os usuários digitem inválidos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Permitir que somente determinados caracteres em uma caixa de texto (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impedir que os usuários digitem caracteres inválidos e tentar enviar o formulário.


## <a name="overview"></a>Visão Geral

Controles de validação ASP.NET podem garantir que somente determinados caracteres são permitidos em entrada do usuário. No entanto isso ainda não impedir que os usuários digitem caracteres inválidos e tentar enviar o formulário.

## <a name="steps"></a>Etapas

O Kit de ferramentas de controle do ASP.NET AJAX contém o `FilteredTextBox` controle que se estende de uma caixa de texto. Quando ativado, apenas um determinado conjunto de caracteres pode ser inserido no campo.

Para que isso funcione, precisamos primeiro como de costume ASP.NET AJAX `ScriptManager` que carrega as bibliotecas JavaScript que também são usadas pelo Kit de ferramentas de controle AJAX ASP.NET:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Em seguida, precisamos de uma caixa de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Por fim, o `FilteredTextBoxExtender` controle cuida de restringir os caracteres que o usuário tem permissão para o tipo. Primeiro, defina o `TargetControlID` de atributo para o `ID` do `TextBox` controle. Em seguida, escolha uma das disponíveis `FilterType` valores:

- `Custom`padrão. Você deve fornecer uma lista de caracteres válidos
- `LowercaseLetters`apenas letras minúsculas
- `Numbers`apenas dígitos
- `UppercaseLetters`somente as letras maiusculas

Se o `Custom FilterType` for usado, o `ValidChars` propriedade deve ser definido e fornecer uma lista de caracteres que podem ser digitados. Aliás: se você tentar colar o texto na caixa de texto, todos os caracteres inválidos são removidos.

Aqui está a marcação para o `FilteredTextBoxExtender` controle que permite apenas dígitos (algo que também seria possível com `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Execute a página e tente inserir uma letra se JavaScript estiver habilitado, ele não funcionará; No entanto, dígitos exibida na página. No entanto, observe que a proteção `FilteredTextBox` fornece não é prova: JavaScript se estiver habilitada, todos os dados podem ser inseridos na caixa de texto, você precisará usar meios de validação adicionais, ou seja, o ASP. Controles de validação do NET.


[![Apenas dígitos podem ser inseridos.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Podem ser inseridos apenas dígitos ([clique para exibir a imagem em tamanho normal](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](allowing-only-certain-characters-in-a-text-box-cs.md)
