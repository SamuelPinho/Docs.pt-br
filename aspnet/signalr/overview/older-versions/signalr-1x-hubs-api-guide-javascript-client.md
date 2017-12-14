---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Guia de API de Hubs do SignalR 1. x - JavaScript cliente | Microsoft Docs
author: pfletcher
description: "Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 1.1 em clientes de JavaScript, como navegadores e da Windows Store (WinJS) applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 56931827a1a1edf003d2662b2d36964b9b6f3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="5c0fb-103">Guia de API de Hubs do SignalR 1. x - cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="5c0fb-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="5c0fb-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5c0fb-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="5c0fb-105">Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 1.1 em clientes de JavaScript, como navegadores e aplicativos da Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="5c0fb-106">A API de Hubs de SignalR permite fazer chamadas de procedimento remoto (RPCs) de um servidor para clientes conectados e de clientes para o servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="5c0fb-107">No código do servidor, você define métodos que podem ser chamados por clientes e chamar os métodos que são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="5c0fb-108">No código do cliente, você define métodos que podem ser chamados do servidor e chamar os métodos que são executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="5c0fb-109">SignalR cuida de todos os detalhes do cliente para servidor para você.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="5c0fb-110">O SignalR também oferece uma API de nível inferior chamada conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="5c0fb-111">Para obter uma introdução para o SignalR, Hubs e conexões persistentes, ou para um tutorial que mostra como criar um aplicativo SignalR completo, consulte [SignalR - Introdução](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="5c0fb-112">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="5c0fb-112">Overview</span></span>

