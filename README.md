# Asp.Net-Core REST APIs

This is a perfect sample of how to implement REST APIs in asp.net core.

Here I have learned how to get resource by REST API. I have also learned how to apply paging, order by, filter and shaping data as requested.
The "X-Pagination" header is used for representing pagination metadata. 

    var paginationMetadata = new
    {
        totalCount = authorsFromRepo.TotalCount,
        pageSize = authorsFromRepo.PageSize,
        currentPage = authorsFromRepo.CurrentPage,
        totalPages = authorsFromRepo.TotalPages,
    };

    Response.Headers.Add("X-Pagination", Newtonsoft.Json.JsonConvert.SerializeObject(paginationMetadata));

I have learned when to use HTTP POST, PUT, HEAD, OPTIONS and PATCH requests. I have learned by PATH request updating object can be very faster and safely. 
Perfect model mapping and validations are done by asp.net core provided return types e.g. BadRequest, NotFound etc

    if (!_typeHelperService.TypeHasProperties<AuthorDto>
        (fields))
    {
        return **BadRequest**();
    }

    var authorFromRepo = _libraryRepository.GetAuthor(id);

    if (authorFromRepo == null)
    {
        return **NotFound**();
    }

Example of handling OPTIONS request, that returns the allowed options by the API:

    [HttpOptions]
    public IActionResult GetAuthorsOptions()
    {
        Response.Headers.Add("Allow", "GET,OPTIONS,POST,PUT,PATCH,HEAD");
        return Ok();
    }


In asp.net core RateLimit and Throatling can be done by the following configurations in the middleware:
Here the limit for each client from a IP address is set to maximum 10 per each 5 minutes.


    services.AddMemoryCache();

    services.Configure<IpRateLimitOptions>((options) =>
    {
        options.GeneralRules = new System.Collections.Generic.List<RateLimitRule>()
        {
            new RateLimitRule()
            {
                Endpoint = "*",
                Limit = 10,
                Period = "5m"
            } 
        };
    });

    services.AddSingleton<IRateLimitCounterStore, MemoryCacheRateLimitCounterStore>();
    services.AddSingleton<IIpPolicyStore, MemoryCacheIpPolicyStore>();

This sample shows how to use dummy data for entity framework.

The BooksController is a sample that shows how to add, update and delete single child and collection of child objects.


In ASP.NET core any kind of internal server error can be handled globally with following configurations:

        app.UseExceptionHandler(appBuilder =>
        {
            appBuilder.Run(async context =>
            {
                var exceptionHandlerFeature = context.Features.Get<IExceptionHandlerFeature>();
                if (exceptionHandlerFeature != null)
                {
                    var logger = loggerFactory.CreateLogger("Global exception logger");
                    logger.LogError(500,
                        exceptionHandlerFeature.Error,
                        exceptionHandlerFeature.Error.Message);
                }

                context.Response.StatusCode = 500;
                await context.Response.WriteAsync("An unexpected fault happened. Try again later.");

            });                      
        });
