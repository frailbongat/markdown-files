## Laravel Framework - Blog Website

### Environment Setup

**Requirements**
```
1. XAMPP or LAMP
2. Composer
3. VSCODE or (any text editor or IDE)
```

**Initialize Project**
```php
composer create-project laravel/laravel {Project Name}
```

**Setup Vitual Host**
```
1. Activate virtual host in httpd.conf
2. Add vhost entry in http-vhosts.conf
3. Update host file
```

### Frontend Process

**Create View**
```
1. In views, create a pages folder
2. In pages, create index.blade.php
```

**Create Controller**
```php
1. php artisan make:controller PagesController
```
```
2. In PagesController, create a function 
```
```php
public function inde(){
	return view('pages.index');
}
```

**Create Route**
```php
Route::get('/', 'PagesController@index');
```

### Blade Templating

**Layouts**
```
1. In view, create layouts folder
2. In layouts, create app.blade.php
3. Put any repeating elements
```
```blade
@yield('content')
@extends('layouts.app')
@section('content')
@endsection
```

**Includes**
```
1. In view, create includes folder
2. In includes, create navbar.blade.php
3. In app.blade.php
```
```blade
@include('include.navbar')
```

### Pass Parameter To Blade Template

**Method 1**
```
1. In controller
```
```php
public function index() {
	$title = 'Welcome To Laravel';
	return view('pages.index', compact('title'));
}
```
```
2. In view
```
```blade
<h1>{{ $title }}</h1>
```

**Method 2**
```
1. In controller
```
```php
public function index() {
	$title = 'Welcome To Laravel';
	return view('pages.index')->with('title', $title);
}
```
```
2. In view
```
```blade
<h1>{{ $title }}</h1>
```

**Method 3 (Multiple Values)**
```
1. In controller
```
```php
public function services() {
	$data = [
		'title' => 'Services',
		'services' => 'Web Design'
	];
	return view('pages.services')->with($data);
}
```
```
2. In view
```
```blade
<h1>{{ $title }}</h1>
<p>{{ $services }}</p>
```

### Assets

**To Link**
```blade
href="{{ asset('css/app.css') }}"
```

**Compile CSS**
```
1. Npm install - required for compiling
2. Change the resources folder css
3. Npm run dev - compile assets
4. Npm run watch - watch assets for changes and compile automatically
```

**Custom CSS**
```
1. In resources/sass
2. Create _custom.scss
3. In app.scss
4. Add @import 'custom';
```

**If you don't wanna use sass**
```
1. Go to public/css
2. Change app.css content
```

### Database

**Create a database in phpmyadmin**

**For Post**
```
1. Make a controller
--resource -> insert basic function for crud
```
```php
php artisan make:controller PostsController --resource
```
```
2. Make a model
-m -> create migration for the model
```
```php
php artisan make:model Post -m
```

**Tables**
```
1. Add tables for post in migration file
2. Before running migration, insert database information in .env file
3. Make migration
```
```php
php artisan migrate
```

**Data**
```
Use tinker for data manipulation - insert, select, etc.
Tinker lets you interact with the databse using eloquent ORM
1. To use tinker
2. To access the model use App\{model_name}
3. To quit
```
```php
1. php artisan tinker
2. App\Post::count()
3. quit
```
```
To create new data
1. Create an instance which is being held in a memory
2. Add data to a column
3. Save it to the database
```
```php
1. $post = new App\Post()
2. $post->title = 'Post One';
2. $post->body = 'This is the first post body';
3. $post->save();
```

**Routes**
```
1. Show routes
2. Create all the routes for resource
```
```php
1. php artisan route:list
2. Route::resource('posts', 'PostsController');
```

### Models

**In post model**
```php
// Table Name
protected $table = 'posts';
// Primary Key
public $primaryKey = 'id';
// Timestamps
public $timestamps = true;
```

**Create a post page**
```
1. In resources/views/posts, create a view
2. Call the view in controller
```

**Fetch Post (In PostsController)**
```
1. Bring in post model
2. Call the eloquent function for fetching data inside index()
```
```php
1. use App\Post;
2. Post::all();

/**
 * Additional Info:
 * Post::all();							show all the post
 * Post::find($id); 					show each post
 * Post::orderBy(‘title’, asc)->get();	Order all post
 */
```

**Pagination**
```php
// 1. Use ->pagination(10)
$posts = Post::orderBy('created_at', 'desc')->paginate(10);
// 2. In view
{{ $posts->links() }}
```

