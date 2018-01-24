---
title: "Middleware no núcleo do ASP.NET de regravação de URL"
author: guardrex
description: "Saiba mais sobre a URL de regravação e redirecionar com Middleware de regravação de URL em aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 99f8d1cc73fdcbd99cffe595ae89f3c61a6f9a53
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="a11ea-103">Middleware no núcleo do ASP.NET de regravação de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="a11ea-104">Por [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="a11ea-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="a11ea-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a11ea-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a11ea-106">Regravação de URL é o ato de modificar as URLs com base em uma ou mais regras predefinidas de solicitação.</span><span class="sxs-lookup"><span data-stu-id="a11ea-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="a11ea-107">Regravação de URL criará uma abstração entre locais de recursos e seus endereços de forma que os locais e os endereços não estão vinculados firmemente.</span><span class="sxs-lookup"><span data-stu-id="a11ea-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="a11ea-108">Há várias situações em que a regravação de URL é útil:</span><span class="sxs-lookup"><span data-stu-id="a11ea-108">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="a11ea-109">Movendo ou substituindo recursos de servidor temporariamente ou permanentemente mantendo localizadores estáveis para esses recursos</span><span class="sxs-lookup"><span data-stu-id="a11ea-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="a11ea-110">Dividir em diferentes aplicativos ou áreas de um aplicativo de processamento de solicitação</span><span class="sxs-lookup"><span data-stu-id="a11ea-110">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="a11ea-111">Removendo, adicionando ou reorganizando segmentos de URL em solicitações de entrada</span><span class="sxs-lookup"><span data-stu-id="a11ea-111">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="a11ea-112">Otimizando URLs públicas para otimização de mecanismo de pesquisa (SEO)</span><span class="sxs-lookup"><span data-stu-id="a11ea-112">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="a11ea-113">Permitir o uso de URLs amigáveis do públicos ajudam as pessoas a prever o conteúdo que eles encontrarão seguindo um link</span><span class="sxs-lookup"><span data-stu-id="a11ea-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="a11ea-114">Redirecionar solicitações não seguras para proteger pontos de extremidade</span><span class="sxs-lookup"><span data-stu-id="a11ea-114">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="a11ea-115">Impedindo hotlinking de imagem</span><span class="sxs-lookup"><span data-stu-id="a11ea-115">Preventing image hotlinking</span></span>

<span data-ttu-id="a11ea-116">Você pode definir regras para alterar a URL de várias maneiras, incluindo regex, regras de módulo mod_rewrite Apache, regras de módulo de reescrita de IIS e usando lógica de regra personalizada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-116">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="a11ea-117">Este documento apresenta a regravação de URL com instruções sobre como usar o Middleware de regravação de URL em aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a11ea-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="a11ea-118">Regravação de URL pode reduzir o desempenho de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="a11ea-119">Sempre que possível, você deve limitar o número e a complexidade de regras.</span><span class="sxs-lookup"><span data-stu-id="a11ea-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="a11ea-120">Redirecionamento de URL e a URL de reconfiguração</span><span class="sxs-lookup"><span data-stu-id="a11ea-120">URL redirect and URL rewrite</span></span>
<span data-ttu-id="a11ea-121">A diferença na frase entre *de redirecionamento de URL* e *regravação de URL* pode parecer sutis no primeiro, mas tem implicações importantes para fornecer recursos aos clientes.</span><span class="sxs-lookup"><span data-stu-id="a11ea-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="a11ea-122">Middleware de regravação de URL do ASP.NET Core é capaz de atender às necessidades de ambos.</span><span class="sxs-lookup"><span data-stu-id="a11ea-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="a11ea-123">Um *de redirecionamento de URL* é uma operação de cliente, em que o cliente é instruído para acessar um recurso em outro endereço.</span><span class="sxs-lookup"><span data-stu-id="a11ea-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="a11ea-124">Isso requer uma viagem para o servidor.</span><span class="sxs-lookup"><span data-stu-id="a11ea-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="a11ea-125">A URL de redirecionamento retornada ao cliente aparece na barra de endereços do navegador quando o cliente faz uma nova solicitação para o recurso.</span><span class="sxs-lookup"><span data-stu-id="a11ea-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="a11ea-126">Se `/resource` é *redirecionado* para `/different-resource`, as solicitações do cliente `/resource`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="a11ea-127">O servidor responde que o cliente deve obter o recurso no `/different-resource` com um código de status que indica que o redirecionamento é temporário ou permanente.</span><span class="sxs-lookup"><span data-stu-id="a11ea-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="a11ea-128">O cliente executa uma nova solicitação para o recurso na URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="a11ea-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Um ponto de extremidade de serviço WebAPI foi alterado temporariamente da versão 1 (v1) para a versão 2 (v2) no servidor.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="a11ea-134">Ao redirecionar solicitações para uma URL diferente, você indica se o redirecionamento é permanente ou temporária.</span><span class="sxs-lookup"><span data-stu-id="a11ea-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="a11ea-135">O código de status (movido permanentemente) 301 é usado em que o recurso tem uma nova URL permanente e você desejar para instruir o cliente que todas as solicitações futuras para o recurso devem usar a nova URL.</span><span class="sxs-lookup"><span data-stu-id="a11ea-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="a11ea-136">*O cliente pode armazenar em cache a resposta quando um código de 301 status é recebido.*</span><span class="sxs-lookup"><span data-stu-id="a11ea-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="a11ea-137">O código de status 302 (não encontrado) é usado em que o redirecionamento é temporário ou geralmente assunto para alterar, de modo que o cliente não deve armazenar e reutilizar a URL de redirecionamento no futuro.</span><span class="sxs-lookup"><span data-stu-id="a11ea-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="a11ea-138">Para obter mais informações, consulte [RFC 2616: definições de código de Status](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a11ea-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="a11ea-139">Um *regravação de URL* é uma operação do servidor para fornecer um recurso de um endereço de recurso diferente.</span><span class="sxs-lookup"><span data-stu-id="a11ea-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="a11ea-140">Reconfiguração de uma URL não requer uma viagem para o servidor.</span><span class="sxs-lookup"><span data-stu-id="a11ea-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="a11ea-141">A URL regravada não é retornada ao cliente e não aparecerá na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="a11ea-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="a11ea-142">Quando `/resource` é *reescrita* para `/different-resource`, as solicitações do cliente `/resource`e o servidor *internamente* busca o recurso no `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="a11ea-143">Embora o cliente possa ser capaz de recuperar o recurso na URL regravada, o cliente não ser informado que o recurso existe na URL regravada quando ele faz a solicitação e recebe a resposta.</span><span class="sxs-lookup"><span data-stu-id="a11ea-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Um ponto de extremidade de serviço WebAPI foi alterado da versão 1 (v1) para a versão 2 (v2) no servidor.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="a11ea-148">Aplicativo de exemplo de regravação de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-148">URL rewriting sample app</span></span>
<span data-ttu-id="a11ea-149">Você pode explorar os recursos de Middleware de regravação de URL com o [aplicativo de exemplo de regravação de URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="a11ea-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="a11ea-150">O aplicativo se aplica a regravação e regras de redirecionamento e mostra a URL redirecionada ou reconfigurada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="a11ea-151">Quando usar o Middleware de regravação de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-151">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="a11ea-152">Usar o Middleware de regravação de URL quando não for possível usar o [módulo de reescrita de URL](https://www.iis.net/downloads/microsoft/url-rewrite) com o IIS no Windows Server, o [módulo do Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) no servidor do Apache, [regravação de URL em Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), ou seu aplicativo é hospedado em [servidor HTTP. sys](xref:fundamentals/servers/httpsys) (anteriormente chamado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a11ea-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="a11ea-153">As principais razões para usar as tecnologias regravação de URL com base em servidor no IIS, o Apache ou Nginx são que o middleware não oferece suporte os recursos completos desses módulos e o desempenho do middleware provavelmente não corresponde dos módulos.</span><span class="sxs-lookup"><span data-stu-id="a11ea-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="a11ea-154">No entanto, há alguns recursos dos módulos de servidor que não funcionam com projetos do ASP.NET Core, como o `IsFile` e `IsDirectory` restrições do módulo de reescrita de IIS.</span><span class="sxs-lookup"><span data-stu-id="a11ea-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="a11ea-155">Nesses cenários, use o middleware.</span><span class="sxs-lookup"><span data-stu-id="a11ea-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="a11ea-156">Pacote</span><span class="sxs-lookup"><span data-stu-id="a11ea-156">Package</span></span>
<span data-ttu-id="a11ea-157">Para incluir o middleware em seu projeto, adicione uma referência para o [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pacote.</span><span class="sxs-lookup"><span data-stu-id="a11ea-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="a11ea-158">Este recurso está disponível para aplicativos que se destinam a ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="a11ea-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="a11ea-159">Extensão e opções</span><span class="sxs-lookup"><span data-stu-id="a11ea-159">Extension and options</span></span>
<span data-ttu-id="a11ea-160">Estabelecer a regravação de URL e regras de redirecionamento, criando uma instância do `RewriteOptions` classe com métodos de extensão para cada uma das suas regras.</span><span class="sxs-lookup"><span data-stu-id="a11ea-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="a11ea-161">Várias regras na ordem em que você gostaria que elas processado da cadeia.</span><span class="sxs-lookup"><span data-stu-id="a11ea-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="a11ea-162">O `RewriteOptions` são passados para o Middleware de regravação de URL, como ele é adicionado ao pipeline de solicitação com `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="a11ea-165">Redirecionamento de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-165">URL redirect</span></span>
<span data-ttu-id="a11ea-166">Use `AddRedirect` para redirecionar as solicitações.</span><span class="sxs-lookup"><span data-stu-id="a11ea-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="a11ea-167">O primeiro parâmetro contém o regex para correspondência no caminho da URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="a11ea-168">O segundo parâmetro é a cadeia de caracteres de substituição.</span><span class="sxs-lookup"><span data-stu-id="a11ea-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="a11ea-169">O terceiro parâmetro, se presente, especifica o código de status.</span><span class="sxs-lookup"><span data-stu-id="a11ea-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="a11ea-170">Se você não especificar o código de status, o padrão é 302 (não encontrado), que indica que o recurso é movido temporariamente ou substituído.</span><span class="sxs-lookup"><span data-stu-id="a11ea-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="a11ea-173">Em um navegador com as ferramentas de desenvolvedor habilitado, fazer uma solicitação para o aplicativo de exemplo com o caminho `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="a11ea-174">A regex corresponda ao caminho de solicitação em `redirect-rule/(.*)`, e o caminho é substituído por `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="a11ea-175">O redirecionamento de URL é enviada de volta ao cliente com um código de status 302 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="a11ea-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="a11ea-176">O navegador faz uma solicitação de novo na URL de redirecionamento, que aparece na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="a11ea-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="a11ea-177">Uma vez que nenhuma regra no aplicativo de exemplo corresponde a URL de redirecionamento, a segunda solicitação recebe uma resposta 200 do (Okey) do aplicativo e o corpo da resposta mostra a URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="a11ea-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="a11ea-178">Uma ida e volta é feita para o servidor quando uma URL é *redirecionado*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="a11ea-179">Tenha cuidado ao estabelecer suas regras de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="a11ea-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="a11ea-180">As regras de redirecionamento são avaliadas em cada solicitação para o aplicativo, incluindo depois de um redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="a11ea-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="a11ea-181">É fácil acidentalmente criar um loop de redirecionamentos infinitos.</span><span class="sxs-lookup"><span data-stu-id="a11ea-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="a11ea-182">Solicitação original:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="a11ea-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="a11ea-184">A parte da expressão contida dentro dos parênteses é chamada uma *grupo de captura*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="a11ea-185">O ponto final (`.`) da expressão significa *corresponder qualquer caractere*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="a11ea-186">O asterisco (`*`) indica *corresponde ao caractere precedente zero ou mais vezes*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="a11ea-187">Portanto, os segmentos de duas últimas caminho da URL, `1234/5678`, são capturados pelo grupo de captura `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="a11ea-188">Qualquer valor que você fornecer a URL de solicitação após `redirect-rule/` é capturado por esse grupo de captura único.</span><span class="sxs-lookup"><span data-stu-id="a11ea-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="a11ea-189">Na cadeia de caracteres de substituição, grupos capturados são injetados na cadeia de caracteres com o símbolo de dólar (`$`) seguido pelo número de sequência da captura.</span><span class="sxs-lookup"><span data-stu-id="a11ea-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="a11ea-190">O primeiro valor de grupo de captura é obtido com `$1`, a segunda com `$2`, e eles continuam em sequência para os grupos de captura em sua regex.</span><span class="sxs-lookup"><span data-stu-id="a11ea-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="a11ea-191">Há apenas um grupo capturado no regex de regra de redirecionamento no aplicativo de exemplo, para que haja apenas um grupo inserido na cadeia de caracteres de substituição, que é `$1`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="a11ea-192">Quando a regra é aplicada, torna-se a URL `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="a11ea-193">Redirecionamento de URL para um ponto de extremidade seguro</span><span class="sxs-lookup"><span data-stu-id="a11ea-193">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="a11ea-194">Use `AddRedirectToHttps` para redirecionar solicitações HTTP para o mesmo host e caminho usando HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="a11ea-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="a11ea-195">Se o código de status não for fornecido, o middleware padrão 302 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="a11ea-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="a11ea-196">Se a porta não for fornecida, o middleware padrão `null`, que significa que o protocolo é alterado para `https://` e o cliente acessa o recurso na porta 443.</span><span class="sxs-lookup"><span data-stu-id="a11ea-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="a11ea-197">O exemplo mostra como definir o código de status para 301 (movido permanentemente) e altere a porta para 5001.</span><span class="sxs-lookup"><span data-stu-id="a11ea-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="a11ea-198">Use `AddRedirectToHttpsPermanent` para redirecionar solicitações não seguras para o mesmo host e o caminho com o protocolo HTTPS seguro (`https://` na porta 443).</span><span class="sxs-lookup"><span data-stu-id="a11ea-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="a11ea-199">O middleware define o código de status 301 (movido permanentemente).</span><span class="sxs-lookup"><span data-stu-id="a11ea-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="a11ea-200">O aplicativo de exemplo é capaz de demonstrar como usar `AddRedirectToHttps` ou `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-200">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="a11ea-201">Adicione o método de extensão para o `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-201">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="a11ea-202">Fazer uma solicitação não segura para o aplicativo em qualquer URL.</span><span class="sxs-lookup"><span data-stu-id="a11ea-202">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="a11ea-203">Ignore a aviso que o certificado autoassinado não é confiável de segurança do navegador.</span><span class="sxs-lookup"><span data-stu-id="a11ea-203">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="a11ea-204">Solicitação original usando `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="a11ea-204">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="a11ea-206">Solicitação original usando `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="a11ea-206">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="a11ea-208">Regravar URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-208">URL rewrite</span></span>
<span data-ttu-id="a11ea-209">Use `AddRewrite` para criar uma regra de regravação de URLs.</span><span class="sxs-lookup"><span data-stu-id="a11ea-209">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="a11ea-210">O primeiro parâmetro contém o regex para correspondência no caminho de URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-210">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="a11ea-211">O segundo parâmetro é a cadeia de caracteres de substituição.</span><span class="sxs-lookup"><span data-stu-id="a11ea-211">The second parameter is the replacement string.</span></span> <span data-ttu-id="a11ea-212">O terceiro parâmetro, `skipRemainingRules: {true|false}`, indica se deve ou não ignorar regras de regravação adicionais se a regra atual é aplicada para o middleware.</span><span class="sxs-lookup"><span data-stu-id="a11ea-212">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-213">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="a11ea-215">Solicitação original:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="a11ea-215">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor de rastreamento de solicitação e resposta](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="a11ea-217">A primeira coisa que você observar no regex é o circunflexo (`^`) no início da expressão.</span><span class="sxs-lookup"><span data-stu-id="a11ea-217">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="a11ea-218">Isso significa que a correspondência começa no início do caminho da URL.</span><span class="sxs-lookup"><span data-stu-id="a11ea-218">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="a11ea-219">No exemplo anterior com a regra de redirecionamento, `redirect-rule/(.*)`, não há nenhum circunflexo no início do regex; portanto, qualquer caractere pode preceder `redirect-rule/` no caminho para uma correspondência com êxito.</span><span class="sxs-lookup"><span data-stu-id="a11ea-219">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="a11ea-220">Caminho</span><span class="sxs-lookup"><span data-stu-id="a11ea-220">Path</span></span>                               | <span data-ttu-id="a11ea-221">Corresponder a</span><span class="sxs-lookup"><span data-stu-id="a11ea-221">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="a11ea-222">Sim</span><span class="sxs-lookup"><span data-stu-id="a11ea-222">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="a11ea-223">Sim</span><span class="sxs-lookup"><span data-stu-id="a11ea-223">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="a11ea-224">Sim</span><span class="sxs-lookup"><span data-stu-id="a11ea-224">Yes</span></span>   |

<span data-ttu-id="a11ea-225">A regra de regravação `^rewrite-rule/(\d+)/(\d+)`, corresponde apenas a caminhos se forem iniciados com `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-225">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="a11ea-226">Observe a diferença de correspondência entre a regra de regravação abaixo e a regra de redirecionamento acima.</span><span class="sxs-lookup"><span data-stu-id="a11ea-226">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="a11ea-227">Caminho</span><span class="sxs-lookup"><span data-stu-id="a11ea-227">Path</span></span>                              | <span data-ttu-id="a11ea-228">Corresponder a</span><span class="sxs-lookup"><span data-stu-id="a11ea-228">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="a11ea-229">Sim</span><span class="sxs-lookup"><span data-stu-id="a11ea-229">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="a11ea-230">Não</span><span class="sxs-lookup"><span data-stu-id="a11ea-230">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="a11ea-231">Não</span><span class="sxs-lookup"><span data-stu-id="a11ea-231">No</span></span>    |

<span data-ttu-id="a11ea-232">Após o `^rewrite-rule/` parte da expressão, há dois grupos de captura, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-232">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="a11ea-233">O `\d` significa *corresponder um dígito (número)*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-233">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="a11ea-234">O sinal de adição (`+`) significa *corresponde a um ou mais o caractere anterior*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-234">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="a11ea-235">Portanto, a URL deve conter um número seguido por uma barra seguida por outro número.</span><span class="sxs-lookup"><span data-stu-id="a11ea-235">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="a11ea-236">Esses grupos são injetados para a URL regravada como de captura `$1` e `$2`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-236">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="a11ea-237">A cadeia de caracteres de substituição de regra de regravação coloca os grupos capturados em querystring.</span><span class="sxs-lookup"><span data-stu-id="a11ea-237">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="a11ea-238">O caminho solicitado de `/rewrite-rule/1234/5678` foi reescrito para obter o recurso no `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-238">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="a11ea-239">Se uma querystring estiver presente na solicitação original, ele é preservado quando a URL é recriada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-239">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="a11ea-240">Não há nenhum ida e volta ao servidor para obter o recurso.</span><span class="sxs-lookup"><span data-stu-id="a11ea-240">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="a11ea-241">Se o recurso existir, ela tem buscadas e retornada ao cliente com um código de status (Okey) 200.</span><span class="sxs-lookup"><span data-stu-id="a11ea-241">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="a11ea-242">Porque o cliente não for redirecionado, não altere a URL na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="a11ea-242">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="a11ea-243">Quanto o cliente está interessado, a operação de regravação de URL nunca ocorreu.</span><span class="sxs-lookup"><span data-stu-id="a11ea-243">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="a11ea-244">Use `skipRemainingRules: true` sempre que possível, porque as regras de correspondência é um processo caro e reduz o tempo de resposta do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-244">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="a11ea-245">Para a resposta mais rápida do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="a11ea-245">For the fastest app response:</span></span>
> * <span data-ttu-id="a11ea-246">Ordene suas regras de reescrita da regra de correspondência com mais frequência para a regra de correspondência com menos frequência.</span><span class="sxs-lookup"><span data-stu-id="a11ea-246">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="a11ea-247">Ignore o processamento das regras restantes quando ocorre uma correspondência e nenhum processamento de regra adicional é necessário.</span><span class="sxs-lookup"><span data-stu-id="a11ea-247">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="a11ea-248">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="a11ea-248">Apache mod_rewrite</span></span>
<span data-ttu-id="a11ea-249">Aplicar regras de mod_rewrite Apache com `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-249">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="a11ea-250">Certifique-se de que o arquivo de regras é implantado com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-250">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="a11ea-251">Para obter mais informações e exemplos de regras mod_rewrite, consulte [mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="a11ea-251">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a11ea-253">Um `StreamReader` é usado para ler as regras do *ApacheModRewrite.txt* arquivo de regras.</span><span class="sxs-lookup"><span data-stu-id="a11ea-253">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a11ea-255">O primeiro parâmetro aceita um `IFileProvider`, que é fornecido por meio de [injeção de dependência](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="a11ea-255">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="a11ea-256">O `IHostingEnvironment` é injetado para fornecer o `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-256">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="a11ea-257">O segundo parâmetro é o caminho para o arquivo de regras, que é *ApacheModRewrite.txt* no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-257">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="a11ea-258">O aplicativo de exemplo redireciona solicitações de `/apache-mod-rules-redirect/(.\*)` para `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-258">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="a11ea-259">O código de status de resposta é 302 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="a11ea-259">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="a11ea-260">Solicitação original:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="a11ea-260">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="a11ea-262">Variáveis de servidor com suporte</span><span class="sxs-lookup"><span data-stu-id="a11ea-262">Supported server variables</span></span>
<span data-ttu-id="a11ea-263">O middleware suporta as seguintes variáveis de servidor Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="a11ea-263">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="a11ea-264">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="a11ea-264">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="a11ea-265">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="a11ea-265">HTTP_ACCEPT</span></span>
* <span data-ttu-id="a11ea-266">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="a11ea-266">HTTP_CONNECTION</span></span>
* <span data-ttu-id="a11ea-267">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="a11ea-267">HTTP_COOKIE</span></span>
* <span data-ttu-id="a11ea-268">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="a11ea-268">HTTP_FORWARDED</span></span>
* <span data-ttu-id="a11ea-269">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="a11ea-269">HTTP_HOST</span></span>
* <span data-ttu-id="a11ea-270">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="a11ea-270">HTTP_REFERER</span></span>
* <span data-ttu-id="a11ea-271">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="a11ea-271">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="a11ea-272">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a11ea-272">HTTPS</span></span>
* <span data-ttu-id="a11ea-273">IPV6</span><span class="sxs-lookup"><span data-stu-id="a11ea-273">IPV6</span></span>
* <span data-ttu-id="a11ea-274">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="a11ea-274">QUERY_STRING</span></span>
* <span data-ttu-id="a11ea-275">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="a11ea-275">REMOTE_ADDR</span></span>
* <span data-ttu-id="a11ea-276">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="a11ea-276">REMOTE_PORT</span></span>
* <span data-ttu-id="a11ea-277">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="a11ea-277">REQUEST_FILENAME</span></span>
* <span data-ttu-id="a11ea-278">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="a11ea-278">REQUEST_METHOD</span></span>
* <span data-ttu-id="a11ea-279">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="a11ea-279">REQUEST_SCHEME</span></span>
* <span data-ttu-id="a11ea-280">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="a11ea-280">REQUEST_URI</span></span>
* <span data-ttu-id="a11ea-281">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="a11ea-281">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="a11ea-282">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="a11ea-282">SERVER_ADDR</span></span>
* <span data-ttu-id="a11ea-283">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="a11ea-283">SERVER_PORT</span></span>
* <span data-ttu-id="a11ea-284">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="a11ea-284">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="a11ea-285">TEMPO</span><span class="sxs-lookup"><span data-stu-id="a11ea-285">TIME</span></span>
* <span data-ttu-id="a11ea-286">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="a11ea-286">TIME_DAY</span></span>
* <span data-ttu-id="a11ea-287">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="a11ea-287">TIME_HOUR</span></span>
* <span data-ttu-id="a11ea-288">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="a11ea-288">TIME_MIN</span></span>
* <span data-ttu-id="a11ea-289">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="a11ea-289">TIME_MON</span></span>
* <span data-ttu-id="a11ea-290">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="a11ea-290">TIME_SEC</span></span>
* <span data-ttu-id="a11ea-291">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="a11ea-291">TIME_WDAY</span></span>
* <span data-ttu-id="a11ea-292">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="a11ea-292">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="a11ea-293">Regras do módulo de reescrita de URL do IIS</span><span class="sxs-lookup"><span data-stu-id="a11ea-293">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="a11ea-294">Para usar as regras que se aplicam para o módulo de reescrita de URL do IIS, use `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-294">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="a11ea-295">Certifique-se de que o arquivo de regras é implantado com o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-295">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="a11ea-296">Não direta de middleware para usar o *Web. config* arquivo quando em execução no IIS do Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a11ea-296">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="a11ea-297">Com o IIS, essas regras devem ser armazenadas fora do seu *Web. config* para evitar conflitos com o módulo de reescrita de IIS.</span><span class="sxs-lookup"><span data-stu-id="a11ea-297">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="a11ea-298">Para obter mais informações e exemplos de regras do módulo de reescrita de URL do IIS, consulte [usando o Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [URL de referência de configuração de módulo reescreva](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="a11ea-298">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-299">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-299">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a11ea-300">Um `StreamReader` é usado para ler as regras do *IISUrlRewrite.xml* arquivo de regras.</span><span class="sxs-lookup"><span data-stu-id="a11ea-300">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a11ea-302">O primeiro parâmetro aceita um `IFileProvider`, enquanto o segundo parâmetro é o caminho para o arquivo de regras XML, que é *IISUrlRewrite.xml* no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="a11ea-303">O aplicativo de exemplo reconfigura as solicitações de `/iis-rules-rewrite/(.*)` para `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="a11ea-304">A resposta será enviada ao cliente com um código de status 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="a11ea-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="a11ea-305">Solicitação original:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="a11ea-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor de rastreamento de solicitação e resposta](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="a11ea-307">Se você tiver um módulo de reescrita de IIS ativa com as regras de nível de servidor configuradas que poderia afetar seu aplicativo de maneira indesejada, você pode desabilitar o módulo de reescrita de IIS para um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a11ea-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="a11ea-308">Para obter mais informações, consulte [módulos do IIS desabilitando](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="a11ea-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="a11ea-309">Recursos sem suporte</span><span class="sxs-lookup"><span data-stu-id="a11ea-309">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a11ea-311">O middleware liberado com o ASP.NET Core 2. x não oferece suporte para os seguintes recursos do IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="a11ea-311">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="a11ea-312">Regras de saída</span><span class="sxs-lookup"><span data-stu-id="a11ea-312">Outbound Rules</span></span>
* <span data-ttu-id="a11ea-313">Variáveis de servidor personalizado</span><span class="sxs-lookup"><span data-stu-id="a11ea-313">Custom Server Variables</span></span>
* <span data-ttu-id="a11ea-314">Curingas</span><span class="sxs-lookup"><span data-stu-id="a11ea-314">Wildcards</span></span>
* <span data-ttu-id="a11ea-315">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="a11ea-315">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a11ea-317">O middleware liberado com o ASP.NET Core 1. x não oferece suporte para os seguintes recursos do IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="a11ea-317">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="a11ea-318">Regras globais</span><span class="sxs-lookup"><span data-stu-id="a11ea-318">Global Rules</span></span>
* <span data-ttu-id="a11ea-319">Regras de saída</span><span class="sxs-lookup"><span data-stu-id="a11ea-319">Outbound Rules</span></span>
* <span data-ttu-id="a11ea-320">Mapas de regravação</span><span class="sxs-lookup"><span data-stu-id="a11ea-320">Rewrite Maps</span></span>
* <span data-ttu-id="a11ea-321">Ação de CustomResponse</span><span class="sxs-lookup"><span data-stu-id="a11ea-321">CustomResponse Action</span></span>
* <span data-ttu-id="a11ea-322">Variáveis de servidor personalizado</span><span class="sxs-lookup"><span data-stu-id="a11ea-322">Custom Server Variables</span></span>
* <span data-ttu-id="a11ea-323">Curingas</span><span class="sxs-lookup"><span data-stu-id="a11ea-323">Wildcards</span></span>
* <span data-ttu-id="a11ea-324">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="a11ea-324">Action:CustomResponse</span></span>
* <span data-ttu-id="a11ea-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="a11ea-325">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="a11ea-326">Variáveis de servidor com suporte</span><span class="sxs-lookup"><span data-stu-id="a11ea-326">Supported server variables</span></span>
<span data-ttu-id="a11ea-327">O middleware suporta as seguintes variáveis de servidor IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="a11ea-327">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="a11ea-328">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="a11ea-328">CONTENT_LENGTH</span></span>
* <span data-ttu-id="a11ea-329">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="a11ea-329">CONTENT_TYPE</span></span>
* <span data-ttu-id="a11ea-330">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="a11ea-330">HTTP_ACCEPT</span></span>
* <span data-ttu-id="a11ea-331">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="a11ea-331">HTTP_CONNECTION</span></span>
* <span data-ttu-id="a11ea-332">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="a11ea-332">HTTP_COOKIE</span></span>
* <span data-ttu-id="a11ea-333">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="a11ea-333">HTTP_HOST</span></span>
* <span data-ttu-id="a11ea-334">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="a11ea-334">HTTP_REFERER</span></span>
* <span data-ttu-id="a11ea-335">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-335">HTTP_URL</span></span>
* <span data-ttu-id="a11ea-336">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="a11ea-336">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="a11ea-337">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a11ea-337">HTTPS</span></span>
* <span data-ttu-id="a11ea-338">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="a11ea-338">LOCAL_ADDR</span></span>
* <span data-ttu-id="a11ea-339">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="a11ea-339">QUERY_STRING</span></span>
* <span data-ttu-id="a11ea-340">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="a11ea-340">REMOTE_ADDR</span></span>
* <span data-ttu-id="a11ea-341">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="a11ea-341">REMOTE_PORT</span></span>
* <span data-ttu-id="a11ea-342">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="a11ea-342">REQUEST_FILENAME</span></span>
* <span data-ttu-id="a11ea-343">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="a11ea-343">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="a11ea-344">Você também pode obter um `IFileProvider` por meio de um `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-344">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="a11ea-345">Essa abordagem pode fornecer maior flexibilidade para o local da sua reescrita arquivos de regras.</span><span class="sxs-lookup"><span data-stu-id="a11ea-345">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="a11ea-346">Certifique-se de que os arquivos de regras de regravação são implantados no servidor no caminho que você fornecer.</span><span class="sxs-lookup"><span data-stu-id="a11ea-346">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="a11ea-347">Regra com base em método:</span><span class="sxs-lookup"><span data-stu-id="a11ea-347">Method-based rule</span></span>
<span data-ttu-id="a11ea-348">Use `Add(Action<RewriteContext> applyRule)` para implementar sua própria lógica de regra em um método.</span><span class="sxs-lookup"><span data-stu-id="a11ea-348">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="a11ea-349">O `RewriteContext` expõe o `HttpContext` para uso em seu método.</span><span class="sxs-lookup"><span data-stu-id="a11ea-349">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="a11ea-350">O `context.Result` determina o pipeline adicional como o processamento é tratado.</span><span class="sxs-lookup"><span data-stu-id="a11ea-350">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="a11ea-351">context.Result</span><span class="sxs-lookup"><span data-stu-id="a11ea-351">context.Result</span></span>                       | <span data-ttu-id="a11ea-352">Ação</span><span class="sxs-lookup"><span data-stu-id="a11ea-352">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="a11ea-353">`RuleResult.ContinueRules` (padrão)</span><span class="sxs-lookup"><span data-stu-id="a11ea-353">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="a11ea-354">Continue a aplicar regras</span><span class="sxs-lookup"><span data-stu-id="a11ea-354">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="a11ea-355">Parar de aplicar regras e enviar a resposta</span><span class="sxs-lookup"><span data-stu-id="a11ea-355">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="a11ea-356">Parar de aplicar regras e enviar o contexto para o próximo middleware</span><span class="sxs-lookup"><span data-stu-id="a11ea-356">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-357">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-357">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-358">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-358">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="a11ea-359">O aplicativo de exemplo demonstra um método que redirecione as solicitações para caminhos que terminam com *. XML*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-359">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="a11ea-360">Se você fizer uma solicitação `/file.xml`, ele é redirecionado para `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-360">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="a11ea-361">O código de status é definido como 301 (movido permanentemente).</span><span class="sxs-lookup"><span data-stu-id="a11ea-361">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="a11ea-362">Para um redirecionamento, você deve definir explicitamente o código de status da resposta; Caso contrário, será retornado um código de status 200 (Okey) e o redirecionamento não ocorrerá no cliente.</span><span class="sxs-lookup"><span data-stu-id="a11ea-362">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="a11ea-365">Solicitação original:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="a11ea-365">Original Request: `/file.xml`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor controle as solicitações e respostas para File. XML](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="a11ea-367">Regra com base em IRule</span><span class="sxs-lookup"><span data-stu-id="a11ea-367">IRule-based rule</span></span>
<span data-ttu-id="a11ea-368">Use `Add(IRule)` para implementar sua própria lógica de regra em uma classe que deriva de `IRule`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="a11ea-369">Usando um `IRule` fornece maior flexibilidade em relação a usar a abordagem de regra com base em método.</span><span class="sxs-lookup"><span data-stu-id="a11ea-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="a11ea-370">A classe derivada pode incluir um construtor, onde você pode passar parâmetros para o `ApplyRule` método.</span><span class="sxs-lookup"><span data-stu-id="a11ea-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="a11ea-373">Os valores dos parâmetros no aplicativo de exemplo para o `extension` e `newPath` são verificados para atender a várias condições.</span><span class="sxs-lookup"><span data-stu-id="a11ea-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="a11ea-374">O `extension` deve conter um valor, e o valor deve ser *. PNG*, *. jpg*, ou *. gif*.</span><span class="sxs-lookup"><span data-stu-id="a11ea-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="a11ea-375">Se o `newPath` não é válido, um `ArgumentException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="a11ea-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="a11ea-376">Se você fizer uma solicitação *image.png*, ele é redirecionado para `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="a11ea-377">Se você fizer uma solicitação *image.jpg*, ele é redirecionado para `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="a11ea-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="a11ea-378">O código de status é definido como 301 (movido permanentemente) e o `context.Result` é definida para parar o processamento de regras e enviar a resposta.</span><span class="sxs-lookup"><span data-stu-id="a11ea-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a11ea-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a11ea-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a11ea-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="a11ea-381">Solicitação original:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="a11ea-381">Original Request: `/image.png`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas para image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="a11ea-383">Solicitação original:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="a11ea-383">Original Request: `/image.jpg`</span></span>

![Janela do navegador com as ferramentas de desenvolvedor do controle as solicitações e respostas para image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="a11ea-385">Exemplos de regex</span><span class="sxs-lookup"><span data-stu-id="a11ea-385">Regex examples</span></span>

| <span data-ttu-id="a11ea-386">Goal</span><span class="sxs-lookup"><span data-stu-id="a11ea-386">Goal</span></span> | <span data-ttu-id="a11ea-387">Cadeia de caracteres regex &</span><span class="sxs-lookup"><span data-stu-id="a11ea-387">Regex String &</span></span><br><span data-ttu-id="a11ea-388">Exemplo de correspondência</span><span class="sxs-lookup"><span data-stu-id="a11ea-388">Match Example</span></span> | <span data-ttu-id="a11ea-389">Cadeia de caracteres de substituição &</span><span class="sxs-lookup"><span data-stu-id="a11ea-389">Replacement String &</span></span><br><span data-ttu-id="a11ea-390">Exemplo de saída</span><span class="sxs-lookup"><span data-stu-id="a11ea-390">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="a11ea-391">Reescreva o caminho em querystring</span><span class="sxs-lookup"><span data-stu-id="a11ea-391">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="a11ea-392">Barra à direita da faixa</span><span class="sxs-lookup"><span data-stu-id="a11ea-392">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="a11ea-393">Impor a barra à direita</span><span class="sxs-lookup"><span data-stu-id="a11ea-393">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="a11ea-394">Evitar a reconfiguração de solicitações específicas</span><span class="sxs-lookup"><span data-stu-id="a11ea-394">Avoid rewriting specific requests</span></span> | <span data-ttu-id="a11ea-395">`^(.*)(?<!\.axd)$` ou `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="a11ea-395">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="a11ea-396">Sim:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="a11ea-396">Yes: `/resource.htm`</span></span><br><span data-ttu-id="a11ea-397">Não:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="a11ea-397">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="a11ea-398">Reorganizar os segmentos de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-398">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="a11ea-399">Substituir um segmento de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-399">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="a11ea-400">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="a11ea-400">Additional resources</span></span>
* [<span data-ttu-id="a11ea-401">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="a11ea-401">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="a11ea-402">Middleware</span><span class="sxs-lookup"><span data-stu-id="a11ea-402">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="a11ea-403">Expressões regulares no .NET</span><span class="sxs-lookup"><span data-stu-id="a11ea-403">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="a11ea-404">Linguagem de expressão regular – referência rápida</span><span class="sxs-lookup"><span data-stu-id="a11ea-404">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="a11ea-405">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="a11ea-405">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="a11ea-406">Usando o módulo de reescrita de Url 2.0 (para IIS)</span><span class="sxs-lookup"><span data-stu-id="a11ea-406">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="a11ea-407">Referência de configuração do módulo de regravação de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-407">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="a11ea-408">Fórum de módulo de regravação de URL de IIS</span><span class="sxs-lookup"><span data-stu-id="a11ea-408">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="a11ea-409">Manter uma estrutura simples de URL</span><span class="sxs-lookup"><span data-stu-id="a11ea-409">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="a11ea-410">Regravação de URL 10 dicas e truques</span><span class="sxs-lookup"><span data-stu-id="a11ea-410">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="a11ea-411">Barra ou não de barra</span><span class="sxs-lookup"><span data-stu-id="a11ea-411">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)