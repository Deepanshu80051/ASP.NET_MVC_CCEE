# ASP.NET_MVC_CCEE

## MVC
1. User Request → Router → Controller → Model → Controller → View → Response
2. Controller returns ActionResult (ViewResult, JsonResult, etc.)
3. EXAM TIP: "Controller acts as coordinator" - correct statement

## Folder Structure & Configuration
1. /App_Start → Configuration files (startup settings)
2. /App_Data → Database files (local database)
3. Configuration Files:
```
Web.config → Application settings, connection strings (main config file)
RouteConfig.cs → URL routing rules (URL kaise work kare)
BundleConfig.cs → CSS/JS bundling (multiple files ko combine)
FilterConfig.cs → Global filters (global level pe filters)
```
## Controllers & Actions
```
ActionResult Types:

ViewResult → Returns view (return View()) - HTML page
JsonResult → Returns JSON (return Json(data)) - JSON data
RedirectResult → Redirects to URL (return Redirect("/path"))
RedirectToRouteResult → Redirects to action (return RedirectToAction("Index"))
ContentResult → Returns string (return Content("Hello")) - plain text
FileResult → Returns file (file download ke liye)
EmptyResult → Returns nothing (kuch return nahi)
PartialViewResult → Returns partial view
Controller must inherit from Controller class (mandatory inheritance)
Action methods must be public (private nahi ho sakte)
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View(); // View return karta hai
    }
}
ActionResult is base class for all result types (sabka parent)
Cannot be static (static methods action nahi ban sakte)
```
## HTTP Verbs - HttpGet, HttpPost, NonAction
```
Attributes:
csharp[HttpGet] // Form dikhane ke liye
public ActionResult Create() 
{ 
    return View(); 
}

[HttpPost] // Form submit karne ke liye
public ActionResult Create(Student student) 
{ 
    // Save data
    return RedirectToAction("Index");
}

[NonAction] // Ye action nahi hai
public void HelperMethod() 
{ 
    // Helper code
}
Tricky MCQ Points:

[HttpGet] is default (likhna optional hai, by default GET hota hai)
[HttpPost] required for form submissions (form submit pe POST chahiye)
[NonAction] prevents method from being action (URL se call nahi ho sakta)
Same action name with different verbs = method overloading (same naam, alag HTTP verb)
GET for retrieving data (data lena)
POST for sending data (data bhejna)
[AcceptVerbs(HttpVerbs.Get | HttpVerbs.Post)] for multiple verbs
EXAM TIP: HttpPost actions usually return RedirectToAction (PRG pattern)
```
## Views & Models
view :-
```
@model Student  <!-- Strongly typed -->

<h1>@Model.Name</h1>  <!-- Model data access -->
<p>@Model.Age</p>
```
Tricky MCQ Points:
```
View files have .cshtml extension (Razor syntax)
@model (lowercase) defines type (compile time - ek baar)
@Model (uppercase) accesses data (runtime - bar bar)
Strongly typed views are type-safe (compile time errors milti hain)
Can pass model using View(model)
View without model is possible (dynamic views)
EXAM TIP: @model Student (singular - ek object), @model IEnumerable<Student> (multiple - list)
```

## Razor Views
```
@* Comment (Razor comment) *@
@{ 
    // C# code block
    var name = "John";
    int age = 25;
}

<p>Name: @name</p>  <!-- Inline expression -->
<p>Age: @age</p>

@if(age >= 18) 
{
    <p>Adult</p>  <!-- HTML mixing -->
}

@for(int i = 0; i < 5; i++)
{
    <p>Item @i</p>
}

@foreach(var item in Model)
{
    <li>@item.Name</li>
}
```
Tricky MCQ Points:
```
@ symbol starts Razor code (@naam se C# code shuru)
@{ } for multi-line C# code (multiple lines ke liye)
@*  *@ for comments (HTML comment nahi, Razor comment)
No need to close @ tags (automatically detect karta hai)
Can mix HTML and C# seamlessly (asaani se mix kar sakte)
@: for single line plain text
<text></text> for multi-line plain text
EXAM TIP: @Model (capital M) vs @model (small m) - case sensitive hai
```
## HTML Helpers
```
@Html.TextBox("Name")  // <input type="text" name="Name" />
@Html.TextBoxFor(m => m.Name)  // Strongly typed
@Html.Hidden("Id")  // Hidden field
@Html.HiddenFor(m => m.Id)
@Html.ActionLink("Home", "Index", "Home")  // Link
// <a href="/Home/Index">Home</a>
```
Form Helpers
```
@using(Html.BeginForm("Action", "Controller", FormMethod.Post))
{
    @Html.TextBoxFor(m => m.Name)
    <input type="submit" value="Save" />
}
```
## ViewBag, ViewData, TempData
ViewBag (Dynamic):
```
csharp// Controller
ViewBag.Message = "Hello";  // Dynamic property
ViewBag.Count = 10;

// View
<p>@ViewBag.Message</p>
<p>@ViewBag.Count</p>
```
ViewData (Dictionary):
```
csharp// Controller
ViewData["Message"] = "Hello";  // Key-value pair
ViewData["Count"] = 10;

// View
<p>@ViewData["Message"]</p>
<p>@ViewData["Count"]</p>

```
https://docs.google.com/document/d/1M_JP4-pk8UD8EXMI5zy6UhZENbBlaX8unikjxq3pubU/edit?tab=t.ebzbi1hqwp03
## Validation
1. @Html.ValidationSummary(true)  <!-- All errors ek saath -->
2. ModelState.IsValid checks server-side validation
3. Data annotations work on both sides (client + server dono pe)
4. ValidationMessageFor shows field-specific error (us field ka error)
5. EXAM TIP: Server validation always executes, client validation optional hai

