<h1>Let's improve our Json response with API Resource </h1>

This tutorial is a increment of tutorials:
- "crud-api-by-laravel" (https://github.com/uilhamello/crud-api-by-laravel)
- "validation-request-laravel" (https://github.com/uilhamello/validation-request-laravel)


Let's get started! :star_struck:


<h2>API Resource</h2>

contextualizing: Normally a API uses as response a Json format.
With Laravel it is pretty simple, we just need to use response()->json(array) and it is done, like below:

```php
    pubic function index(Product $product){
        return response()->json($product->all());
    }
```

The code above will retorn exactly de Product structure in Json format. But with do we want to have just part of those datas? Or in a specific case just show some datas and hides other ones? For sure we can do all these logics in the Controller, but laravel as a API Resource that abstract that logic in another sublayer an provide a bunch of functions to facilite in these cases.

Let's get started! We are going to use the case in the tutorial crud-api-by-laravel with a Product Model.

<h3>1° Step - Creating a Resource for Product</h3>


```php
php arisan make:resource ProductResource
```

It has created the file app/Http/Resources/ProductResource.php


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

Now when we request the product@index for examplo, we are going to receive a json of product with just these two datas.

Great! Now we have our response logic separeted from Controller. 


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
For this tutorial we are just focus in to abstract our response from controller and add to it some logic as example.


That is Done! :star_struck: