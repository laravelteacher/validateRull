 Create Custom Validation Rule
In first step, we will create custom validation 
rules by run following command. Bellow command through you can create Validation rules file and we can write a 
logic on that file. So let's run bellow command.
php artisan make:rule CheckAgeRule

Next, after successfully run above command, you will found new "rules" folder in app directory. In rules folder you will also
 found CheckAgeRule.php file. We have to simple write logic there, if you enter even number then return true otherwise don't do anything.

So, you should have file like as bellow:

app/Rules/CheckAgeRule.php

public function passes($attribute, $value)
    {
        if ($value < 30) {
            return true;
        }
    }

    
    public function message()
    {
        return 'The :attribute must be less than 30.';
    }
	
Add Routes	
Here, we need to add two routes for create 
form get method and another for post method. so open your routes/web.php file and add following route.	

use App\Http\Controllers\CustomValidationController;

//Custom Validation
Route::get('/custom-validation', [CustomValidationController::class, 'index'])->name('custom.validation.index');
Route::post('/custom-validation/store', [CustomValidationController::class, 'store'])->name('custom.validation.store');



 Create CustomValidationController
 Here, now we should create new controller as CustomValidationController. So run bellow command and create new controller. bellow controller for create controller.
 php artisan make:controller CustomValidationController
 
 Next bellow command you will find new file in this path app/Http/Controllers/CustomValidationController.php.

In this controller will create seven methods by default as bellow methods:

1)index()

2)store()

So, let's copy bellow code and put on CustomValidationController.php file.

app/Http/Controllers/CustomValidationController.php

use App\Rules\CheckAgeRule;

public function index()
    {
    	return view('customValidation');
    }

    public function store(Request $request)
    {
    	$request->validate([
            'name' => 'required',
            'age' => [
                'required', 
                new CheckAgeRule()
            ]
        ]);

    	return back()->with('success', 'We have received your message');
    }
	
Create View File

Finally at the end of this example, we have to create customValidation.blade.php file, in this file we will create form and display validation error so let's create customValidation.blade.php file and put bellow code:

resources/views/customValidation.blade.php


<!DOCTYPE html>
<html>
<head>
    <title>Laravel 8 Custom Validation</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" integrity="sha256-NuCn4IvuZXdBaFKJOAcsU2Q3ZpwbdFisd5dux4jkQ5w=" crossorigin="anonymous" />
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
<body>
<div class="container">
    <div class="row">
        <div class="col-md-6 offset-3 mt-5">
            <div class="card mt-5">
                <div class="card-header text-center bg-info">
                    <h2 class="text-white"> <strong>Custom Validation</strong></h2>
                </div>
                <div class="card-body">
                    @if (count($errors) > 0)
                        <ul class="alert alert-danger pl-5">
	                        @foreach($errors->all() as $error)
	                           <li>{{ $error }}</li> 
	                        @endforeach
                        </ul>
                    @endif
                    <form action="{{ route('custom.validation.store') }}" method="post">
                        @csrf
                        <div class="form-group">
                            <label>Name</label>
                            <input type="text" name="name" class="form-control">
                        </div>
                        <div class="form-group">
                            <label>Age</label>
                            <input type="text" name="age" class="form-control">
                        </div>
                        <div class="text-center">
                            <button class="btn btn-success"><i class="fa fa-floppy-o" aria-hidden="true"></i> Save </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
</body>
</html>



php artisan serve
http://localhost:8000/custom-validation	
