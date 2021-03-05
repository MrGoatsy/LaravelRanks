# Laravel Custom Rank Middleware

## Installation

1. Drag all folders inside the `LaravelRanks` folder into the root of your Laravel project
2. Change the ranks that you want to have in `database\seeders\RankSeeder.php`
2.1 (Optional) Customize ranks by adding an array and changing `RankName` and `RankValue` to what you want.
        [
        'rankName'  => 'RankName',
        'rankValue' => RankValue,
        ],
3. In `app\Http\Kernel.php` add: `'rank' => \App\Http\Middleware\RankChecker::class,` at the bottom.
4. Add `'rank',` to `$fillable` in `user.php`.
5. Add the following to `user.php`
        public function rank() {
            return $this->hasOne(UserRanks::class);
        }
6. Add `$rank = !User::count() ? 999 : 1;` in `RegistrationController.php` after you validate the filled in form details.
7. Add `'rank' => $rank` to your `User::create` for example:
        User::create([
            'name'      => $request->name,
            'username'  => $request->username,
            'email'     => $request->email,
            'password'  => Hash::make($request->password),
            'rank'      => $rank
        ])->sendEmailVerificationNotification();
8. Run `php artisan migrate`.

## Usage

You can authorize a rank to access a route by adding them to the `middleware()` like this:

    public function __construct() {
        $this->middleware(['rank:Admin']);
    }

Or if you want multiple ranks to be able to access a route:

    public function __construct() {
        $this->middleware(['rank:Admin,Moderator']);
    }
