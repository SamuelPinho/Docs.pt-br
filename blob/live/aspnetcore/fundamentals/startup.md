---
title: "Inicialização do aplicativo no núcleo do ASP.NET"
author: ardalis
description: "Descobrir como a classe de inicialização no ASP.NET Core configura serviços e pipeline de solicitação do aplicativo."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="64e56-103">Inicialização do aplicativo no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="64e56-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="64e56-104">Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="64e56-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="64e56-105">O `Startup` classe configura os serviços e pipeline de solicitação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="64e56-106">A classe de inicialização</span><span class="sxs-lookup"><span data-stu-id="64e56-106">The Startup class</span></span>

<span data-ttu-id="64e56-107">Uso de aplicativos do ASP.NET Core um `Startup` classe, que é chamado `Startup` por convenção.</span><span class="sxs-lookup"><span data-stu-id="64e56-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="64e56-108">O `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="64e56-108">The `Startup` class:</span></span>

* <span data-ttu-id="64e56-109">Opcionalmente, pode incluir um [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método para configurar os serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="64e56-110">Deve incluir um [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método para criar o pipeline de processamento de solicitação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="64e56-111">`ConfigureServices`e `Configure` são chamados pelo tempo de execução quando o aplicativo for iniciado:</span><span class="sxs-lookup"><span data-stu-id="64e56-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="64e56-112">Especifique o `Startup` classe com o [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método:</span><span class="sxs-lookup"><span data-stu-id="64e56-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="64e56-113">O `Startup` construtor de classe aceita dependências definidas pelo host.</span><span class="sxs-lookup"><span data-stu-id="64e56-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="64e56-114">Um uso comum de [injeção de dependência](xref:fundamentals/dependency-injection) para o `Startup` classe é injetar [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar serviços pelo ambiente:</span><span class="sxs-lookup"><span data-stu-id="64e56-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="64e56-115">Uma alternativa para injetar `IHostingStartup` é usar uma abordagem baseada em convenções.</span><span class="sxs-lookup"><span data-stu-id="64e56-115">An alternative to injecting `IHostingStartup` is to use a conventions-based approach.</span></span> <span data-ttu-id="64e56-116">O aplicativo pode definir separada `Startup` classes para ambientes diferentes (por exemplo, `StartupDevelopment`), e a classe de inicialização adequado é selecionada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="64e56-116">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="64e56-117">A classe sufixo cujo nome corresponde ao ambiente atual é priorizada.</span><span class="sxs-lookup"><span data-stu-id="64e56-117">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="64e56-118">Se o aplicativo é executado no ambiente de desenvolvimento e inclui tanto um `Startup` classe e um `StartupDevelopment` classe, o `StartupDevelopment` classe é usada.</span><span class="sxs-lookup"><span data-stu-id="64e56-118">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="64e56-119">Para obter mais informações, consulte [trabalhando com vários ambientes](xref:fundamentals/environments#startup-conventions).</span><span class="sxs-lookup"><span data-stu-id="64e56-119">For more information see [Working with multiple environments](xref:fundamentals/environments#startup-conventions).</span></span>

<span data-ttu-id="64e56-120">Para saber mais sobre `WebHostBuilder`, consulte o [hospedagem](xref:fundamentals/hosting) tópico.</span><span class="sxs-lookup"><span data-stu-id="64e56-120">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/hosting) topic.</span></span> <span data-ttu-id="64e56-121">Para obter informações sobre como manipular erros durante a inicialização, consulte [tratamento de exceção de inicialização](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="64e56-121">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="64e56-122">O método ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="64e56-122">The ConfigureServices method</span></span>

<span data-ttu-id="64e56-123">O [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método é:</span><span class="sxs-lookup"><span data-stu-id="64e56-123">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="64e56-124">Opcional.</span><span class="sxs-lookup"><span data-stu-id="64e56-124">Optional.</span></span>
* <span data-ttu-id="64e56-125">Chamado pelo host da web antes do `Configure` método para configurar os serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-125">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="64e56-126">Onde [opções de configuração](xref:fundamentals/configuration/index) são definidos por convenção.</span><span class="sxs-lookup"><span data-stu-id="64e56-126">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="64e56-127">Adicionar serviços ao contêiner de serviço torna-os disponíveis dentro do aplicativo e no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="64e56-127">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="64e56-128">Os serviços são resolvidos por meio de [injeção de dependência](xref:fundamentals/dependency-injection) ou [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span><span class="sxs-lookup"><span data-stu-id="64e56-128">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="64e56-129">O host da web pode configurar alguns serviços antes `Startup` métodos são chamados.</span><span class="sxs-lookup"><span data-stu-id="64e56-129">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="64e56-130">Detalhes estão disponíveis no [hospedagem](xref:fundamentals/hosting) tópico.</span><span class="sxs-lookup"><span data-stu-id="64e56-130">Details are available in the [Hosting](xref:fundamentals/hosting) topic.</span></span> 

<span data-ttu-id="64e56-131">Para recursos que exigem configuração significativa, há `Add[Service]` métodos de extensão em [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span><span class="sxs-lookup"><span data-stu-id="64e56-131">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="64e56-132">Um aplicativo web típico registra os serviços para Entity Framework, identidade e MVC:</span><span class="sxs-lookup"><span data-stu-id="64e56-132">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a><span data-ttu-id="64e56-133">Serviços disponíveis na inicialização</span><span class="sxs-lookup"><span data-stu-id="64e56-133">Services available in Startup</span></span>

<span data-ttu-id="64e56-134">O host da web fornece alguns serviços que estão disponíveis para o `Startup` construtor de classe.</span><span class="sxs-lookup"><span data-stu-id="64e56-134">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="64e56-135">O aplicativo adiciona serviços adicionais por meio de `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="64e56-135">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="64e56-136">O host e o aplicativo de serviços, em seguida, estão disponíveis em `Configure` e em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-136">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="64e56-137">O método de configurar</span><span class="sxs-lookup"><span data-stu-id="64e56-137">The Configure method</span></span>

<span data-ttu-id="64e56-138">O [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método é usado para especificar como o aplicativo responde às solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="64e56-138">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="64e56-139">O pipeline de solicitação está configurado com a adição de [middleware](xref:fundamentals/middleware) componentes para uma [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instância.</span><span class="sxs-lookup"><span data-stu-id="64e56-139">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="64e56-140">`IApplicationBuilder`está disponível para o `Configure` método, mas ele não está registrado no contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="64e56-140">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="64e56-141">Hospedagem cria uma `IApplicationBuilder` e passá-lo diretamente para `Configure` ([fonte de referência](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span><span class="sxs-lookup"><span data-stu-id="64e56-141">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="64e56-142">O [modelos ASP.NET Core](/dotnet/core/tools/dotnet-new) configurar o pipeline com suporte para uma página de exceção de desenvolvedor, [BrowserLink](http://vswebessentials.com/features/browserlink), ASP.NET MVC, arquivos estáticos e páginas de erro:</span><span class="sxs-lookup"><span data-stu-id="64e56-142">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="64e56-143">Cada `Use` método de extensão adiciona um componente de middleware no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="64e56-143">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="64e56-144">Por exemplo, o `UseMvc` método de extensão adiciona o [roteamento middleware](xref:fundamentals/routing) para o pipeline de solicitação e configura [MVC](xref:mvc/overview) como o manipulador padrão.</span><span class="sxs-lookup"><span data-stu-id="64e56-144">For instance, the `UseMvc` extension method adds the [routing middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="64e56-145">Serviços adicionais, como `IHostingEnvironment` e `ILoggerFactory`, também pode ser especificado na assinatura do método.</span><span class="sxs-lookup"><span data-stu-id="64e56-145">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="64e56-146">Quando especificado, os serviços adicionais podem ser adicionados se elas estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="64e56-146">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="64e56-147">Para obter mais informações sobre como usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="64e56-147">For more information on how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="64e56-148">Métodos de conveniência</span><span class="sxs-lookup"><span data-stu-id="64e56-148">Convenience methods</span></span>

<span data-ttu-id="64e56-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) e [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) métodos de conveniência podem ser usados em vez de especificar um `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="64e56-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="64e56-150">Diversas chamadas para `ConfigureServices` acrescentar um ao outro.</span><span class="sxs-lookup"><span data-stu-id="64e56-150">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="64e56-151">Diversas chamadas para `Configure` usar a última chamada de método.</span><span class="sxs-lookup"><span data-stu-id="64e56-151">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a><span data-ttu-id="64e56-152">Filtros de inicialização</span><span class="sxs-lookup"><span data-stu-id="64e56-152">Startup filters</span></span>

<span data-ttu-id="64e56-153">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar o middleware no início ou no final de um aplicativo [configurar](#the-configure-method) pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="64e56-153">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="64e56-154">`IStartupFilter`é útil para garantir que um middleware seja executado antes ou depois de adicionadas pelas bibliotecas no início ou no final do pipeline de processamento de solicitação do aplicativo de middleware.</span><span class="sxs-lookup"><span data-stu-id="64e56-154">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="64e56-155">`IStartupFilter`implementa um método único, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recebe e retorna um `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="64e56-155">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="64e56-156">Um [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define uma classe para configurar o pipeline de solicitação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-156">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="64e56-157">Para obter mais informações, consulte [criando um pipeline de middleware com IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="64e56-157">For more information, see [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="64e56-158">Cada `IStartupFilter` implementa uma ou mais middlewares no pipeline de solicitação.</span><span class="sxs-lookup"><span data-stu-id="64e56-158">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="64e56-159">Os filtros são invocados na ordem em que eles foram adicionados ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="64e56-159">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="64e56-160">Filtros podem adicionar middleware antes ou depois de passar o controle para o próximo filtro, assim eles acrescentam ao início ou no final do pipeline de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64e56-160">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="64e56-161">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) demonstra como registrar um middleware com `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="64e56-161">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="64e56-162">O aplicativo de exemplo inclui um middleware que define um valor de opção de um parâmetro de cadeia de caracteres de consulta:</span><span class="sxs-lookup"><span data-stu-id="64e56-162">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="64e56-163">O `RequestSetOptionsMiddleware` é configurado no `RequestSetOptionsStartupFilter` classe:</span><span class="sxs-lookup"><span data-stu-id="64e56-163">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="64e56-164">O `IStartupFilter` está registrado no contêiner de serviço no `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64e56-164">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="64e56-165">Quando um parâmetro de cadeia de caracteres de consulta para `option` for fornecido, o middleware processa a atribuição de valor antes do middleware MVC processa a resposta:</span><span class="sxs-lookup"><span data-stu-id="64e56-165">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![Janela do navegador mostrando a página renderizada.](startup/_static/index.png)

<span data-ttu-id="64e56-168">Ordem de execução de middleware é definida pela ordem de `IStartupFilter` registros:</span><span class="sxs-lookup"><span data-stu-id="64e56-168">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="64e56-169">Vários `IStartupFilter` implementações podem interagir com os mesmos objetos.</span><span class="sxs-lookup"><span data-stu-id="64e56-169">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="64e56-170">Se a ordem é importante, ordene suas `IStartupFilter` registros para corresponder à ordem em que devem ser executados por seus middlewares de serviço.</span><span class="sxs-lookup"><span data-stu-id="64e56-170">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="64e56-171">Bibliotecas podem adicionar middleware com um ou mais `IStartupFilter` implementações executam antes ou depois de outro middleware aplicativo registrado com `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="64e56-171">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="64e56-172">Para invocar um `IStartupFilter` middleware antes de um middleware adicionado por uma biblioteca `IStartupFilter`, posicione o registro do serviço antes da biblioteca é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="64e56-172">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="64e56-173">Para invocar posteriormente, posicione o registro do serviço depois que a biblioteca é adicionado.</span><span class="sxs-lookup"><span data-stu-id="64e56-173">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64e56-174">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="64e56-174">Additional resources</span></span>

* [<span data-ttu-id="64e56-175">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="64e56-175">Hosting</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="64e56-176">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="64e56-176">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="64e56-177">Middleware</span><span class="sxs-lookup"><span data-stu-id="64e56-177">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="64e56-178">Registro em log</span><span class="sxs-lookup"><span data-stu-id="64e56-178">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="64e56-179">Configuração</span><span class="sxs-lookup"><span data-stu-id="64e56-179">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="64e56-180">Classe StartupLoader: método FindStartupType (fonte de referência)</span><span class="sxs-lookup"><span data-stu-id="64e56-180">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)