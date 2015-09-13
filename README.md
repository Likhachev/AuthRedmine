AuthRedmine extension allows MediaWiki and Redmine to share authentication info. Redmine does all the work in this case, with all user creation and management being done there. Mediawiki's account creation is disabled and all authentication checks go to the Redmine database.

Plugin inspired by [AuthSymfony](https://www.mediawiki.org/wiki/Extension:AuthSymfony)

Tested with version *1.25*

## Installation

1. Clone repository into *extensions* folder
2. Check plugin settings

## Settings

Place these settings in *LocalSettings.php*

```php
$wgAuthRedmine_useExtDb = true;                // if true, fill out db info below
$wgAuthRedmine_host = 'localhost';             // redmine database host
$wgAuthRedmine_username = 'redmine';           // redmine database user login
$wgAuthRedmine_password = 'password';  // ... password
$wgAuthRedmine_database = 'redmine';     // and name

$wgAuthRedmine_login_groups = array(1, 2, 3);  // groups required to allow logins. leave blank if everyone can log in
$wgAuthRedmine_editor_groups= array(10);  // groups required to allow editing. leave blank if everyone can edit

$wgGroupPermissions['*'] = array();      // no rights for anonymous users
$wgGroupPermissions['*']['read'] = true; // except viewing

$wgGroupPermissions['editor'] = $wgGroupPermissions['user']; // this lets us control who's a "normal user"
$wgGroupPermissions['user'] = array();                       // logged-in users have no rights by default
$wgGroupPermissions['editor']['createaccount'] = false;      // no one can make an account on their own

$wgGroupPermissions['bureaucrat'] = array(); // not using this one, as Symfony does rights management

$wgGroupPermissions['sysop']['createaccount'] = false; // No one makes accounts!

// If we don't want anonymous users to be able to use the site, do this:
$wgGroupPermissions['*']['read'] = false; // Disable viewing by anonymous users
$wgWhitelistRead =  array ("Special:Userlogin"/*, "Main Page", "Help:Contents"*/); // but let them log in

require_once("$IP/extensions/AuthRedmine/AuthRedmine.php");
$wgAuth = new AuthRedmine();
```

