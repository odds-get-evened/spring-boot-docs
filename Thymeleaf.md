Using the layout template structure, which creates a global HTML template, that can have "fragments" of markup, included in a top-level template.

Be sure to add the necessary dependency for this to work:

```xml
...
<!-- thymeleaf hierarchical layouts -->  
<dependency>  
    <groupId>nz.net.ultraq.thymeleaf</groupId>  
    <artifactId>thymeleaf-layout-dialect</artifactId>  
    <version>2.4.1</version>  
</dependency>
...
```

Create the global hierarchical layout HTML:

```html
<!DOCTYPE html>  
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">  
<head>  
    <meta charset="UTF-8">  
    <title>springles</title>  
    <link rel="stylesheet" th:href="@{/assets/bootstrap-5.3.3/css/bootstrap.min.css}">  
</head>  
<body>  
    <div class="container">  
        <div layout:fragment="content">main content placeholder</div>  
    </div>    
    <script th:src="@{/assets/bootstrap-5.3.3/js/bootstrap.bundle.min.js}"></script>  
</body>  
</html>
```

Using `th:replace` attribute, we can assign the markup location where other page fragments will be added.

Below is a template fragment for the homepage:

```html
<div layout:fragment="content" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout" layout:decorate="~{layout.html}">  
    <div>Welcome, home!s</div>  
</div>
```

