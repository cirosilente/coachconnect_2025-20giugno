

✅ 1. Installa Bootstrap via NPM
npm install bootstrap @popperjs/core

Modifica resources/css/app.css:
@import "bootstrap/dist/css/bootstrap.min.css";

Modifica resources/js/app.js:
import 'bootstrap';

Compila gli asset:
npm run build


✅ 2.Installa Font Awesome
npm install --save @fortawesome/fontawesome-free

Importa in resources/css/app.css:
@import '@fortawesome/fontawesome-free/css/all.min.css';

Compila gli asset:
npm run build

php artisan make:controller AuthController : 
- public function showLoginForm() {
  return view('components.login');
  }

✅ 3.Authentication

  public function login(Request $request) {
  $credentials = $request->only('email', 'password');

        if (Auth::attempt($credentials)) {
            $request->session()->regenerate();
            return redirect()->intended('/dashboard');
        }

        return back()->withErrors([
            'email' => 'Credenziali non valide.',
        ]);
  }

  public function showRegisterForm() {
  return view('components.register');
  }

  public function register(Request $request) {
  $request->validate([
  'name' => 'required|string|max:255',
  'email' => 'required|email|unique:users',
  'password' => 'required|min:6|confirmed',
  ]);

        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => Hash::make($request->password),
        ]);

        Auth::login($user);
        return redirect('/dashboard');
  }

  public function logout(Request $request) {
  Auth::logout();
  $request->session()->invalidate();
  $request->session()->regenerateToken();

        return redirect('/login');
  }


in web.php inserire :

Route::get('/login', [AuthController::class, 'showLoginForm'])->name('login');
Route::post('/login', [AuthController::class, 'login']);
Route::get('/register', [AuthController::class, 'showRegisterForm'])->name('register');
Route::post('/register', [AuthController::class, 'register']);
Route::post('/logout', [AuthController::class, 'logout'])->name('logout');

Route::get('/dashboard', function () {
return view('dashboard');
})->middleware('auth');

resources/views/auth/login.blade.php
<form method="POST" action="/login">
    @csrf
    <input type="email" name="email" placeholder="Email" required>
    <input type="password" name="password" placeholder="Password" required>
    <button type="submit">Login</button>
</form>


resources/views/auth/register.blade.php
<form method="POST" action="/register">
    @csrf
    <input type="text" name="name" placeholder="Nome" required>
    <input type="email" name="email" placeholder="Email" required>
    <input type="password" name="password" placeholder="Password" required>
    <input type="password" name="password_confirmation" placeholder="Conferma Password" required>
    <button type="submit">Registrati</button>
</form>


✅ 4. include file css dir resources/css
includerlo nel file:
resources/js/app.js
ad esempio -> import '../css/form.css'
