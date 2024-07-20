# ASP.NET Web API Documentation

## Introduction
1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Installation](#installation)

## Fundamentals
4. [Routing](#routing)
5. [Controllers](#controllers)
6. [Actions](#actions)
7. [Models](#models)
8. [Dependency Injection](#dependency-injection)
9. [Configuration](#configuration)

## Request and Response
10. [HTTP Methods](#http-methods)
11. [Request Binding](#request-binding)
12. [Response Formats](#response-formats)
13. [Model Validation](#model-validation)

## Data Access
14. [Entity Framework Core](#entity-framework-core)
15. [Dapper](#dapper)
16. [Connecting to Databases](#connecting-to-databases)
17. [Repository Pattern](#repository-pattern)

## Security
18. [Authentication](#authentication)
19. [Authorization](#authorization)
20. [JWT Tokens](#jwt-tokens)
21. [CORS](#cors)

## Middleware
22. [Custom Middleware](#custom-middleware)
23. [Exception Handling](#exception-handling)
24. [Logging](#logging)

## Advanced Topics
25. [Filters](#filters)
26. [Action Results](#action-results)
27. [Media Formatters](#media-formatters)
28. [Content Negotiation](#content-negotiation)
29. [Rate Limiting](#rate-limiting)

## Testing
30. [Unit Testing](#unit-testing)
31. [Integration Testing](#integration-testing)
32. [Mocking Dependencies](#mocking-dependencies)

## Performance
33. [Caching](#caching)
34. [Performance Tuning](#performance-tuning)
35. [Async Programming](#async-programming)

## Deployment
36. [Hosting](#hosting)
37. [Deployment Strategies](#deployment-strategies)
38. [CI/CD Pipelines](#ci-cd-pipelines)

## Tools and Extensions
39. [Swagger/OpenAPI](#swagger-openapi)
40. [Postman](#postman)
41. [API Versioning](#api-versioning)

## Real-World Examples
42. [Building a Simple API](#building-a-simple-api)
43. [CRUD Operations](#crud-operations)
44. [File Upload/Download](#file-upload-download)
45. [Background Tasks](#background-tasks)




## Routing

Routing in ASP.NET Web API is responsible for mapping HTTP requests to the appropriate action methods in controllers. It defines how URLs are structured and determines which controller and action method handles a given request.

### Types of Routing

1. **Attribute Routing**
   - Attribute routing uses attributes to define routes directly on controller actions.
   - Provides more control over the URIs in your web API.
   - Example:
     ```csharp
     [Route("api/products")]
     public class ProductsController : ApiController
     {
         [HttpGet]
         [Route("{id:int}")]
         public IHttpActionResult GetProduct(int id)
         {
             // Action method code here
         }
     }
     ```

2. **Convention-based Routing**
   - Convention-based routing is defined in the `Startup.cs` or `WebApiConfig.cs` file.
   - Routes are defined using a pattern that matches request URLs to controllers and actions.
   - Example:
     ```csharp
     public static class WebApiConfig
     {
         public static void Register(HttpConfiguration config)
         {
             config.MapHttpAttributeRoutes();
             
             config.Routes.MapHttpRoute(
                 name: "DefaultApi",
                 routeTemplate: "api/{controller}/{id}",
                 defaults: new { id = RouteParameter.Optional }
             );
         }
     }
     ```

### Defining Routes

- **Route Templates**: Define the pattern for matching request URIs.
  - Example: `"api/{controller}/{id}"` where `id` is optional.
- **Route Constraints**: Enforce rules on route parameters (e.g., type constraints).
  - Example: `[Route("{id:int}")]` ensures `id` is an integer.

### Route Parameters

- **Optional Parameters**: Use `RouteParameter.Optional` to make parameters optional.
- **Default Values**: Provide default values for parameters.
  - Example: `{ controller = "Home", id = RouteParameter.Optional }`

### Advanced Routing

- **Custom Route Handlers**: Implement custom route handlers for more complex scenarios.
- **Route Prefixes**: Use `[RoutePrefix]` attribute to specify a common prefix for all routes in a controller.
  - Example:
    ```csharp
    [RoutePrefix("api/products")]
    public class ProductsController : ApiController
    {
        [HttpGet]
        [Route("{id:int}")]
        public IHttpActionResult GetProduct(int id)
        {
            // Action method code here
        }
    }
    ```

### Route Debugging

- **Route Debugging Tool**: Use tools like RouteDebugger to troubleshoot routing issues.
  - Install via NuGet: `Install-Package RouteDebugger`
  - Add to your route configuration to see detailed route information.

### Common Scenarios

1. **Handling 404 Errors**:
   - Ensure routes are correctly defined to prevent 404 errors.
   - Example of a catch-all route to handle 404s:
     ```csharp
     config.Routes.MapHttpRoute(
         name: "NotFound",
         routeTemplate: "{*url}",
         defaults: new { controller = "Error", action = "Handle404" }
     );
     ```

2. **Versioning**:
   - Use route prefixes or query parameters to version your API.
   - Example with route prefix:
     ```csharp
     [RoutePrefix("api/v1/products")]
     public class ProductsV1Controller : ApiController
     {
         // Actions for version 1
     }
     
     [RoutePrefix("api/v2/products")]
     public class ProductsV2Controller : ApiController
     {
         // Actions for version 2
     }
     ```

### Best Practices

- Keep your route templates simple and consistent.
- Use attribute routing for more control and clarity.
- Regularly review and test your routes to ensure they behave as expected.

### Additional Resources
- [ASP.NET Web API Routing - Official Documentation](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api)
- [Attribute Routing in ASP.NET Web API 2](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)

By understanding and effectively using routing, you can create a more organized and efficient ASP.NET Web API application.
