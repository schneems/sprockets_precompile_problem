# Sprockets Problem

To reproduce run:

```sh
$ git clone git@github.com:schneems/sprockets_precompile_problem.git
$ cd sprockets_precompile_problem
```

Now execute the rake task:

```sh
$ bundle exec rake precompile_problem
```

At the end you will see something like this:

```
============= RESULTS =================
First css:    'public/assets/application-085d08f1e600e8f6de75e7a8180e6930.css'
Modified css: 'public/assets/application-6f3bcd84f5d8f4c23c5fc06b96409d55.css'
First JS:     'var cssFile="/assets/application-58c7c0e35a67f189e19b8c485930e614.css";'
Modified JS:  'var cssFile="/assets/application-58c7c0e35a67f189e19b8c485930e614.css";'
```

If there was no problem the "first" and "modified" should be different for both CSS and JS. Here we see that the first CSS filename is different from the modified CSS filename. However the JS file is not properly updated to reference the correct css file.

```erb
var cssFile = '<%= asset_url("application.css") %>';
```


## Fixed

By adding this to `application.js.erb`

```
//= depend_on_asset "application.css.erb"
```