<span data-ttu-id="5c0fb-113">Este documento contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="5c0fb-114">O proxy gerado e o que ele faz para você</span><span class="sxs-lookup"><span data-stu-id="5c0fb-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="5c0fb-115">Quando usar o proxy gerado</span><span class="sxs-lookup"><span data-stu-id="5c0fb-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="5c0fb-116">Instalação do cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="5c0fb-117">Como referenciar o proxy gerado dinamicamente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="5c0fb-118">Como criar um arquivo físico para o SignalR gerado proxy</span><span class="sxs-lookup"><span data-stu-id="5c0fb-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="5c0fb-119">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="5c0fb-120">$. connection.hub é o mesmo objeto que $.hubConnection() cria</span><span class="sxs-lookup"><span data-stu-id="5c0fb-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="5c0fb-121">Execução assíncrona do método start</span><span class="sxs-lookup"><span data-stu-id="5c0fb-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="5c0fb-122">Como estabelecer uma conexão entre domínios</span><span class="sxs-lookup"><span data-stu-id="5c0fb-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="5c0fb-123">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="5c0fb-124">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="5c0fb-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="5c0fb-125">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="5c0fb-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="5c0fb-126">Como obter um proxy para uma classe de Hub</span><span class="sxs-lookup"><span data-stu-id="5c0fb-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="5c0fb-127">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5c0fb-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="5c0fb-128">Como chamar métodos do servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="5c0fb-129">Como manipular eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="5c0fb-130">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="5c0fb-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="5c0fb-131">Como habilitar o registro de cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="5c0fb-132">Para obter a documentação sobre como programar o servidor ou os clientes .NET, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="5c0fb-133">Guia de API de Hubs de SignalR - servidor</span><span class="sxs-lookup"><span data-stu-id="5c0fb-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="5c0fb-134">Guia de API de Hubs de SignalR - cliente .NET</span><span class="sxs-lookup"><span data-stu-id="5c0fb-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="5c0fb-135">Links para tópicos de referência de API são para a versão do .NET 4.5 da API.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="5c0fb-136">Se você estiver usando o .NET 4, consulte [a versão do .NET 4 dos tópicos da API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="5c0fb-137">O proxy gerado e o que ele faz para você</span><span class="sxs-lookup"><span data-stu-id="5c0fb-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="5c0fb-138">Você pode programar um cliente JavaScript para se comunicar com um serviço de SignalR com ou sem um proxy que SignalR gera para você.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="5c0fb-139">O que o proxy é simplificar a sintaxe do código que você usa para se conectar, métodos de gravação que o servidor chama, e chamar os métodos no servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="5c0fb-140">Quando você escrever código para chamar os métodos de servidor, o proxy gerado permite que você use a sintaxe que parece como se você estivesse executando uma função local: você pode escrever `serverMethod(arg1, arg2)` em vez de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="5c0fb-141">A sintaxe do proxy gerado também permite que um erro de imediato e inteligível do lado do cliente se você digitar um nome de método server.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="5c0fb-142">E se você criar manualmente o arquivo que define os proxies, você também pode obter suporte ao IntelliSense para escrever código que chama os métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="5c0fb-143">Por exemplo, suponha que você tem a seguinte classe de Hub no servidor:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="5c0fb-144">Os exemplos de código a seguir mostram que JavaScript código parece para invocar o `NewContosoChatMessage` método no servidor e receber invocações da `addContosoChatMessageToPage` método do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="5c0fb-145">**Com o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="5c0fb-146">**Sem o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="5c0fb-147">Quando usar o proxy gerado</span><span class="sxs-lookup"><span data-stu-id="5c0fb-147">When to use the generated proxy</span></span>

<span data-ttu-id="5c0fb-148">Se você deseja registrar vários manipuladores de eventos para um método de cliente que chama o servidor, você não pode usar o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="5c0fb-149">Caso contrário, você pode optar por usar o proxy gerado ou não com base em sua preferência de codificação.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="5c0fb-150">Se você optar por não utilizá-lo, você não precisa referenciar a URL "/ hubs de signalr" em um `script` elemento no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="5c0fb-151">Instalação do cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-151">Client setup</span></span>

<span data-ttu-id="5c0fb-152">Um cliente JavaScript requer referências para jQuery e o arquivo de JavaScript do SignalR core.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="5c0fb-153">A versão do jQuery deve ser 1.6.4 ou versões posteriores principais, como 1.7.2, 1.8.2 ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="5c0fb-154">Se você decidir usar o proxy gerado, você também precisará uma referência ao proxy SignalR gerado arquivo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="5c0fb-155">O exemplo a seguir mostra as referências de aparência em uma página HTML que usa o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="5c0fb-156">Essas referências devem ser incluídas nesta ordem: jQuery, SignalR core depois disso e proxies SignalR sobrenome.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="5c0fb-157">Como referenciar o proxy gerado dinamicamente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="5c0fb-158">No exemplo anterior, a referência ao proxy SignalR gerado é gerado dinamicamente código JavaScript, não em um arquivo físico.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="5c0fb-159">SignalR cria o código JavaScript para o proxy em tempo real e serve para o cliente em resposta à URL "hubs do signalr /".</span><span class="sxs-lookup"><span data-stu-id="5c0fb-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="5c0fb-160">Se você especificou uma URL base diferente para conexões do SignalR no servidor em seu `MapHubs` , a URL do arquivo proxy gerado dinamicamente é sua URL personalizada com "/ hubs" anexada a ele.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="5c0fb-161">Para clientes de JavaScript do Windows 8 (Windows Store), use o arquivo físico proxy em vez de gerado dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="5c0fb-162">Para obter mais informações, consulte [como criar um arquivo físico para o SignalR gerado proxy](#manualproxy) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="5c0fb-163">No modo de exibição Razor do ASP.NET MVC 4, use o til para referir-se a raiz do aplicativo em sua referência de arquivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="5c0fb-164">Para obter mais informações sobre como usar o SignalR no MVC 4, consulte [Introdução ao SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="5c0fb-165">No modo de exibição Razor do ASP.NET MVC 3, use `Url.Content` para sua referência de arquivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="5c0fb-166">Em um aplicativo de Web Forms do ASP.NET, use `ResolveClientUrl` para os proxies referência do arquivo ou registrá-lo por meio do ScriptManager usando um caminho relativo da raiz de aplicativo (começando com um til):</span><span class="sxs-lookup"><span data-stu-id="5c0fb-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="5c0fb-167">Como regra geral, use o mesmo método para especificar a URL "signalr/hubs" que você usa para arquivos CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="5c0fb-168">Se você especificar uma URL sem usar um til, em alguns cenários de seu aplicativo funcionará corretamente quando você testar no Visual Studio usando o IIS Express, mas falhará com um erro 404 quando você implanta para o IIS completo.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="5c0fb-169">Para obter mais informações, consulte **resolver referências a recursos de nível raiz** na [servidores Web no Visual Studio para projetos Web ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="5c0fb-170">Quando você executar um projeto da web no Visual Studio 2012 no modo de depuração e se você usar o Internet Explorer como navegador, você pode ver o arquivo de proxy no **Solution Explorer** em **documentos de Script**, conforme mostrado do ilustração a seguir.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Arquivo de proxy gerado JavaScript no Gerenciador de soluções](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="5c0fb-172">Para ver o conteúdo do arquivo, clique duas vezes em **hubs**.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="5c0fb-173">Se você não estiver usando o Visual Studio 2012 e do Internet Explorer, ou se você não estiver no modo de depuração, você também pode obter o conteúdo do arquivo, navegando até a URL "hubs do signalR /".</span><span class="sxs-lookup"><span data-stu-id="5c0fb-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="5c0fb-174">Por exemplo, se seu site estiver em execução em `http://localhost:56699`, vá para `http://localhost:56699/SignalR/hubs` em seu navegador.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="5c0fb-175">Como criar um arquivo físico para o SignalR gerado proxy</span><span class="sxs-lookup"><span data-stu-id="5c0fb-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="5c0fb-176">Como alternativa para o proxy gerado dinamicamente, você pode criar um arquivo físico que tem o código de proxy e fazer referência a esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="5c0fb-177">Convém fazer isso para controle de cache ou comportamento de agrupamento ou para obter o IntelliSense quando você estiver codificando chamadas para métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="5c0fb-178">Para criar um arquivo de proxy, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="5c0fb-179">Instalar o [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="5c0fb-180">Abra um prompt de comando e navegue até o *ferramentas* pasta que contém o arquivo SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="5c0fb-181">É a pasta Ferramentas no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="5c0fb-182">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="5c0fb-183">O caminho para o *. dll* é normalmente o *bin* na sua pasta de projeto.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="5c0fb-184">Este comando cria um arquivo chamado *server.js* na mesma pasta que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="5c0fb-185">Coloque o *server.js* do arquivo em uma pasta apropriada em seu projeto, renomeá-la conforme for apropriado para seu aplicativo e adicione uma referência a ele em vez da referência "/ hubs de signalr".</span><span class="sxs-lookup"><span data-stu-id="5c0fb-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="5c0fb-186">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-186">How to establish a connection</span></span>

<span data-ttu-id="5c0fb-187">Antes de estabelecer uma conexão, você precisa criar um objeto de conexão, criar um proxy e registrar manipuladores de eventos para métodos que podem ser chamados do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="5c0fb-188">Ao configurar os proxy e manipuladores de eventos, estabelece a conexão chamando o `start` método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="5c0fb-189">Se você estiver usando o proxy gerado, você não precisa criar o objeto de conexão em seu próprio código, pois o código de proxy gerado faz isso para você.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="5c0fb-190">**Estabelecer uma conexão (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="5c0fb-191">**Estabelecer uma conexão (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="5c0fb-192">O código de exemplo usa o padrão "/ signalr" URL para se conectar ao seu serviço SignalR.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="5c0fb-193">Para obter informações sobre como especificar uma URL base diferente, consulte [ASP.NET SignalR Hubs API guia - Server - a URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="5c0fb-194">Normalmente você registrar manipuladores de eventos antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="5c0fb-195">Se você deseja registrar alguns manipuladores de eventos depois de estabelecer a conexão, você pode fazer isso, mas você deve registrar pelo menos um dos seus handler(s) evento antes de chamar o `start` método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="5c0fb-196">Uma razão para isso é que pode haver muitos Hubs em um aplicativo, mas você não iria querer disparar o `OnConnected` eventos em cada Hub se apenas você pretende usar para um deles.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="5c0fb-197">Quando a conexão é estabelecida, a presença de um método de cliente no proxy do Hub é o que informa ao SignalR para disparar o `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="5c0fb-198">Se você não registrar os manipuladores de evento antes de chamar o `start` método, você poderá invocar métodos de Hub, mas o Hub `OnConnected` método não será chamado e nenhum método de cliente será chamado de servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="5c0fb-199">$. connection.hub é o mesmo objeto que $.hubConnection() cria</span><span class="sxs-lookup"><span data-stu-id="5c0fb-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="5c0fb-200">Como você pode ver exemplos, quando você usar o proxy gerado, `$.connection.hub` refere-se ao objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="5c0fb-201">Este é o mesmo objeto que você obtém chamando `$.hubConnection()` quando você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="5c0fb-202">O código de proxy gerado cria a conexão para você, executando a instrução a seguir:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Criando uma conexão no arquivo proxy gerado](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="5c0fb-204">Quando você estiver usando o proxy gerado, você pode fazer tudo com `$.connection.hub` que você pode fazer com um objeto de conexão quando você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="5c0fb-205">Execução assíncrona do método start</span><span class="sxs-lookup"><span data-stu-id="5c0fb-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="5c0fb-206">O `start` método é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="5c0fb-207">Ele retorna um [jQuery adiado objeto](http://api.jquery.com/category/deferred-object/), que significa que você pode adicionar funções de retorno de chamada por chamar métodos como `pipe`, `done`, e `fail`.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="5c0fb-208">Se você tiver o código que você deseja executar depois que a conexão é estabelecida, como uma chamada para um método de servidor, coloque esse código em uma função de retorno de chamada ou chamá-lo de uma função de retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="5c0fb-209">O `.done` método de retorno de chamada é executado depois que a conexão foi estabelecida, e depois de qualquer código que você tem em seu `OnConnected` conclui a execução de método do manipulador de eventos no servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="5c0fb-210">Se você colocar a instrução "Agora conectado" do exemplo anterior, como a próxima linha de código após a `start` chamada de método (não em um `.done` retorno de chamada), o `console.log` linha será executada antes que a conexão seja estabelecida, conforme mostrado a seguir exemplo:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Errada para escrever código que é executado após o estabelecimento de conexão](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="5c0fb-212">Como estabelecer uma conexão entre domínios</span><span class="sxs-lookup"><span data-stu-id="5c0fb-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="5c0fb-213">Normalmente se o navegador carrega uma página da `http://contoso.com`, a conexão do SignalR está no mesmo domínio, no `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="5c0fb-214">Se a página de `http://contoso.com` faz uma conexão para `http://fabrikam.com/signalr`, que é uma conexão entre domínios.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="5c0fb-215">Por motivos de segurança, conexões de domínio cruzado são desabilitadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="5c0fb-216">Para estabelecer uma conexão entre domínios, certifique-se de que as conexões de domínio cruzado são habilitadas no servidor e especificar a URL de conexão quando você cria o objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="5c0fb-217">SignalR usará a tecnologia adequada para conexões entre domínios, como [JSONP](http://en.wikipedia.org/wiki/JSONP) ou [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="5c0fb-218">No servidor, habilitar conexões de domínio cruzado, selecionando essa opção quando você chamar o `MapHubs` método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="5c0fb-219">No cliente, especifique a URL quando você cria o objeto de conexão (sem o proxy gerado) ou antes de chamar o método start (com o proxy gerado).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="5c0fb-220">**Código do cliente que especifica uma conexão de domínio cruzado (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="5c0fb-221">**Código do cliente que especifica uma conexão de domínio cruzado (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="5c0fb-222">Quando você usa o `$.hubConnection` construtor, você não precisa incluir `signalr` na URL porque ele é adicionado automaticamente (a menos que você especifique `useDefaultUrl` como `false`).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="5c0fb-223">Você pode criar várias conexões para pontos de extremidade diferentes.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="5c0fb-224">Não defina `jQuery.support.cors` como true no seu código.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Não defina jQuery.support.cors como true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="5c0fb-226">SignalR lida com o uso de JSONP ou CORS.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="5c0fb-227">Definindo `jQuery.support.cors` para true desabilita JSONP porque ele faz com que o SignalR assuma o navegador dá suporte a CORS.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="5c0fb-228">Quando você estiver se conectando a uma URL de localhost, Internet Explorer 10 não considerá-la uma conexão entre domínios, para que o aplicativo funcione localmente com o Internet Explorer 10 mesmo se você não tiver habilitado conexões entre domínios no servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="5c0fb-229">Para obter informações sobre como usar conexões entre domínios com o Internet Explorer 9, consulte [esse thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="5c0fb-230">Para obter informações sobre como usar conexões entre domínios com o Chrome, consulte [esse thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="5c0fb-231">O código de exemplo usa o padrão "/ signalr" URL para se conectar ao seu serviço SignalR.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="5c0fb-232">Para obter informações sobre como especificar uma URL base diferente, consulte [ASP.NET SignalR Hubs API guia - Server - a URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="5c0fb-233">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-233">How to configure the connection</span></span>

<span data-ttu-id="5c0fb-234">Antes de estabelecer uma conexão, você pode especificar parâmetros de cadeia de caracteres de consulta ou especificar o método de transporte.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="5c0fb-235">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="5c0fb-235">How to specify query string parameters</span></span>

<span data-ttu-id="5c0fb-236">Se você deseja enviar dados para o servidor quando o cliente se conecta, você pode adicionar parâmetros de cadeia de caracteres de consulta para o objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="5c0fb-237">Os exemplos a seguir mostram como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="5c0fb-238">**Defina um valor de cadeia de caracteres de consulta antes de chamar o método start (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="5c0fb-239">**Defina um valor de cadeia de caracteres de consulta antes de chamar o método start (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="5c0fb-240">O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="5c0fb-241">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="5c0fb-241">How to specify the transport method</span></span>

<span data-ttu-id="5c0fb-242">Como parte do processo de conexão, um cliente SignalR normalmente negocia com o servidor para determinar o melhor transporte com suporte pelo servidor e cliente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="5c0fb-243">Se você já souber qual transporte você deseja usar, você pode ignorar esse processo de negociação, especificando o método de transporte ao chamar o `start` método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="5c0fb-244">**Código do cliente que especifica o método de transporte (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="5c0fb-245">**Código do cliente que especifica o método de transporte (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="5c0fb-246">Como alternativa, você pode especificar vários métodos de transporte na ordem em que você deseja que o SignalR para testá-los:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="5c0fb-247">**Código do cliente que especifica um esquema de fallback de transporte personalizado (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="5c0fb-248">**Código do cliente que especifica um esquema de fallback de transporte personalizado (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="5c0fb-249">Você pode usar os seguintes valores para especificar o método de transporte:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="5c0fb-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="5c0fb-250">"webSockets"</span></span>
- <span data-ttu-id="5c0fb-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="5c0fb-251">"foreverFrame"</span></span>
- <span data-ttu-id="5c0fb-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="5c0fb-252">"serverSentEvents"</span></span>
- <span data-ttu-id="5c0fb-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="5c0fb-253">"longPolling"</span></span>

<span data-ttu-id="5c0fb-254">Os exemplos a seguir mostram como descobrir qual método de transporte está sendo usado por uma conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="5c0fb-255">**Código do cliente que exibe o método de transporte usado por uma conexão (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="5c0fb-256">**Código do cliente que exibe o método de transporte usado por uma conexão (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="5c0fb-257">Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia de API de Hubs do ASP.NET SignalR - Server - como obter informações sobre o cliente da propriedade de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="5c0fb-258">Para obter mais informações sobre os transportes e restaurações, consulte [Introdução ao SignalR - transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="5c0fb-259">Como obter um proxy para uma classe de Hub</span><span class="sxs-lookup"><span data-stu-id="5c0fb-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="5c0fb-260">Cada objeto de conexão que você criar encapsula informações sobre uma conexão a um serviço de SignalR que contém uma ou mais classes de Hub.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="5c0fb-261">Para se comunicar com uma classe de Hub, você usa um objeto de proxy que você mesmo cria (se você não estiver usando o proxy gerado) ou que é gerado para você.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="5c0fb-262">No cliente, o nome do proxy é uma versão concatenados de nome de classe do Hub.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="5c0fb-263">SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="5c0fb-264">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="5c0fb-265">**Obter uma referência para o proxy de cliente gerada para o Hub**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="5c0fb-266">**Criar proxy de cliente para a classe de Hub (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="5c0fb-267">Se você decora sua classe de Hub com um `HubName` de atributo, use o nome exato sem alterar o caso.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="5c0fb-268">**Classe de Hub no servidor com o atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="5c0fb-269">**Obter uma referência para o proxy de cliente gerada para o Hub**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="5c0fb-270">**Criar proxy de cliente para a classe de Hub (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="5c0fb-271">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="5c0fb-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="5c0fb-272">Para definir um método que o servidor pode chamar de um Hub, adicione um manipulador de eventos para o proxy do Hub usando o `client` propriedade do proxy gerado ou chamada de `on` método se você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="5c0fb-273">Os parâmetros podem ser objetos complexos.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="5c0fb-274">Adicione o manipulador de eventos antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="5c0fb-275">(Se você quiser adicionar manipuladores de eventos após a chamada a `start` método, consulte a observação na [como estabelecer uma conexão](#establishconnection) anteriormente neste documento e usar a sintaxe mostrada para definir um método sem usar o proxy gerado.)</span><span class="sxs-lookup"><span data-stu-id="5c0fb-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="5c0fb-276">Correspondência de nome do método diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="5c0fb-277">Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor executará `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, ou `addcontosochatmessagetopage` no cliente.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="5c0fb-278">**Definir o método no cliente (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="5c0fb-279">**Maneira alternativa para definir o método no cliente (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="5c0fb-280">**Definir o método no cliente (sem o proxy gerado, ou ao adicionar depois de chamar o método start)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="5c0fb-281">**Código de servidor que chama o método de cliente**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="5c0fb-282">Os exemplos a seguir incluem um objeto complexo como um parâmetro de método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="5c0fb-283">**Definir o método no cliente que usa um objeto complexo (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="5c0fb-284">**Definir o método no cliente que usa um objeto complexo (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="5c0fb-285">**Código do servidor que define o objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="5c0fb-286">**Código de servidor que chama o método de cliente usando um objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="5c0fb-287">Como chamar métodos do servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-287">How to call server methods from the client</span></span>

<span data-ttu-id="5c0fb-288">Para chamar um método de servidor do cliente, use o `server` propriedade do proxy gerado ou o `invoke` método no proxy do Hub, se você não estiver usando o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="5c0fb-289">O valor de retorno ou os parâmetros podem ser objetos complexos.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="5c0fb-290">Passar em uma versão de caso de ter o nome do método no Hub.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="5c0fb-291">SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="5c0fb-292">Os exemplos a seguir mostram como chamar um método de servidor que não tem um valor de retorno e como chamar um método de servidor que tem um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="5c0fb-293">**Método de servidor sem o atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="5c0fb-294">**Código do servidor que define o objeto complexo passado em um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="5c0fb-295">**Código do cliente que chama o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="5c0fb-296">**Código do cliente que chama o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="5c0fb-297">Se você decorados com o método de Hub um `HubMethodName` de atributo, use esse nome sem alterar o caso.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="5c0fb-298">**Método de servidor** com um atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="5c0fb-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="5c0fb-299">**Código do cliente que chama o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="5c0fb-300">**Código do cliente que chama o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="5c0fb-301">Os exemplos acima mostram como chamar um método de servidor que não tem nenhum valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="5c0fb-302">Os exemplos a seguir mostram como chamar um método de servidor que tem um valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="5c0fb-303">**Código do servidor para um método que tem um valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="5c0fb-304">**A classe de estoque para o** valor de retorno</span><span class="sxs-lookup"><span data-stu-id="5c0fb-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="5c0fb-305">**Código do cliente que chama o método de servidor (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="5c0fb-306">**Código do cliente que chama o método de servidor (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="5c0fb-307">Como manipular eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="5c0fb-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="5c0fb-308">O SignalR fornece a seguinte conexão eventos de tempo de vida que você pode manipular:</span><span class="sxs-lookup"><span data-stu-id="5c0fb-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="5c0fb-309">`starting`: Gerado antes de todos os dados são enviados pela conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="5c0fb-310">`received`: Gerado quando nenhum dado é recebido na conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="5c0fb-311">Fornece os dados recebidos.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-311">Provides the received data.</span></span>
- <span data-ttu-id="5c0fb-312">`connectionSlow`: Gerado quando o cliente detecta uma conexão lenta ou remoção com frequência.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="5c0fb-313">`reconnecting`: Gerado quando o transporte subjacente inicia a reconexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="5c0fb-314">`reconnected`: Gerado quando o transporte subjacente foi reconectada.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="5c0fb-315">`stateChanged`: Gerado quando o estado da conexão é alterado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="5c0fb-316">Fornece o estado antigo e o novo estado (conectando, conectado, Reconnecting ou Disconnected).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="5c0fb-317">`disconnected`: Gerado quando a conexão foi desconectado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="5c0fb-318">Por exemplo, se você quiser exibir mensagens de aviso quando há problemas de conexão que podem causar atrasos notáveis, tratar o `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="5c0fb-319">**Manipular o evento connectionSlow (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="5c0fb-320">**Manipular o evento connectionSlow (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="5c0fb-321">Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="5c0fb-322">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="5c0fb-322">How to handle errors</span></span>

<span data-ttu-id="5c0fb-323">O cliente SignalR JavaScript fornece um `error` que você pode adicionar um manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="5c0fb-324">Você também pode usar o método de falha para adicionar um manipulador de erros resultantes de uma invocação de método server.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="5c0fb-325">Se você não habilitar explicitamente as mensagens de erro detalhadas no servidor, o objeto de exceção SignalR retorna após um erro contém algumas informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="5c0fb-326">Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro contém "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensagens de erro detalhadas para clientes de produção não é recomendado por motivos de segurança, mas se você deseja habilitar as mensagens de erro detalhadas para solução de problemas, use o seguinte código no servidor.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="5c0fb-327">O exemplo a seguir mostra como adicionar um manipulador para o evento de erro.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="5c0fb-328">**Adicionar um manipulador de erros (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="5c0fb-329">**Adicionar um manipulador de erro (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="5c0fb-330">O exemplo a seguir mostra como tratar o erro de uma invocação de método.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="5c0fb-331">**Lidar com um erro de uma invocação de método (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="5c0fb-332">**Lidar com um erro de uma invocação de método (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="5c0fb-333">Se uma invocação de método falhar, o `error` também é gerado, portanto o código do `error` manipulador de método e no `.fail` retorno de chamada do método seria executado.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="5c0fb-334">Como habilitar o registro de cliente</span><span class="sxs-lookup"><span data-stu-id="5c0fb-334">How to enable client-side logging</span></span>

<span data-ttu-id="5c0fb-335">Para habilitar o registro de cliente em uma conexão, defina o `logging` propriedade no objeto de conexão antes de chamar o `start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="5c0fb-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="5c0fb-336">**Habilitar registro em log (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="5c0fb-337">**Habilitar o log (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="5c0fb-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="5c0fb-338">Para ver os logs, abra Ferramentas de desenvolvedor do seu navegador e vá para a guia Console. Para obter um tutorial que mostra a tela e instruções passo a passo capturas que mostram como fazer isso, consulte [de difusão de servidor com ASP.NET Signalr - Enable Logging](index.md).</span><span class="sxs-lookup"><span data-stu-id="5c0fb-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>