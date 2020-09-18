---
ms.openlocfilehash: 26e521a86b504824f035ad5d40e4a04d2ea26abc
ms.sourcegitcommit: 6d4ee46871deb9ea1e45bb5f3784474e240bbc26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90022957"
---
### <a name="middleware-exception-handler-middleware-throws-original-exception-if-handler-not-found"></a><span data-ttu-id="f7d21-101">中间件：如果找不到处理程序，则异常处理程序中间件会引发原始异常</span><span class="sxs-lookup"><span data-stu-id="f7d21-101">Middleware: Exception Handler Middleware throws original exception if handler not found</span></span>

<span data-ttu-id="f7d21-102">在 ASP.NET Core 5.0 及更低版本中，[异常处理程序中间件](xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A)在发生异常时执行配置的异常处理程序。</span><span class="sxs-lookup"><span data-stu-id="f7d21-102">Before ASP.NET Core 5.0, the [Exception Handler Middleware](xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler%2A) executes the configured exception handler when an exception has occurred.</span></span> <span data-ttu-id="f7d21-103">如果找不到通过 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerOptions.ExceptionHandlingPath> 配置的异常处理程序，则会生成 HTTP 404 响应。</span><span class="sxs-lookup"><span data-stu-id="f7d21-103">If the exception handler, configured via <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerOptions.ExceptionHandlingPath>, can't be found, an HTTP 404 response is produced.</span></span> <span data-ttu-id="f7d21-104">该响应具有误导性，因为它：</span><span class="sxs-lookup"><span data-stu-id="f7d21-104">The response is misleading in that it:</span></span>

* <span data-ttu-id="f7d21-105">似乎是一个用户错误。</span><span class="sxs-lookup"><span data-stu-id="f7d21-105">Seems to be a user error.</span></span>
* <span data-ttu-id="f7d21-106">掩盖了服务器上发生异常的事实。</span><span class="sxs-lookup"><span data-stu-id="f7d21-106">Obscures the fact that an exception occurred on the server.</span></span>

<span data-ttu-id="f7d21-107">为了解决 ASP.NET Core 5.0 中的误导性错误，`ExceptionHandlerMiddleware` 会在找不到异常处理程序的情况下引发原始异常。</span><span class="sxs-lookup"><span data-stu-id="f7d21-107">To address the misleading error in ASP.NET Core 5.0, the `ExceptionHandlerMiddleware` throws the original exception if the exception handler can't be found.</span></span> <span data-ttu-id="f7d21-108">因此，服务器会生成 HTTP 500 响应。</span><span class="sxs-lookup"><span data-stu-id="f7d21-108">As a result, an HTTP 500 response is produced by the server.</span></span> <span data-ttu-id="f7d21-109">调试发生的错误时，可以更容易地在服务器日志中检查响应。</span><span class="sxs-lookup"><span data-stu-id="f7d21-109">The response will be easier to examine in the server logs when debugging the error that occurred.</span></span>

<span data-ttu-id="f7d21-110">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25288](https://github.com/dotnet/aspnetcore/issues/25288)。</span><span class="sxs-lookup"><span data-stu-id="f7d21-110">For discussion, see GitHub issue [dotnet/aspnetcore#25288](https://github.com/dotnet/aspnetcore/issues/25288).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="f7d21-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="f7d21-111">Version introduced</span></span>

<span data-ttu-id="f7d21-112">5.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="f7d21-112">5.0 RC 1</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="f7d21-113">旧行为</span><span class="sxs-lookup"><span data-stu-id="f7d21-113">Old behavior</span></span>

<span data-ttu-id="f7d21-114">如果找不到配置的异常处理程序，异常处理程序中间件会生成 HTTP 404 响应。</span><span class="sxs-lookup"><span data-stu-id="f7d21-114">The Exception Handler Middleware produces an HTTP 404 response if the configured exception handler can't be found.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="f7d21-115">新行为</span><span class="sxs-lookup"><span data-stu-id="f7d21-115">New behavior</span></span>

<span data-ttu-id="f7d21-116">如果找不到配置的异常处理程序，异常处理程序中间件会引发原始异常。</span><span class="sxs-lookup"><span data-stu-id="f7d21-116">The Exception Handler Middleware throws the original exception if the configured exception handler can't be found.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="f7d21-117">更改原因</span><span class="sxs-lookup"><span data-stu-id="f7d21-117">Reason for change</span></span>

<span data-ttu-id="f7d21-118">HTTP 404 错误并不意味着服务器发生了异常。</span><span class="sxs-lookup"><span data-stu-id="f7d21-118">The HTTP 404 error doesn't make it obvious that an exception occurred on the server.</span></span> <span data-ttu-id="f7d21-119">此更改会生成 HTTP 500 错误，很明显：</span><span class="sxs-lookup"><span data-stu-id="f7d21-119">This change produces an HTTP 500 error to make it obvious that:</span></span>

* <span data-ttu-id="f7d21-120">此问题不是由用户错误引起的。</span><span class="sxs-lookup"><span data-stu-id="f7d21-120">The problem isn't caused by a user error.</span></span>
* <span data-ttu-id="f7d21-121">服务器上发生了异常。</span><span class="sxs-lookup"><span data-stu-id="f7d21-121">An exception was encountered on the server.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="f7d21-122">建议操作</span><span class="sxs-lookup"><span data-stu-id="f7d21-122">Recommended action</span></span>

<span data-ttu-id="f7d21-123">无 API 更改。</span><span class="sxs-lookup"><span data-stu-id="f7d21-123">There are no API changes.</span></span> <span data-ttu-id="f7d21-124">所有现有的应用将继续编译并运行。</span><span class="sxs-lookup"><span data-stu-id="f7d21-124">All existing apps will continue to compile and run.</span></span> <span data-ttu-id="f7d21-125">引发的异常由服务器处理。</span><span class="sxs-lookup"><span data-stu-id="f7d21-125">The exception thrown is handled by the server.</span></span> <span data-ttu-id="f7d21-126">例如，[Kestrel](/aspnet/core/fundamentals/servers/kestrel) 或 [HTTP.sys](/aspnet/core/fundamentals/servers/httpsys) 会将异常转换为 HTTP 500 错误响应。</span><span class="sxs-lookup"><span data-stu-id="f7d21-126">For example, the exception is converted to an HTTP 500 error response by [Kestrel](/aspnet/core/fundamentals/servers/kestrel) or [HTTP.sys](/aspnet/core/fundamentals/servers/httpsys).</span></span>

#### <a name="category"></a><span data-ttu-id="f7d21-127">类别</span><span class="sxs-lookup"><span data-stu-id="f7d21-127">Category</span></span>

<span data-ttu-id="f7d21-128">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7d21-128">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="f7d21-129">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="f7d21-129">Affected APIs</span></span>

<span data-ttu-id="f7d21-130">无</span><span class="sxs-lookup"><span data-stu-id="f7d21-130">None</span></span>

<!--

#### Affected APIs

Not detectable via API analysis

-->