### Forms & Saving Data

**Setup Page**
```
1. In resources/views/posts, create create.blade.php
2. Call it in controller
```

**Use Laravel Collective**
```php
// 1. Install
composer require laravelcollective/html
// 2. Add providers
Collective\Html\HtmlServiceProvider::class,
// 3. Add alias
'Form' => Collective\Html\FormFacade::class,
'Html' => Collective\Html\HtmlFacade::class,
```

**Opening And Closing Form**
```php
{!! Form::open(['action' => 'PostsController@store', 'method' => 'POST']) !!}
{!! Form::close() !!}
```

```
Some examples
{{ Form::label('title', 'Title') }}
{{ Form::text('title', '', ['class' => 'form-control', 'placeholder' => 'Title']) }}
{{ Form::textarea('body', '', ['class' => 'form-control', 'placeholder' => 'Post Body']) }}
{{ Form::submit('Submit', ['class' => 'btn btn-primary']) }}
Sample form group
<div class="form-group">
	{{ Form::label('title', 'Title') }}
	{{ Form::text('title', '', ['class' => 'form-control', 'placeholder' => 'Title']) }}	
</div>

Create a validation when submitting the form
1. inside store function
	$this->validate($request, [
            'title' => 'required',
            'body' => 'required'
        ]);
Add messaging or alerts
1. In views/includes — create messages.blade.php
	@if(count($errors) > 0)
		@foreach($errors->all() as $error)
			<div class="alert alert-danger">
				{{ $error }}
			</div>
		@endforeach
	@endif

	@if(session('success'))
		<div class="alert alert-success">
			{{ session('success') }}
		</div>
	@endif

	@if(session('error'))
		<div class="alert alert-danger">
			{{ session(‘error’) }}
		</div>
	@endif
2. Add to  main layout file
Save the data using tinker commands
$post = new Post;
$post->title = $request->input(‘title’);
$post->body = $request->input(‘body’);
$post->save();
Redirect with a message
return redirect(‘/posts’)->with(‘success’, ‘Post Created’);

Implement form editor
Use laravel-ckeditor
1. Follow installation
2. Follow usage — add it to layout
3. Add the id to the form — body
4. Update show so that it can parse html — {!! $post->body !!}


EDIT POST
1. Create an edit button that will go to /posts/{{ $post->id }}/edit — link
2. Create an edit view similar to create
	change: title — action and add id — values of title & body — Spoof a PUT request
	{{Form::hidden(‘_method’, ‘PUT’)}} 

3. Add codes to edit function — hint: find the post & load up edit view 
4. Add codes to update function — similar to store — change new and use find — change the message

DELETE POST
1. Create a post request using form
2. Spoof a DELETE request
3. Create a submit button for delete
4. In destroy function
5. Find the post
6. Delete a post
7. Redirect to list of posts

USER AUTHENTICATION
1. Enable user authentication — php aritsan make:auth
2. When you create auth, it creates a layout — hence it will replace your old layout
	Copy your layout file
	and let it artisan replace your layout
3. For navbar
	Cut it and paste it to your include navbar
	Get what’s needed from your old navbar
		old links
		the design
4. For layout view
	include navbar
	put content to container
	add messages
	add ckeditor
5. Change home to dashboard (optional)
	Controller — return view — rename file
	LoginController — RegisterController — ResetPasswordController
	routes — delete name — rename view
	Link it to navbar
6. Add create post button to dashboard or home view
Add user_id to post table to connect it to the user who create the post
1. php artisan make:migration add_user_id_to_posts — create a migration file
2. In up function
	Schema::table(‘posts’, function($table){
		$table->integer(‘user_id’)
	});
3. In down function
	Schema::table(‘posts’, function($table){
		$table->dropColumn(‘user_id’)
	});
4. php artisan migrate — to create table
5. In posts table add a user id value to old posts
6. In PostsController add user_id when storing
	before save()
	$post->user_id = auth()->user()->id;

MODEL RELATIONSHIP (BLOG POST AND USER)
1. In post model — tell a post that it i belongs to a user
	public function user(){
		return $this->belongsTo(‘App\User’);
	}
2. In user model — tell a user it can have many posts
	public function posts(){
		return $this->hasMany(‘App\Post’);
	}
3. In dashboard controller — fetch the post for that specific user
	use App\User; — bring in user model
	in index function —  add
	$user_id = auth()->user()->id;
	$user = User::find($user_id);
	return view(‘dashboard’)->with(‘posts’, $user->posts);
4. In dashboard view — render the posts for that specific user
	@if(count($posts) > 0)
		<table  class=“table table-striped”>
			<tr>
				<th>Title</th>
				<th></th>
				<th></th>
			</tr>
			@foreach($posts as post)
				<tr>
					<td>{{ $post->title }}</td>
					<td> 
						//the edit button
					</td>
					<td> // the delete button </td>
				</tr>	
			@endforeach
		</table>
	@else
		<p>You have no posts</p>
	@endif
5. Add user name who write the post all posts and single post (optional)
	{{ $post->user->name }}

ACCESS CONTROL
1. Only registered user can create post
	User middleware from DashboardController — it blocks guest
	Copy to post controller
2. Let guest user access the list of blog and single blog
	Add parameter to middleware
	[‘except’ => [‘index’, ‘show’]]
3. Hide edit and delete in single blog for guest user
	In show view
	@if(!Auth::guest())
	@endif
4. Let only the blog post creator can edit and delete his post
	In show view — inside !auth-guest condition
	@if(Auth::user()->id == $post->user_id)
	@endif
5. Only creator of the post can edit the post even if they manually input the url
	In post controller — under edit function
	after the $post
	// Check for correct user
	if(auth()->user()->id != $post->user_id) {
		return redirect(‘/posts’)->with(‘error’, ‘Unauthorized Page’);
	}
	Do the same thing for delete — destroy function

ADD FILE UPLOADING
1. In create view (this is just the UI)
	right above the submit
	<div class=“form-group”>
		{{Form::file(‘cover_image’)}}
	</div>
	General rule when submitting an image:
	In the form — add an enctype attribute
	‘enctype’ => ‘multipart/form-data’
2. Add another column to posts table
	php artisan make:migration add_cover_image_to_posts — Create migration
	In the migration file — add similar schema from the user id
	In up — Change integer to string set name to ‘cover_image’
	In down — add similar schema, change string to dropColumn
	php artisan migrate — Run migration
	Check database — delete all previous post (optional)	
3. Handle the actual upload
	In post controller — in store — in validation
	Add validation for the image
	‘cover_image’ => ‘image|nullable|max:1999’
	image means it must be an image
	nullable means it is optional
	max:1999 means it must be under 2 megabytes
	After validation
	// Handle File Upload
	if($request->hasFile(‘cover_image’)) {
		// Get Filename with the extension
		$filenameWithExt = $request->file(‘cover_image’)->getClientOriginalName();
		// Get just filename
		$filename = pathinfo($filenameWithExt, PATHINFO_FILENAME);
		// Get just extension
		$extension = $request->file(‘cover_image’)->getClientOriginalExtension();
		// Filename to store
		$fileNameToStore = $filename.’_’.time().’.’.$extension;
		// Upload Image
		$path = $request->file(‘cover_image’)->storeAs(‘public/cover_images’, $fileNameToStore)
	} else {
		$fileNameToStore = ‘noimage.jpg’
	}
	To save it to the database — In create post
	$post->cover_image = $fileNameToStore;
	Make public/cover_images accessable to public — create symlink
	php artisan storage:link
	Create a post and check database if it’s working correctly
	Check public storage and storage folder
4. Display it in posts
	In index view
	<div class=“row>
		<div class=“col-md-4 col-sm-4”>
			<img style=“width: 100%;” src=“/storage/cover_images/{{ $post -> cover_image }}”>
		</div>
		<div class=“col-md-8 col-sm-8”>
			// put title and written by here
		</div>
	</div>
	In show view — under h1
	<img style=“width: 100%;” src=“/storage/cover_images/{{ $post -> cover_image }}”>
5. For edit
	from create view copy file field and enctype to edit view
	Handle update function:
	Copy everything in the store for file upload
	remove else statement
	In edit post before save()
	if($request->hasFile(‘cover_image’)){
		$post->cover_image = $fileNameToStore;
	}
6. When deleting a post the image must also be deleted
	In post controller — in destroy — before delete()
	if($post->cover_image != ‘noimage.jpg’){
		// Delete Image
		Storage::delete(“public/cover_images/$post->cover_image”);
	}
	use Illuminate\Support\Facades\Storage; — bring in storage library
	
EXTRAS
1. Tell if navlink is active
	{{ (request()->is('/')) ? 'active' : '' }}
	{{ (request()->is('about')) ? 'active' : '' }}
	

```