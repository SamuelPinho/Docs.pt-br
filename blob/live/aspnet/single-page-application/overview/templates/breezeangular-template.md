---
uid: single-page-application/overview/templates/breezeangular-template
title: "Modelo muito fácil/Angular | Microsoft Docs"
author: madskristensen
description: "Modelo de aplicativo de página única muito fácil/Angular"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a><span data-ttu-id="39216-103">Modelo muito fácil/Angular</span><span class="sxs-lookup"><span data-stu-id="39216-103">Breeze/Angular template</span></span>
====================
<span data-ttu-id="39216-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="39216-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="39216-105">O modelo MVC muito fácil/Angular foi gravado pelo Ward Bell</span><span class="sxs-lookup"><span data-stu-id="39216-105">The Breeze/Angular MVC Template was written by Ward Bell</span></span>
> 
> [<span data-ttu-id="39216-106">Baixe o modelo MVC muito fácil/Angular</span><span class="sxs-lookup"><span data-stu-id="39216-106">Download the Breeze/Angular MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=286437)


<span data-ttu-id="39216-107">[AngularJS](http://angularjs.org) é uma biblioteca de código aberto do Google para a criação de aplicativos de página única (SPAs).</span><span class="sxs-lookup"><span data-stu-id="39216-107">[AngularJS](http://angularjs.org) is an open source library from Google for building Single Page Applications (SPAs).</span></span> <span data-ttu-id="39216-108">Ele oferece gerenciamento de tela, injeção de dependência e associação de dados.</span><span class="sxs-lookup"><span data-stu-id="39216-108">It offers data binding, dependency injection, and screen management.</span></span> <span data-ttu-id="39216-109">Combinar com [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), outra biblioteca de software livre para modelagem de dados e gerenciamento de dados e você tiver os componentes essenciais de um grande aplicativo cliente HTML/JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39216-109">Combine it with [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), another open source library for data modeling and data management, and you have the essential ingredients for a great HTML/JavaScript client app.</span></span>

<span data-ttu-id="39216-110">O modelo muito fácil/Angular SPA é uma variação no [modelo SPA KnockoutJS](../introduction/knockoutjs-template.md) incluído no ASP.NET e Web Tools 2012.2 atualização.</span><span class="sxs-lookup"><span data-stu-id="39216-110">The Breeze/Angular SPA template is a variation on the [KnockoutJS SPA template](../introduction/knockoutjs-template.md) included in the ASP.NET and Web Tools 2012.2 Update.</span></span> <span data-ttu-id="39216-111">Se você tiver o Visual Studio, você terá um exemplo SPA em execução em menos de 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="39216-111">If you've got Visual Studio, you'll have an example SPA up and running in less than 60 seconds.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

<span data-ttu-id="39216-112">Uso externo, o aplicativo parece muito semelhante ao modelo KnockoutJS SPA.</span><span class="sxs-lookup"><span data-stu-id="39216-112">Outwardly, the application looks the very similar to the KnockoutJS SPA template.</span></span> <span data-ttu-id="39216-113">Mas é bastante diferente nos bastidores.</span><span class="sxs-lookup"><span data-stu-id="39216-113">But it's quite different under the hood.</span></span> <span data-ttu-id="39216-114">O modelo de KnockoutJS usa Knockout para associação de dados e AJAX bruto para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="39216-114">The KnockoutJS template uses Knockout for data binding and raw AJAX for data access.</span></span> <span data-ttu-id="39216-115">O modelo muito fácil/Angular usa Angular para associação de dados e muito fácil para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="39216-115">The Breeze/Angular template uses Angular for data binding and Breeze for data access.</span></span> <span data-ttu-id="39216-116">Essas bibliotecas de habilitar recursos adicionais, incluindo o histórico e navegação de página.</span><span class="sxs-lookup"><span data-stu-id="39216-116">These libaries enable additional capabilities, including page navigation and history.</span></span>

<span data-ttu-id="39216-117">Aqui está a página do aplicativo sobre:</span><span class="sxs-lookup"><span data-stu-id="39216-117">Here is the application's About page:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

<span data-ttu-id="39216-118">Esta página exibe um log de eventos durante a sessão atual do usuário, incluindo:</span><span class="sxs-lookup"><span data-stu-id="39216-118">This page displays a running log of events during the current user session, including:</span></span>

- <span data-ttu-id="39216-119">Paginação.</span><span class="sxs-lookup"><span data-stu-id="39216-119">Paging.</span></span> <span data-ttu-id="39216-120">Observe a criação do controlador de tarefas em 2 de # e #7.</span><span class="sxs-lookup"><span data-stu-id="39216-120">Note the Todo controller creation at #2 and #7.</span></span>
- <span data-ttu-id="39216-121">Consultas remotas (3) e consultas de cache local (7).</span><span class="sxs-lookup"><span data-stu-id="39216-121">Remote queries (#3) and local cache queries (#7).</span></span>
- <span data-ttu-id="39216-122">Salvar novas (n º 5, &#6;) e modificação de entidades (4).</span><span class="sxs-lookup"><span data-stu-id="39216-122">Saving new (#5, #6) and modified (#4) entities.</span></span>
- <span data-ttu-id="39216-123">Alterações validadas no cliente (n º 9), para que o usuário pode corrigir os erros antes de confirmar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="39216-123">Changes validated on the client (#9), so the user can correct mistakes before committing changes to the database.</span></span>

<span data-ttu-id="39216-124">Há mais para explorar neste modelo, incluindo:</span><span class="sxs-lookup"><span data-stu-id="39216-124">There's more to explore in this template, including:</span></span>

- <span data-ttu-id="39216-125">Carregamento dinâmico de modelos de exibição HTML.</span><span class="sxs-lookup"><span data-stu-id="39216-125">Dynamic loading of HTML view templates.</span></span>
- <span data-ttu-id="39216-126">Associação de dados personalizados por meio de Angular "diretivas".</span><span class="sxs-lookup"><span data-stu-id="39216-126">Custom data binding through Angular "directives."</span></span>
- <span data-ttu-id="39216-127">Injeção de dependência e modularidade.</span><span class="sxs-lookup"><span data-stu-id="39216-127">Modularity and dependency injection.</span></span>
- <span data-ttu-id="39216-128">Filtros de consulta, classificações, paginação, projeções e inclusão de entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="39216-128">Query filters, sorts, paging, projections, and inclusion of related entities.</span></span>
- <span data-ttu-id="39216-129">Compartilhamento de dados em várias telas.</span><span class="sxs-lookup"><span data-stu-id="39216-129">Sharing data across multiple screens.</span></span>
- <span data-ttu-id="39216-130">Salvar várias alterações como uma única transação.</span><span class="sxs-lookup"><span data-stu-id="39216-130">Saving multiple changes as a single transaction.</span></span>
- <span data-ttu-id="39216-131">Regras de validação propagadas automaticamente do servidor ao cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="39216-131">Validation rules propagated automatically from the server to the JavaScript client.</span></span>

<span data-ttu-id="39216-132">Vamos começar.</span><span class="sxs-lookup"><span data-stu-id="39216-132">Let's get started.</span></span>

## <a name="create-a-breezeangular-template-project"></a><span data-ttu-id="39216-133">Criar um projeto de modelo muito fácil/Angular</span><span class="sxs-lookup"><span data-stu-id="39216-133">Create a Breeze/Angular Template Project</span></span>

<span data-ttu-id="39216-134">Baixe e instale o modelo, clique no botão de Download acima.</span><span class="sxs-lookup"><span data-stu-id="39216-134">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="39216-135">O modelo é empacotado como um arquivo de extensão de Visual Studio (VSIX).</span><span class="sxs-lookup"><span data-stu-id="39216-135">The template is packaged as a Visual Studio Extension (VSIX) file.</span></span> <span data-ttu-id="39216-136">Talvez seja necessário reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39216-136">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="39216-137">No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó.</span><span class="sxs-lookup"><span data-stu-id="39216-137">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="39216-138">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="39216-138">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="39216-139">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="39216-139">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="39216-140">Nomeie o projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="39216-140">Name the project and click **OK**.</span></span>

<span data-ttu-id="39216-141">No **novo projeto** assistente, selecione **SPA Angular muito fácil**.</span><span class="sxs-lookup"><span data-stu-id="39216-141">In the **New Project** wizard, select **Breeze Angular SPA**.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

<span data-ttu-id="39216-142">Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração, ou pressione F5 para executar com depuração.</span><span class="sxs-lookup"><span data-stu-id="39216-142">Press Ctrl-F5 to build and run the application without debugging, or press F5 to run with debugging.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

<span data-ttu-id="39216-143">Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon.</span><span class="sxs-lookup"><span data-stu-id="39216-143">When the application first runs, it displays a login screen.</span></span> <span data-ttu-id="39216-144">Clique no link "Inscrição" e uma nova página glides no modo de exibição, onde você pode inserir um nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="39216-144">Click the "Sign up" link and a new page glides into view, where you can enter a username and password.</span></span> <span data-ttu-id="39216-145">(As páginas de logon e registro são criadas usando o ASP.NET MVC). Quando você envia o formulário de registro, o servidor gera uma lista de tarefas com dois itens para sua conta.</span><span class="sxs-lookup"><span data-stu-id="39216-145">(The login and registration pages are built using ASP.NET MVC.) When you submit the registration form, the server generates a TodoList with two items for your account.</span></span> <span data-ttu-id="39216-146">Em seguida, apresenta-los para você em uma nota amarela.</span><span class="sxs-lookup"><span data-stu-id="39216-146">Then it presents them to you on a yellow note.</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

<span data-ttu-id="39216-147">Agora você está na Terra da controladora.</span><span class="sxs-lookup"><span data-stu-id="39216-147">Now you are in the land of SPA.</span></span> <span data-ttu-id="39216-148">Tudo o que você vê e experiência enquanto manipular Todos é renderizado e gerenciado no cliente com a Ajuda do Knockout e muito fácil.</span><span class="sxs-lookup"><span data-stu-id="39216-148">Everything you see and experience while manipulating Todos is rendered and managed on the client with the help of Knockout and Breeze.</span></span> <span data-ttu-id="39216-149">Explore o aplicativo como um usuário...</span><span class="sxs-lookup"><span data-stu-id="39216-149">Explore the app as a user …</span></span> <span data-ttu-id="39216-150">mas com olho do desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="39216-150">but with a developer's eye.</span></span> <span data-ttu-id="39216-151">Use as ferramentas de desenvolvedor em seu navegador para capturar o tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="39216-151">Use the developer tools in your browser to capture the network traffic.</span></span> <span data-ttu-id="39216-152">(No Internet Explorer: pressione F12, selecione o **rede** guia e, em seguida, clique em **iniciar a captura**.) Agora, tente o seguinte:</span><span class="sxs-lookup"><span data-stu-id="39216-152">(In Internet Explorer: Press F12, select the **Network** tab, and click **Start capturing**.) Now try the following:</span></span>

- <span data-ttu-id="39216-153">Adicione um novo item de tarefas.</span><span class="sxs-lookup"><span data-stu-id="39216-153">Add a new Todo item.</span></span>
- <span data-ttu-id="39216-154">Clique no rótulo e editar o título do item de tarefas</span><span class="sxs-lookup"><span data-stu-id="39216-154">Click the label and edit the Todo item title</span></span>
- <span data-ttu-id="39216-155">Marque a caixa de seleção para marcar o item concluído.</span><span class="sxs-lookup"><span data-stu-id="39216-155">Check a checkbox to mark the item done.</span></span> <span data-ttu-id="39216-156">Observe que a caixa de texto está desabilitada, portanto o título não é mais editável.</span><span class="sxs-lookup"><span data-stu-id="39216-156">Notice that the textbox is disabled, so the title is no longer editable.</span></span>
- <span data-ttu-id="39216-157">Clique em 'x' à direita do rótulo.</span><span class="sxs-lookup"><span data-stu-id="39216-157">Click the ‘x' to the right of the label.</span></span> <span data-ttu-id="39216-158">O item desaparece e é excluído do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="39216-158">The item disappears and is deleted from the database.</span></span>
- <span data-ttu-id="39216-159">Escolha outro item e limpar seu título.</span><span class="sxs-lookup"><span data-stu-id="39216-159">Pick another item and clear its title.</span></span> <span data-ttu-id="39216-160">Você obterá um erro de validação que o título é necessário.</span><span class="sxs-lookup"><span data-stu-id="39216-160">You'll get a validation error that the title is required.</span></span> <span data-ttu-id="39216-161">Após uma pequena pausa, o título do anterior é restaurado.</span><span class="sxs-lookup"><span data-stu-id="39216-161">After a brief pause, the previous title is restored.</span></span>
- <span data-ttu-id="39216-162">Digite um título ridiculamente longo.</span><span class="sxs-lookup"><span data-stu-id="39216-162">Type in a ridiculously long title.</span></span> <span data-ttu-id="39216-163">Você obterá um erro de validação diferente que o título é muito longo.</span><span class="sxs-lookup"><span data-stu-id="39216-163">You'll get a different validation error that the title is too long.</span></span>
- <span data-ttu-id="39216-164">Clique no botão "Adicionar a lista de tarefas".</span><span class="sxs-lookup"><span data-stu-id="39216-164">Click the "Add Todo List" button.</span></span> <span data-ttu-id="39216-165">Uma nova lista aparece à esquerda da lista anterior.</span><span class="sxs-lookup"><span data-stu-id="39216-165">A new list appears to the left of the previous list.</span></span>
- <span data-ttu-id="39216-166">Executar com o título da lista de tarefas, acionando necessário e validações de comprimento.</span><span class="sxs-lookup"><span data-stu-id="39216-166">Play with the TodoList title, triggering its required and length validations.</span></span>
- <span data-ttu-id="39216-167">Clique na caixa de texto título para limpar a mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="39216-167">Click in the title textbox to clear the error message.</span></span>
- <span data-ttu-id="39216-168">Clique no círculo no canto superior direito para excluir a lista de tarefas e seu todos "x".</span><span class="sxs-lookup"><span data-stu-id="39216-168">Click the "x" in the circle in the upper right corner to delete the TodoList and its todos.</span></span>
- <span data-ttu-id="39216-169">Clique no link "Sobre" no canto superior direito para ver um log dessas atividades.</span><span class="sxs-lookup"><span data-stu-id="39216-169">Click the "About" link in the upper right to see a log of these activities.</span></span>

<span data-ttu-id="39216-170">A lógica de validação é executada no lado do cliente por muito fácil.</span><span class="sxs-lookup"><span data-stu-id="39216-170">The validation logic is performed client-side by Breeze.</span></span> <span data-ttu-id="39216-171">Atributos de validação em classes de modelo do servidor são propagados para o cliente e executados automaticamente antes do cliente contata o servidor.</span><span class="sxs-lookup"><span data-stu-id="39216-171">Validation attributes on the server model classes are propagated to the client and executed automatically before the client contacts the server.</span></span>

<span data-ttu-id="39216-172">Analise o tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="39216-172">Review the network traffic.</span></span> <span data-ttu-id="39216-173">Observe que não houve nenhuma chamada para o servidor quando muito fácil detectou um erro.</span><span class="sxs-lookup"><span data-stu-id="39216-173">Notice that there were no calls to the server when Breeze detected an error.</span></span> <span data-ttu-id="39216-174">Cada alteração válida resultou em uma solicitação POST para "/ tarefas/api/SaveChanges".</span><span class="sxs-lookup"><span data-stu-id="39216-174">Each valid change resulted in a POST request to "/api/Todo/SaveChanges".</span></span> <span data-ttu-id="39216-175">Muito fácil agrupa as alterações e as envia juntos como uma única solicitação para o controlador de API da Web `SaveChanges` método.</span><span class="sxs-lookup"><span data-stu-id="39216-175">Breeze bundles the changes and sends them together as a single request to the Web API controller's `SaveChanges` method.</span></span> <span data-ttu-id="39216-176">Isso é diferente do modelo do SPA KockoutJS, o que torna PUT, POST e excluir solicitações de cada item individualmente.</span><span class="sxs-lookup"><span data-stu-id="39216-176">That's different from KockoutJS SPA template, which makes PUT, POST, and DELETE requests for each item individually.</span></span>

<span data-ttu-id="39216-177">Além disso, observe que não há nenhum tráfego de rede quando você alternar entre a lista de tarefas e sobre as páginas.</span><span class="sxs-lookup"><span data-stu-id="39216-177">Also, notice there is no network traffic when you switch between the TodoList and About pages.</span></span> <span data-ttu-id="39216-178">Isso ocorre porque a consulta tiver sido restrito ao cache local muito fácil.</span><span class="sxs-lookup"><span data-stu-id="39216-178">That's because the query has been constrained to the local Breeze cache.</span></span>

## <a name="peek-inside"></a><span data-ttu-id="39216-179">Inspecionar dentro</span><span class="sxs-lookup"><span data-stu-id="39216-179">Peek inside</span></span>

<span data-ttu-id="39216-180">Este aplicativo tem um cliente e no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="39216-180">This application has a client side and a server side.</span></span> <span data-ttu-id="39216-181">A pilha do lado do cliente consiste em HTML um pouco e uma combinação de módulos de JavaScript do aplicativo (na pasta "aplicativo") e bibliotecas JavaScript de terceiros (na pasta "Scripts").</span><span class="sxs-lookup"><span data-stu-id="39216-181">The client-side stack consists of a little HTML and a combination of application JavaScript modules (in the "app" folder) plus third-party JavaScript libraries (in the "Scripts" folder).</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

<span data-ttu-id="39216-182">A arquitetura de interface de usuário separa os widgets HTML dos modos de exibição do código de apresentação suporte nos controladores.</span><span class="sxs-lookup"><span data-stu-id="39216-182">The UI architecture separates the HTML widgets of the views from the supporting presentation code in the controllers.</span></span> <span data-ttu-id="39216-183">O sistema de associação de dados Angular coordena controladores e exibições para que cada uma pode fazer seu trabalho sem conhecimento profundo do outro.</span><span class="sxs-lookup"><span data-stu-id="39216-183">The Angular data-binding system coordinates views and controllers so that each can do its job without intimate knowledge of the other.</span></span>

<span data-ttu-id="39216-184">O controlador solicita o contexto de dados para adquirir e salvar as entidades de modelo.</span><span class="sxs-lookup"><span data-stu-id="39216-184">The controller asks the data context to acquire and save the model entities.</span></span> <span data-ttu-id="39216-185">O contexto de dados delega a maior parte do trabalho para muito fácil, que cria objetos de modelo de rastreamento automático JSON de resultados da consulta.</span><span class="sxs-lookup"><span data-stu-id="39216-185">The data context delegates most of the work to Breeze, which constructs self-tracking model objects from JSON query results.</span></span>

<span data-ttu-id="39216-186">A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas de entidade de segurança do .NET: API da Web, Entity Framework e Breeze.NET:</span><span class="sxs-lookup"><span data-stu-id="39216-186">The server-side stack consists of some developer code and three principle .NET libraries: Web API, Entity Framework, and Breeze.NET:</span></span>

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

<span data-ttu-id="39216-187">A arquitetura básica é o mesmo que o modelo KockoutJS SPA.</span><span class="sxs-lookup"><span data-stu-id="39216-187">The basic architecture is the same as the KockoutJS SPA template.</span></span> <span data-ttu-id="39216-188">No entanto, a implementação é muito mais simples: O DTOs foram excluídos e a maioria dos detalhes do Entity Framework foram delegada a Breeze.NET.</span><span class="sxs-lookup"><span data-stu-id="39216-188">However, the implementation is much simpler: The DTOs were deleted, and most of the Entity Framework details have been delegated to Breeze.NET.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39216-189">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="39216-189">Next Steps</span></span>

<span data-ttu-id="39216-190">Sugerimos que você explore o código, seguindo o [discussão abrangente](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) do cliente e as pilhas de servidor no site muito fácil.</span><span class="sxs-lookup"><span data-stu-id="39216-190">We suggest that you explore the code, guided by the [extensive discussion](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) of both the client and the server stacks on the Breeze website.</span></span>

<span data-ttu-id="39216-191">Você pode tentar brincando com consultas de cliente muito fácil; Adicione filtros e classificações.</span><span class="sxs-lookup"><span data-stu-id="39216-191">You might try playing with Breeze client-side query; add some filters and sorts.</span></span> <span data-ttu-id="39216-192">Você pode adicionar mais propriedades do modelo e mais entidades para entender melhor para o desenvolvimento de SPA de ponta a ponta.</span><span class="sxs-lookup"><span data-stu-id="39216-192">You might add more model properties and more entities to get a better feel for end-to-end SPA development.</span></span> <span data-ttu-id="39216-193">Quando você tiver certeza do design, você pode desfazer os recursos de Todo e substituí-los com seus próprios.</span><span class="sxs-lookup"><span data-stu-id="39216-193">When you are confident of the design, you can tear out the Todo features and replace them with your own.</span></span>

<span data-ttu-id="39216-194">Boa codificação!</span><span class="sxs-lookup"><span data-stu-id="39216-194">Happy coding!</span></span>