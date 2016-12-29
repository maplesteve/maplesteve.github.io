checkstyle2redmine – Create issues in Redmine from PHP_CodeSniffer
===================

[Download from github.](https://github.com/maplesteve/checkstyle2redmine)


Tools like [PHP_CodeSniffer](http://pear.php.net/manual/en/package.php.php-codesniffer.php) check your PHP code for compliance with various coding standards.
PHP_CodeSniffer can write the results to a XML file.

**checkstyle2redmine reads this output file and creates a new issue for every error in [Redmine](http://www.redmine.org/).**

### Installation
1. Create a directory (e.g. `c2r`) on your development system.
2. Download checkstyle2redmine.php and put it into a the directory
3. checkstyle2redmine uses the ActiveResource library which can be found [here](https://github.com/lux/phpactiveresource) on github. Download ActiveResource.php and  put it into the directory you created above.
4. Make checkstyle2redmine.php executable.

### Redmine integration
checkstyle2redmine uses the REST API provided by Redmine since version 1.0. Check the [Redmine docu](http://www.redmine.org/projects/redmine/wiki/Rest_api#Authentication) on this topic.
In short:

1. Enable the REST API in your Redmine frontend: Administration -> Settings -> Authentication
2. Get the API key for the Redmine user. You can find your API key on your account page ( /my/account ) when logged in, on the right-hand pane of the default layout.[^1]

### Usage
Generic call of the script:

`checkstyle2redmine.php --file=out.xml --host=URL --API=API_KEY --project=PROJECT_SHORT`

#### Example
Assume the following:

* The output of PHP_CodeSniffer is named `checkstyle.xml` and is in the same directory as the checkstyle2redmine script.
* The short name of the Redmine project in which the issues should be created is `testproject`
* The URL of your Redmine installation is `http://redmine.yourhost.com/`
* Your API key is `ABCD`

Then the call would be:

`checkstyle2redmine.php --file=checkstyle.xml --host=http://redmine.yourhost.com/ --API=API_KEY --project=ABCD`

Note the trailing slash for the host parameter!

[^1]: In my setup, I created a special user "Style Reporter" for the generated issues with the 'Reporter' role.
