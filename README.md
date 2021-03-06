Stamp for Craft
===========

A tiny plugin for adding timestamp to filenames. 


Usage
---
Use it like this:
 
    <script src="{{ craft.stamp.er('/assets/build/js/scripts.js') }}"></script> 

Which results in:

    <script src="/assets/build/js/scripts.1399647655.js"></script>

The er() method takes a second parameter for setting the format of the output. Possible values are `file` (default), `folder`, `query` and `tsonly`.

Example with `folder`:

    <script src="{{ craft.stamp.er('/assets/build/js/scripts.js', 'folder') }}"></script> 

Result:

    <script src="/assets/build/js/1399647655/scripts.js"></script>

Example with `query`:

    <script src="{{ craft.stamp.er('/assets/build/js/scripts.js', 'query') }}"></script> 

Result:

    <script src="/assets/build/js/scripts.js?ts=1399647655"></script>

Example with `tsonly`:

    Timestamp is: {{ craft.stamp.er('/assets/build/js/scripts.js', 'tsonly') }} 

Result:

    Timestamp is: 1399647655



URL rewriting
---
For methods `file` and `folder` you probably want to do some url rewriting. Below are some examples of how this can be done,
adjust as needed for your server and project setup.

Apache:

    # Rewrites asset versioning, ie styles.1399647655.css to styles.css.
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.+)\.(\d{10})\.(js|css)$ $1.$3 [L]  # /assets/build/js/scripts.1399647655.js
    # RewriteRule ^(.+)/(\d{10})/(.+)\.(js|css)$ $1/$3.$4 [L]  # /assets/build/js/1399647655/scripts.js

nginx:

    location @assetversioning {
        rewrite ^(.+)\.[0-9]+\.(css|js)$ $1.$2 last;  # /assets/build/js/scripts.1399647655.js
        # rewrite ^(.+)/([0-9]+)/(.+)\.(js|css)$ $1/$3.$4 last;  # /assets/build/js/1399647655/scripts.js
    }    

    location ~* ^/assets/.*\.(?:css|js)$ {
        try_files $uri @assetversioning;
        expires max;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }


Configuration
---
Stamp needs to know the public document root to know where your files are located. By default
Stamp will use $_SERVER['DOCUMENT_ROOT'], but on some server configurations this is not the correct 
path. You can configure the path by setting the stampPublicRoot setting in your config file 
(usually found in /craft/config/general.php)
 
####Example

    'stampPublicRoot' => '/path/to/website/public/',


Changelog
---
### Version 1.1
 - Added additional parameter to output filepaths in different formats
 
### Version 1.0
 - Initial release