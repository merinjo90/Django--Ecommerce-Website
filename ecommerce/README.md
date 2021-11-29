
# Django Ecommerce Website

This is an Ecommerce website, made with Django. It has features such as, add to cart, Add or update products and their details, and PayPal payment

In this Web Application, anyone can buy any product present in the store,
add it to their cart and can pay using the Integrated PayPal payment processing option.

This Web Application is made with the help of the following -->

    Python web framework -- Django
    Python Imaging Library -- Pillow

## Project setup and Template

### Part 1 : Configure App

#### Overview:

In this section we will take care of getting our project set up. We'll first install Django and handle basic configuration before we move on to part 2 where we will set up our templates.

#### Step 1 : Install Django

Assuming you have Python installed, open up your command prompt and Pip install Django if you have not already done so.
##### 1 -- Pip install Django

#### Step 2 : Start Project
Now that we have Django installed, let's create our project. CD into where you want your project files, mine will be in the desktop. We will use django-admin startproject “project name”.

Once you create your project be sure to CD into it before the next step.
##### 1 -- django-admin startproject ecommerce
##### 2 -- cd ~/Desktop/shopping_cart/ecommerce

#### Step 3 : Create app
Create the first app files with python manage.py startapp “appname". 
##### 1 -- python manage.py startapp store

#### Step 4 : Add app to settings.py
When you open up your project you should see the app we just created in your project folder. Make sure you add the new app to INSTALLED_APPS within settings.py


### Part 2 : Templates

#### Overview:
Now that we have our app (store) configured with our project, let's create a folder to store our templates. Our html files will be stored within our app inside a folder called "templates".

#### Step 1 : Create Templates Folder
Inside the new app (store) create a folder called "Templates", inside the templates folder create another folder with the same name as the app. In this case if you called your app "store", like I did, then that should be the name of the folder.

It looks redundant, but the file structure should look like this
##### appname (store) --> tempaltes --> appname (store).

#### Step 2 : Create Templates
Inside the folder we called "store" (Inside the templates folder) is where we will store our html files. 

Right now, let's create all the templates we will need along with one main template that all the other templates will inherit from.

    Main.html → Template which all will inherit from
    Store.html → Home page/store front with all products
    Cart.html → Users shopping cart
    Checkout.html → Checkout page

### Part 3 : Views & URLs

#### Overview:

Let's create some "views" and "url paths" so we can render these templates.
#### Step 1 | Create Views
Inside your apps views.py file create 3 views. Right now we just want to render the templates we created. Let's create and pass in empty context for now.

    def store(request):
        context={}
        return render(request,"store/store.html",context)

    def cart(request):
        context={}
        return render(request,"store/cart.html",context)

    def checkout(request):
        context={}
        return render(request,"store/checkout.html",context)

#### Step 2 | URLs
Now we need to create some "url paths" to call these views.

Create a file called "urls.py" inside your app.

Inside the app import “path” along with the “views” and create a urlpatterns list. Inside "urlpatterns " create 3 paths, one for each view and give them a name.

    from django.urls import path
    from . import views

    urlpatterns=[
        path('',views.store,name="store"),
        path('/cart',views.cart,name='cart'),
        path('/checkout',views.checkout,name='checkout')
    ]

#### Step 3 | Base URLs Configuration
To connect our new urls we need to open up the urls.py file in the root directory and "include" it. 

First import "inlcude" just after "path" and add a path that points to the new urls.py we created inside "store".

    from django.contrib import admin
    from django.urls import path,include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('',include('store.urls'))

    ]

### Part 4 : Static Files
#### Overview:

Before we start working on our templates, let's configure the static files for or images, css and js files.
#### Step 1 | "Static" Folder
In your root directory create a folder called "static".

Inside the new “static” folder, let's create a folder called "CSS" and another called "Images". These files will hold all of our CSS and Images.
#### Step 2 | CSS File
Inside the CSS folder, let's create a file call main.css. This file will later be the main source of styling for each page along with the bootstrap CDN. 

Inside "main.css", let's just style the body and make the background color blue so we can see the effects right away.
#### Step 3 | STATICFILES_DIRES
Now in order to see this effect, let's configure things in settings.py and then add this link to our template.

