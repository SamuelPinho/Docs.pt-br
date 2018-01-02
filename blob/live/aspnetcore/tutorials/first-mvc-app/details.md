---
title: "Examinando os métodos Details e Delete"
author: rick-anderson
description: "A exibição e o método do controlador Details em um aplicativo ASP.NET Core MVC básico."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 43394106c9074f9487e1065a37a88eb017833bae
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="d702f-104">Examinando os métodos Details e Delete</span><span class="sxs-lookup"><span data-stu-id="d702f-104">Examining the Details and Delete methods</span></span>

<span data-ttu-id="d702f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d702f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d702f-106">Abra o controlador Movie e examine o método `Details`:</span><span class="sxs-lookup"><span data-stu-id="d702f-106">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="d702f-107">O mecanismo de scaffolding MVC que criou este método de ação adiciona um comentário mostrando uma solicitação HTTP que invoca o método.</span><span class="sxs-lookup"><span data-stu-id="d702f-107">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="d702f-108">Nesse caso, é uma solicitação GET com três segmentos de URL, o controlador `Movies`, o método `Details` e um valor `id`.</span><span class="sxs-lookup"><span data-stu-id="d702f-108">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="d702f-109">Lembre-se que esses segmentos são definidos em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d702f-109">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="d702f-110">O EF facilita a pesquisa de dados usando o método `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="d702f-110">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="d702f-111">Um recurso de segurança importante interno do método é que o código verifica se o método de pesquisa encontrou um filme antes de tentar fazer algo com ele.</span><span class="sxs-lookup"><span data-stu-id="d702f-111">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="d702f-112">Por exemplo, um hacker pode introduzir erros no site alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real).</span><span class="sxs-lookup"><span data-stu-id="d702f-112">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="d702f-113">Se você não marcou um filme nulo, o aplicativo gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="d702f-113">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="d702f-114">Examine os métodos `Delete` e `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="d702f-114">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="d702f-115">Observe que o método `HTTP GET Delete` não exclui o filme especificado, mas retorna uma exibição do filme em que você pode enviar (HttpPost) a exclusão.</span><span class="sxs-lookup"><span data-stu-id="d702f-115">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="d702f-116">A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança.</span><span class="sxs-lookup"><span data-stu-id="d702f-116">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="d702f-117">O método `[HttpPost]` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva.</span><span class="sxs-lookup"><span data-stu-id="d702f-117">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="d702f-118">As duas assinaturas de método são mostradas abaixo:</span><span class="sxs-lookup"><span data-stu-id="d702f-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="d702f-119">O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros).</span><span class="sxs-lookup"><span data-stu-id="d702f-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="d702f-120">No entanto, aqui você precisa de dois métodos `Delete` – um para GET e outro para POST – que têm a mesma assinatura de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="d702f-120">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="d702f-121">(Ambos precisam aceitar um único inteiro como parâmetro.)</span><span class="sxs-lookup"><span data-stu-id="d702f-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="d702f-122">Há duas abordagens para esse problema e uma delas é fornecer aos métodos nomes diferentes.</span><span class="sxs-lookup"><span data-stu-id="d702f-122">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="d702f-123">Foi isso o que o mecanismo de scaffolding fez no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="d702f-123">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="d702f-124">No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método.</span><span class="sxs-lookup"><span data-stu-id="d702f-124">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="d702f-125">A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="d702f-125">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="d702f-126">Esse atributo executa o mapeamento para o sistema de roteamento, de modo que uma URL que inclui /Delete/ para uma solicitação POST encontre o método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="d702f-126">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="d702f-127">Outra solução alternativa comum para métodos que têm nomes e assinaturas idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro extra (não utilizado).</span><span class="sxs-lookup"><span data-stu-id="d702f-127">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="d702f-128">Foi isso o que fizemos em uma postagem anterior quando adicionamos o parâmetro `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="d702f-128">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="d702f-129">Você pode fazer o mesmo aqui para o método `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="d702f-129">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="d702f-130">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="d702f-130">Publish to Azure</span></span>

<span data-ttu-id="d702f-131">Confira as instruções sobre como publicar este aplicativo no Azure usando o Visual Studio em [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="d702f-131">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="d702f-132">O aplicativo também pode ser publicado a partir da [linha de comando](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="d702f-132">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="d702f-133">Obrigado por concluir esta introdução ao ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d702f-133">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="d702f-134">Agradecemos todos os comentários deixados.</span><span class="sxs-lookup"><span data-stu-id="d702f-134">We appreciate any comments you leave.</span></span> <span data-ttu-id="d702f-135">[Introdução ao MVC e ao EF Core](xref:data/ef-mvc/intro) é um excelente acompanhamento para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d702f-135">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d702f-136">Anterior</span><span class="sxs-lookup"><span data-stu-id="d702f-136">Previous</span></span>](validation.md)