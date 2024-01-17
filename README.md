# how-to-prevent-host-header-injection-in-laravel
A document about how to prevent host header injection in laravel 

Host header injection is a security vulnerability that occurs when an attacker manipulates the Host header in an HTTP request to trick the web server or application into treating the request as if it came from a different domain. To prevent host header injection in Laravel, you can take the following precautions:

# Use HTTPS:
Always use HTTPS to secure the communication between the client and the server. This helps prevent various attacks, including man-in-the-middle attacks and header manipulation.

# Configure Web Server:
Ensure that your web server is properly configured to reject requests with mismatched Host headers. For example, in Apache, you can configure the server to respond only to specific hostnames using the ServerName and ServerAlias directives.

# Validate Host Header in Middleware:
Create a middleware in Laravel to validate the Host header and ensure that it matches the expected hostname for your application. Add this middleware to the routes or apply it globally.

// Create a middleware (e.g., HostHeaderValidationMiddleware.php)
```
public function handle($request, Closure $next)
{
    $allowedHost = config('app.url'); // Get the expected hostname from your configuration

    if ($request->getHost() !== $allowedHost) {
        abort(400, 'Bad Request');
    }

    return $next($request);
}
```
Register the middleware in your App\Http\Kernel class:
```
protected $middleware = [
    // ...
    \App\Http\Middleware\HostHeaderValidationMiddleware::class,
];
```
Adjust the config('app.url') line to match the hostname you expect for your application.
