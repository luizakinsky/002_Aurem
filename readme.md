# Creating a New Project
## From scratch

First thing to do is open the Command Prompt on your computer and enter the folder where you want your project to be. Then you'll have to type the following commands:

 - echo "# <repository name\>" >> README.md
 - git init
 - git add README.md
 - git commit -m <times you have commited\>
 

 For example:

 > - echo "# movies" >> README.md
 > - git init
 > - git add README.md
 > - git commit -m "first commit"
 

## With a repository initialized

Just continue with the following commands:

- git remote add <remote name\> <link\>
 - git push -u <remote name\> <person editing\>

 For example:

> - git remote add origin_2 https://github.com/luizakinsky/002_Aurem.git
 > - git push -u origin_2 master

 # Starting a New Project

 For the next step you'll need [Composer](https://getcomposer.org/) installed on your computer.

 Now, run the command below:

  - composer create-project --prefer-dist laravel/laravel <project name\>

  For example:

  > - composer create-project --prefer-dist laravel/laravel movies

  And the following ones after that:

   - npm install
   - npm run dev
   - php artisan migrate
   - php artisan serve

   A server will be started.

   Now, you'll need to have [Xampp](https://www.apachefriends.org/pt_br/index.html) on your computer.


Open the application and start the "Apache" and the "My SQL" modules.
Next thing to do ir write down the link that appeared on the command prompt on your browser.

For example:
 - http://127.0.0.1:8000

 Nice! Now the Laravel page might be appearing to you. But we still need to add some things to make it work the way we want.

 ## Adding login/register to your page

 Press Ctrl+C twice to restart your command prompt.
 Now write down the following commands:

  - composer require laravel/ui --dev
  - php artisan ui vue --auth

  It may be required that you repeat the following commands:

  - npm install
  - npm run dev

  Then, you'll have to repeat the one below:

  - php artisan serve

  After these steps you might be able to see the login and the register buttons at top right on your screen.

  # Creating a Database

  Enter Visual Studio and open your project folder. In there you'll have to search, on the sidebar, for the document named ".env". Now, find the part of the code that looks like this:

>DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=laravel
   DB_USERNAME=root
   DB_PASSWORD=

The one we'll use is:

- DB_DATABASE=laravel

You may change what's after the equal sign according to the name of your project. For example:

> DB_DATABASE=movies

Type down "localhost/phpadmin" in your browser. Then, choose the option "New" on the left sidebar, and name it after your project (as it was defined in the previous step).

Now you should have a database working.

> Ps: always remember to save the changes you made in your project before getting out of it.

Go back to your command prompt and press Ctrl+C twice again.
And, one more time, repeat the commands below:

- php artisan migrate
- php artisan serve

Great! Now you might be able to register and login on your website.

# Customizing the Database

## [Migrations](https://laravel.com/docs/6.x/migrations#generating-migrations)

Once again you'll have to restart your command prompt by pressing Ctrl_C twice.
After, type the following commands:

- php artisan make:migration create_<what you want\>_table

For example:

> - php artisan make:migration create_movies_table

A new file has been created in _database/migrations_

Open it up and find the part that says:

- public function up()

In there you'll be able to add all classes you want in our website. Like this:

- $table-><variable type\>(<name\>, <quantity of characters - [when needed](https://laravel.com/docs/6.x/migrations#creating-columns) ->)

For example:

- $table->string('title', 100);
- $table->string('director', 50);
- $table->string('genre', 10);
- $table->integer('main actor');
- $table->integer('time');
- $table->double('price', 8, 2);

## [Controllers](https://laravel.com/docs/5.7/controllers)

The controllers will group related request handling logic into a single class. Type down:

- php artisan make:controller <controller_name> <controller_type> --model=<name\>

For example:

> - php artisan make:controller MoviesController --resource --model=movies

A question may appear in your screen asking if you want to generate a model. Just type "yes".

After that, you'll have to open the file _routes/web.php_ . In there, you may register a resourceful route to the controller, like this:

- Route::resource('<name\>', '<controller_name>);

For example:

> - Route:resource('movies', 'MoviesController');

Then, repeat the commands below:

- php artisan migrate
- php artisan serve

## [Pagination](https://laravel.com/docs/5.8/pagination)

At this step, we'll define how many items will be displayed per page.

First thing is to open up the file _app/Http/Controllers/<controller_name>_. Second, find the
### public function index()

 This function sends the user to the initial page, displays a listing of the resource. So, in there type:

- $<variable\> = <class\>::latest()->paginate(<number of items\>);
- return view('<name\>.index', compact('<name\>'))
- ->with('i', (request()->input('page, 1)-1)*<number of items\>);

For example:

> - $movies = Movies::latest()->paginate(15);
> - return view('movies.index', compact('movies'))
> - ->with('i', (request()->input('page', 1)-1)*15);

Some more modifications need to be made in some functions:

### public function create()

This one shows the form for creating a new resource. Type down:

- return view('<name\>.create');

For example:

> - return view('movies.create');

### public function store(Request $request)

Stores a newly created resource in storage.

- $request->validate([            
   'name'=> 'required',   
   'detail' => 'required',    
]);
- <class\>::create($request->all());
- return redirect()->route('<name\>.index')
- ->with('success', 'Product created successfully!');

For example:

> - $request->validate([            
   'name'=> 'required',   
   'detail' => 'required',    
]);
> - Movies::create($request->all());
> - return redirect()->route('movies.index')
> - ->with('success', 'Product created successfully!');

### public function show(<class\> $<variable\>)

This function displays the specified resource.

- return view('<name\>.show', compact('<name\>'));

For example:

> - return view('movies.show',compact('movies'));

### public function edit(<class\> $<variable\>)

Shows the form for editing the specified resource.

- return view('<name\>.edit',compact('<name\>'));

For example:

> - return view('movies.edit',compact('movies'));

### public function update(Request $request, <class\> $<variable\>)

This one updates the specified resource in storage.

- $request->validate([            
   'name'=> 'required',   
   'detail' => 'required',    
]);
- $<variable\>->update($request->all());
- return redirect()->route('<name\>.index')
- ->with('success', 'Product updated successfully');

For example:

> - $request->validate([            
   'name'=> 'required',   
   'detail' => 'required',    
]);
> - $movies->update($request->all());
> - return redirect()->route('movies.index')
> - ->with('success', 'Product updated successfully');

### public function destroy(<db_name> $<name\>)

Removes the specified resource from storage.

- $<variable\>->delete();
- return redirect()->route('<name\>.index')
- ->with('success', 'Product deleted successfully');

For example:

> - $movies->delete();
> - return redirect()->route('movies.index')
> - ->with('success', 'Product deleted successfully');

Last thing you'll have to do for this step is enter the file _app/<name\>.php_ and type down the following thing inside the 

### class Product extends Model

- protected $fillable = [
   'name', 'detail'
];

