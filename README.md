# Arma RCon Class (ARC) 2.1 for PHP 
[![Codacy](https://img.shields.io/codacy/f42d50a9693b4febb34fab3f68315365.svg)](https://www.codacy.com/app/nizari/arma-rcon-class-php)
[![Packagist Version](https://img.shields.io/packagist/v/nizarii/arma-rcon-class.svg)](https://packagist.org/packages/nizarii/arma-rcon-class)
[![GitHub License](https://img.shields.io/github/license/nizarii/arma-rcon-class-php.svg)](https://github.com/Nizarii/arma-rcon-class-php/)

ARC is a lightweight PHP class, which allows you connecting and sending commands easily via RCon to your ARMA game server. See "Supported Servers" for a list of supported games.
<br>
<br>
## Supported Servers
| App ID        | Game          | RCon Support       |
|---------------|---------------|:------------------:|
|233780         | Arma 3        | :white_check_mark: |
|33935          | Arma 2: Operation Arrowhead       | :white_check_mark: |
|33905          | Arma 2        | :white_check_mark: |
<br>
<br>
## Requirements
ARC 2.1 requires **PHP 5.4** or higher, nothing more! *(For <b>PHP 5.3</b> support use [ARC 1.3](https://github.com/Nizarii/arma-rcon-class-php/tree/ARC-1.3))*
<br>
<br>
## Installation 
#### Via Composer
If you haven't already, download Composer
```shell
$ curl -s http://getcomposer.org/installer | php
```
Now require and install ARC
```shell
$ composer require nizarii/arma-rcon-class
$ composer install
```
#### Without Composer
Just include ARC in your project: `require_once 'arc.php';` 
<br>
<br>
## Examples
#### Getting started
After installing ARC, you can easily use ARC as shown below. It will automatically establish a new connection to the server and login:
```php
use \Nizarii\ARC;

$rcon = new ARC(string $ServerIP, string $RConPassword [, int Port = 2302 [, array $Options = array()]]);
```
You are able to send commands with the `command()` function:
```php
//...
$rcon->command("YourCommand");
```
ARC will throw `Exceptions` if anything goes wrong, so you can do a try-catch function:
```php
use \Nizarii\ARC;

try 
{
    $rcon = new ARC('127.0.0.1', 'password');
       
    $array = $rcon->getPlayersArray();
    
    $rcon
        ->sayGlobal('example')
        ->kickPlayer(1, 'example')
        ->sayPlayer(0, 'example')
        ->disconnect()
    ;
    
    $rcon->getBans(); // Throws exception, because the connection was closed
} 
catch (Exception $e) 
{
    echo "Ups! Something went wrong: {$e->getMessage()}";
}
```
Please consider that ARC only checks whether the command has been *successfully sent* via the socket to the server. It *does not* check if the command has been executed on the server-side.
<br><br>
#### Options
Options can be passed to ARC as an array via the fourth parameter of the constructor. The following options are currently available:
* `bool heartbeat = false`: Send a heartbeat packet to the server
* `int timeout_sec = 1`: Sets a timeout value on the connection

*Suggestions for new options are always welcome!*
<br>
Basic options usage:
```php
use \Nizarii\ARC;

$rcon = new ARC('127.0.0.1', 'RCon password', 2322, [
    'heartbeat'    => true,
    'timeout_sec'  => 2
]);
    
//...
```
<br>
## Functions
ARC features many functions to send BattlEye commands easier. After creating a new connections as explained above, you are able to use any of these functions:
* `string command(string $command)`:  Sends any command to the BattlEye server
* `string getPlayers()`:  Gets a list of all players online
* `array getPlayersArray()`:  Gets an array of all players online
* `string getMissions()`:  Gets a list of the available missions on the server
* `string getBans()`:  Gets a list of all BE server bans
* `array getBansArray()`:  Gets an array of all bans
* `object kickPlayer(int $player [, string $reason = 'Admin Kick'])`:  Kicks a player who is currently on the server
* `object sayGlobal(string $message)`:  Sends a global message to all players
* `object sayPlayer(int $player, string $message)`:  Sends a message to a specific player
* `object loadScripts()`:  Loads the "scripts.txt" file without the need to restart the server
* `object maxPing(int $ping)`:  Changes the MaxPing value. If a player has a higher ping, he will be kicked from the server
* `object changePassword(string $password)`:  Changes RCon password
* `object loadBans()`:  (Re)loads the BE ban list from "bans.txt"
* `object banPlayer(string $player [, string $reason = 'Banned' [, int $time = 0]])`:  Ban a player's BE GUID from the server (If the time is 0, the ban will be permanent)
* `object addBan(string $player [, string $reason = 'Banned' [, int $time = 0]])`:  Same as "banPlayer()", but allows to ban a player that is not currently on the server
* `object removeBan(int $banid)`:  Removes a ban
* `object writeBans()`:  Removes expired bans from the bans file
* `object getBEServerVersion()`: Gets the current version of the BE server
* `disconnect()`: Closes the current connection
* `object reconnect()`: Closes the current connection & creates a new one
* `resource getSocket()`: Get the socket used by ARC to send commands

*See [here](https://community.bistudio.com/wiki/BattlEye "BattlEye Wiki") for more information about BattlEye*
<br>
<br>
## License

ARC is licensed under the MIT License. See `LICENSE`-file for further information.