## Strongly Typed Views
1. Strongly typed views have compile-time type checking (compile time pe error)
2. @model defines type (one time declaration)..
@Model accesses instance (runtime pe use)

## Scaffolding 
1. Auto-generates code (controller + views automatic bante)
2. Choose template (Empty, with views, API)
3. Scaffold Templates:
```
Empty - Blank controller
MVC Controller with read/write actions - Basic CRUD actions
MVC Controller with views, using Entity Framework - Complete CRUD with DB
```
4. Uses Entity Framework for database (DB operations ke liye)
5. Create has two actions - GET and POST (do methods)
6. Edit also has GET and POST (do methods)
7. Delete has GET (confirmation) and POST (actual delete)

Tricky MCQ Points:
```
Session server pe store hota, timeout hone tak (default 20 min)
Cookies client (browser) pe store hota, size limit 4KB
Session requires type casting (object return karta)
Cookies are strings (string format me)
TempData for one-time messages (success/error messages)
ViewBag/ViewData for current request only
Session.Abandon() to clear all session (sab session clear)
Cookie.Expires to set expiry time
Persistent cookies vs Session cookies (expiry wala vs browser close)
EXAM TIP: Session data lost on session timeout or browser close
```
## Partial View
```
// Method 1: Html.Partial (synchronous - wait karta)
@Html.Partial("_UserInfo", Model.User)

// Method 2: Html.RenderPartial (writes directly - fast)
@{ Html.RenderPartial("_UserInfo", Model.User); }
// Method 3: Html.Action (calls action - alag request)
@Html.Action("GetUserInfo", "User")

// Method 4: Html.RenderAction (action call - fast)
@{ Html.RenderAction("GetUserInfo", "User"); }

Naming convention: start with underscore _Name.cshtml

```
## ADO
1. SqlConnection conn = new SqlConnection(connStr)
2. SqlCommand cmd = new SqlCommand(query, conn);
3. SqlDataReader reader = cmd.ExecuteReader();
4.  cmd.ExecuteNonQuery();  // INSERT/UPDATE/DELETE ke liye
5.  ExecuteReader() for SELECT (data padhne ke liye)
6.  ExecuteScalar() for single value (ek value return - like COUNT
7. @Parameter syntax for parameters (SQL injection prevention)
## Router
1. routes.IgnoreRoute() to skip certain URLs (like .axd files)
2. {controller}/{action}/{id} is convention (standard pattern)
3. id is optional by default (UrlParameter.Optional)
## Layout
```
Tricky MCQ Points:

Layout is master page (common template for all pages)
@RenderBody() is mandatory in layout (child content yaha aayega)
@RenderSection() is optional section (named sections ke liye)
_ViewStart.cshtml sets default layout (har view ke liye
```
## Filter
1. Authorization Filter - [Authorize]:
```
[Authorize]  // Login zaruri
public ActionResult SecurePage()
{
    return View();
}

[Authorize(Roles = "Admin")]  // Admin role zaruri
public ActionResult AdminPanel()
{
    return View();
}
```
Tricky MCQ Points:
```
4 types of filters: Authorization, Action, Result, Exception
Filters execute in specific order (order fixed hai)
[Authorize] checks authentication (login check)
[HandleError] needs customErrors enabled in Web.config
Global filters in FilterConfig.cs (sab controllers pe)
Action filters can run before and after action (dono side)
OnActionExecuting runs before action (pehle)
OnActionExecuted runs after action (baad me)
Multiple filters can be applied (ek se zyada)
Order attribute to control sequence (order specify kar sakte)
EXAM TIP: Authorization filter always runs first (sabse pehle)
```
## Security Best Practices:
```
Always use HTTPS (SSL/TLS)
Store passwords hashed (never plain text)
Use AntiForgeryToken for forms
Validate all user input
Use parameterized queries
Don't expose sensitive data in URL
Set appropriate timeout values
Use OutputCache carefully
[Authorize] without parameters = any logged in user
[Authorize(Roles = "Admin")] = specific role needed
[AllowAnonymous] bypasses authorization (public access)
AntiForgeryToken prevents CSRF attacks (form security)
EXAM TIP: [ValidateAntiForgeryToken] must be on POST action only
