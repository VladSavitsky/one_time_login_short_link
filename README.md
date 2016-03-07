The <strong>lightweight</strong> replacement for '<a href="https://www.drupal.org/project/login_one_time" title="'Login one time' module">Login one time</a>' and '<a href="https://www.drupal.org/project/one_time_login" title="'One-time login' module">One-time login</a>' modules.

Module creates <strong>short and secure links  for one-time login.</strong> Links looks like http://example.com/g47Gv2K3. Each link has configurable expire date. Each link could be used only once. This module also supports redirecting the user to a specific internal URL after using the one time login link.

The link could be then sent by email (SMS), shown at screen, written to paper and etc.

<h3>Other solutions</h3>
<ul>
  <li>Drupal has built-in functions <a href="https://api.drupal.org/api/drupal/modules%21user%21user.module/function/user_pass_reset_url/7">user_pass_reset_url()</a> and <a href="https://api.drupal.org/api/drupal/modules!user!user.pages.inc/function/user_pass_reset/7">user_pass_reset()</a> but they expires in 24 hours and has non-changable redirect to user reset form. No integration with Rules module.</li>
  <li>'<a href="https://www.drupal.org/project/login_one_time" title="'Login one time' module">Login one time</a>' assumes a specific workflow with preset email bodies and no short URL support.</li>
  <li>'<a href="https://www.drupal.org/project/one_time_login" title="'One-time login' module">One-time login</a>' module re-uses user_pass_reset_url(). This means link expires in 24 hours and redirect always to password reset form.</li>
  <li></li>
</ul>

<h2>Installation</h2>
<ul>
  <li><a href="https://www.drupal.org/documentation/install/modules-themes/modules-7">To install module using UI please read.</a></li>
  <li>To install only API with Rules module support just run:
<code>drush en one_time_login_short_link -y</code></li>
  <li>To install API + UI:
<code>drush en one_time_login_short_link_ui -y</code></li>
</ul>

<h2>Configuration</h2>
<ul>
  <li>The API-module (one_time_login_short_link) do not require any configuration and works out of the box.</li>
  <li>The UI module (one_time_login_short_link_ui) provides permission which allows to create a links for any user. Without this permission any user (except superuser) can create links only for himself.</li>
</ul>

<h2>API</h2>

This module provides only one simple and clear API-function: one_time_login_short_link(). See example below:
<code>
  $url = one_time_login_short_link( 
    $account = user_load(124),
    $expire = '+1 day',
        // Optional. Default is '+1 day'. Possible values are:
        // Number of seconds from now or from the Unix Epoc 
        // or any date which <a href="http://php.net/manual/en/function.strtotime.php">function strtotime()</a> could handle.
    $redirect = 'node/add' 
        // Optional. Internal page to redirect to.
  );
</code>

<h2>User module integration</h2>

This feature implemented in one_time_login_short_link_ui module. So you need to enable this module first.

<strong>Note:</strong> <em>Destination and Expire date couldn't be set in this case and default values will be used.</em>

<ol>
  <li>Enable module 'One-Time Login Short Link UI'</li>
  <li>Open 'Administration/People/Permissions' page (admin/people/permissions)</li>
  <li>Enable 'One Time Login Short Link UI' permission for desired roles. User without this permission could create links only for himself.</li>
  <li>Open 'Administration/People' page (admin/people).</li>
  <li>Select users from the list.</li>
  <li>In dropdown list 'Update options' choose 'Create one-time login short links for selected users'</li>
  <li>Press 'Update' button.</li>
  <li>Check message at the top of the page with the one-time login links for selected users. If link couldn't be created the reason will be shown instead of link.</li>
</ol>

<h2>Rules module integration</h2>

<h3>Event</h3>
When One Time Login Link is visited  an event 'One-Time Login Short Link was visited' will be fired. This event provide a recently logged-in user object for later use.

<h3>Action</h3>
Module also provides an action 'Create One-Time Login Short Link' which creates the link and passes it to the next action as '<em>[one-time-login-short-link:value]</em>' token.

<strong>Note: You need to restrict access to your rule using Conditions to avoid security issues.
If you're using Rule's Component you should configure it's setting to allow execution to trusted roles only.</strong>

<h2>Maintainers</h2>
Maintained by <a href="https://www.drupal.org/u/vladsavitsky" title="Experienced Drupal Developer">VladSavitsky</a> who is available for professional support and development.

<h2>Similar Modules</h2>

<ul>
  <li><a href="https://www.drupal.org/project/login_one_time" title="'Login one time' module">Login one time</a></li>
  <li><a href="https://www.drupal.org/project/one_time_login" title="'One-time login' module">One-time login</a></li>
</ul>