In "settings.py", we should already have "STATIC_URL". Now we will just need to add "STATICFILES_DIRS" and point it to the new static folder we created in our base directory.

#### Step 4 | Add Static Files to Page
Once we have things configured in settings.py, let's add the stylesheet to our template. For now let's just add it to our home page “store.html”.

First we will need to load static into the template and then add the link with the dynamic path.
#### Step 5 | Add Image
Ok the last thing we need to do for the static files configuration is add in an image and link it to our page.

I will link up the shopping cart icon below. Download the image and add it to your images folder.

### Part 5 : Main Template
#### Overview:

Before we get into designing our template, let's first remove that blue background and image we have.

Your css file should be blank now and your store.html file should only consist of {% load static %} and <h3>store</h3>
#### Step 1 | Add HTML Boilerplate to main.html

Open main.html and create a standard html layout with the title “Ecom".
    
    <!DOCTYPE html>
    <html>
        <head>
           <title>Ecom</title>
        </head>
        <body>

        </body>
    </html>
#### Step 2 | Adding Viewport & Static

Now let's add a few things to the template that we want available on each page.

Just under <!DOCTYPE html> add load static so that we can add images and styling. Under <title> we want to add the viewport meta tag to make our website more mobile responsive (If you don't know what this is; it's ok, just add the tag) and finally our stylsheet that will be made available on every page that inherits from this template.

Viewport meta tag

    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1" />

#### Step 3 | Adding Bootstrap
We will use Bootstrap for a lot of our styling and for the main layout. Let's go to https://getbootstrap.com/ and paste in the necessary css link and javascript links.
#### Step 4 | Container/Navbar Placeholder

Before we add a navbar, let's add a placeholder into the template and ensure that all the pages are inheriting from the main template.

Inside the <body> tag let's first create a div with the class of "container" to center all the content inside the other templates.

Inside the div let's create a block tag for the pages, this is where content for all other pages will set. I added a <br> tag to create some separation between the content and top of the page.

For now let's just add an <h3> tag for the navbar placeholder. We will get to the real navbar in the next step.

    <h3>Navbar Placeholder</h3>
        <hr>
        <div class="container">
            <br>

                {% block content %}
                {% endblock content %}
        </div>

#### Step 5 | Inheriting

Now we want to have all the other pages (store.html, cart.html, checkout.html) inherit from main.html. Let’s add the following code to each template but ensure the text inside the content blocks is still unique.

cart.html
    
    {% extends 'store/main.html' %}
    {% load static %}
    {% block content %}
    <h3>Cart</h3>
    {% endblock content %}

store.html

    {% extends 'store/main.html' %}
    {% load static %}
    {% block content %}
    <h3>Store</h3>
    {% endblock content %}

checkout.html

    {% extends 'store/main.html' %}
    {% load static %}
    {% block content %}
    <h3>Checkout</h3>
    {% endblock content %}

### Part 6 | Navbar
#### Overview:

For our navbar we use something from getbootstrap.com and make some customizations to it after we get it installed. Go to the provided link and find the navbar that looks like the one below and copy the code.

You can also just use the source code I provided in the next step.

#### Step 1 | Bootstrap Navbar
Copy all the code to the navbar and paste it just under the opening body tag in "main.html" and get rid of the placeholder navbar we had there.

#### Step 2 | Dark Theme
To change the color of the nav bar: change the class of "navbar-light" to "navbar-dark" and bg-light to bg-dark

Before:

    <nav class="navbar navbar-expand-lg navbar-light bg-light">

After:

    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">

This is all still in main.html

#### Step 3 | Customize Navbar
We are going to customize the Bootstrap navbar we added to fit our needs.

  1   - Change title from "Navbar" to "Ecom" and set the url to our home page

  2   - Remove extra links

  3   - Remove form wrapper

  4   - Create a div with the class of class="form-inline my-2 my-lg-0". This wrapper needs to hold the positioning of the original form but now hold our shopping cart link and a login button

  5   - Create login button. We will leave the link empty for now until we create the login page

  6   - Create link to cart page

  7   - Inside cart link, add shopping cart image

  8   - Add cart total in paragraph tag with the id of "cart total"

#### Step 4 | Custom CSS

we have set up our main template and added to our main CSS file,

