<h1>Let's improve our Json response with API Resource </h1>

This tutorial is a increment of tutorials:
- "crud-api-by-laravel" (https://github.com/uilhamello/crud-api-by-laravel)
- "validation-request-laravel" (https://github.com/uilhamello/validation-request-laravel)


Let's get started! :star_struck:


<h2>API Resource</h2>

contextualizing: Normally a API uses to response a Json format with all datas from the Model.
With Laravel it is pretty simple, we just need to use response()->json(Object) and it is done, like below:

```php
    pubic function index(Product $product){
        return response()->json($product->all());
    }
```

The code above will retorn exactly de Product structure in Json format. But what with we want to have just part of those datas? Or in a specific case just show some datas and hides other ones? For sure we can do all these logics in the Controller manipuling datas using Eloquent (ORM), but laravel has a solution for that called API Resource that abstract that logic in another sublayer an provide a bunch of functions to facilite in these cases.

Let's get started! 
We are going to use the case in the tutorial crud-api-by-laravel with a Product Model.

<h3>1° Step - Creating a Resource for Product</h3>


```php
php arisan make:resource ProductResource
```

It has created the file app/Http/Resources/ProductResource.php

<h3>2° Step - Changing resource file generated</h3>


Open ProductResource.php and on the toArray() method, we are going to change the content as that:

```php
    public function toArray($request)
    {
        return [
            "name" => $this->name;
            "quantity" => $this->quantity;
        ];
    }
```

Ok, here we are listing the values that will be returned when ProductResource is called.


<h3>3° Step - Change the Controller</h3>


So far ProductController@index has returned the Product Model results of $product->all(); That return a json with all products and its fields.
If we replace the return for our ProductResource, where we speficated to just return "name" and "quantity", it just hide the others information.

So, let's change as above:

```php
    public function index()
    {
        return ProductResource::collection(Product::all());
    }
```

Now we have just the field "name" and "quantity" of each product from our collection.


Great! Now we have our response logic separeted from Controller and we can manipulate it just as our domain needs.s 

<h3>4° Step - Exploring API Resource</h3>

Exploring the Laravel API Resource:

Let's supose that when the request with a parameter "show=all" we are going to show every datas. Like below:

```php
    public function toArray($request)
    {   
        if($request->get('show') === 'all'){
            return return parent::toArray($request);
        }
    
        return [
            "name" => $this->name;
            "quantity" => $this->quantity;
        ];
    }
```

Now when the request comes with 'show=all' it will return every single data of product.


There are a lot of great posibilities to make your reponse as great as you need. 
That tutorial is just a high-level look at API Resource. And quickly we have manipulated our Model Response and leave the controller cleanner respecting Single Responsibility Principle.


That is Done! :star_struck: