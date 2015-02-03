This script was posted on: [Dotbuffer.com Blog](http://dotbuffer.com/script-configure-wordpress-permissions/)

Within your root WordPress directory create an empty file and paste the following content make sure you change the appropriate changeme code lines.

```bash
#!/bin/bash
#
# This script configures WordPress file permissions based on 
# recommendations from:
# http://codex.wordpress.org/Hardening_WordPress#File_permissions
#
# Author: Michael Conigliaro
#
WP_OWNER=changeme # wordpress owner
WP_GROUP=changeme # wordpress group
WP_ROOT=/home/changeme # wordpress root directory
WS_GROUP=changeme # webserver group

# reset to safe defaults
find ${WP_ROOT} -exec chown ${WP_OWNER}:${WP_GROUP} {} ;
find ${WP_ROOT} -type d -exec chmod 755 {} ;
find ${WP_ROOT} -type f -exec chmod 644 {} ;

# allow wordpress to manage wp-config.php (but prevent world access)
chgrp ${WS_GROUP} ${WP_ROOT}/wp-config.php
chmod 660 ${WP_ROOT}/wp-config.php

# allow wordpress to manage .htaccess
touch ${WP_ROOT}/.htaccess
chgrp ${WS_GROUP} ${WP_ROOT}/.htaccess
chmod 664 ${WP_ROOT}/.htaccess

# allow wordpress to manage wp-content
find ${WP_ROOT}/wp-content -exec chgrp ${WS_GROUP} {} ;
find ${WP_ROOT}/wp-content -type d -exec chmod 775 {} ;
find ${WP_ROOT}/wp-content -type f -exec chmod 664 {} ;
```
Save the file as wordpress-perms.sh and set appropriate permissions for that script file using the following command:

chmod +x wordpress-perms.sh

Run the script with the following command:

./wordpress-perms.sh

After successful execution delete wordpress-perms.sh script file and then you are done.

rm wordpress-perms.sh
