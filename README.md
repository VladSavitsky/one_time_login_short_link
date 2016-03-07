[One-Time Login Short Link](https://www.drupal.org/project/one_time_login_short_link) module is the **lightweight** replacement for '[Login one time](https://www.drupal.org/project/login_one_time)' and '[One-time login](https://www.drupal.org/project/one_time_login)' modules.

Module creates short and secure links  for one-time login. Links looks like http://example.com/g47Gv2K3. Each link has configurable expire date. Each link could be used only once. This module also supports redirecting the user to a specific internal URL after using the one time login link.

The link could be then sent by email (SMS), shown at screen, written to paper and etc.

### Other solutions

* Drupal has built-in functions [user_pass_reset_url()](https://api.drupal.org/api/drupal/modules%21user%21user.module/function/user_pass_reset_url/7) and [user_pass_reset()](https://api.drupal.org/api/drupal/modules!user!user.pages.inc/function/user_pass_reset/7) but they expires in 24 hours and has non-changable redirect to user reset form. No integration with Rules module.
* '[Login one time](https://www.drupal.org/project/login_one_time)' assumes a specific workflow with preset email bodies and no short URL support.
* '[One-time login](https://www.drupal.org/project/one_time_login)' module re-uses user_pass_reset_url(). This means link expires in 24 hours and redirect always to password reset form.

## Installation

* [To install module using UI please read](https://www.drupal.org/documentation/install/modules-themes/modules-7).
* To install only API with Rules module support just run: `drush en one_time_login_short_link -y`
* To install API + UI: `drush en one_time_login_short_link_ui -y`

## Configuration

* The API-module (one_time_login_short_link) do not require any configuration and works out of the box.
* The UI module (one_time_login_short_link_ui) provides permission which allows to create a links for any user. Without this permission any user (except superuser) can create links only for himself.

## API

This module provides only one simple and clear API-function: one_time_login_short_link(). See example below:
```
  $url = one_time_login_short_link(
    $account = user_load(124),
    $expire = '+1 day',
        // Optional. Default is '+1 day'. Possible values are:
        // Number of seconds from now or from the Unix Epoc
        // or any date which function strtotime() could handle.
    $redirect = 'node/add'
        // Optional. Internal page to redirect to.
  );
```

## User module integration

This feature implemented in one_time_login_short_link_ui module. So you need to enable this module first.

**Note:** _Destination and Expire date couldn't be set in this case and default values will be used._

1. Enable module 'One-Time Login Short Link UI'
2. Open 'Administration/People/Permissions' page (admin/people/permissions)
3. Enable 'One Time Login Short Link UI' permission for desired roles. User without this permission could create links only for himself.
4. Open 'Administration/People' page (admin/people).
5. Select users from the list.
6. In dropdown list 'Update options' choose 'Create one-time login short links for selected users'
7. Press 'Update' button.
8. Check message at the top of the page with the one-time login links for selected users. If link couldn't be created the reason will be shown instead of link.


## Rules module integration

### Event
When One Time Login Link is visited  an event 'One-Time Login Short Link was visited' will be fired. This event provide a recently logged-in user object for later use.

### Action
Module also provides an action 'Create One-Time Login Short Link' which creates the link and passes it to the next action as '_[one-time-login-short-link:value]_' token.

**Note: You need to restrict access to your rule using Conditions to avoid security issues.
If you're using Rule's Component you should configure it's setting to allow execution to trusted roles only.**

## Maintainers
Maintained by [VladSavitsky](https://www.drupal.org/u/vladsavitsky) who is available for professional support and development.

## Similar Modules

* [Login one time](https://www.drupal.org/project/login_one_time)
* [One-time login](https://www.drupal.org/project/one_time_login)
