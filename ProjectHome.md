# Moodle - Authentication - Crypt #
a **simple** way to enable crypt passwords in an external database used for user authentication on a moodle - plattform.

  * This code **_should_** work with any current Moodle version (a.e. 1.9.XX).

### 1st: edit auth/db/config.html ###
```
<?php

        $passtype = array();

        $passtype["plaintext"] = get_string("plaintext", "auth");

        $passtype["md5"]       = get_string("md5", "auth");
        
            //crypt

        $passtype["crypt"]     = get_string("crypt", "auth"); 

        $passtype["sha1"]      = get_string("sha1", "auth");

        $passtype["internal"]  = get_string("internal", "auth");

        choose_from_menu($passtype, "passtype", $config->passtype, "");

 ?>
```

### 2nd: edit auth/db/auth.php ###
```
 if ($this->config->passtype === 'md5') {   // Re-format password accordingly

                $extpassword = md5($extpassword);

            }else if ($this->config->passtype === 'crypt'){

            //crypt-auth-mechanism

                echo("mwallner.net crypt-hack logon ");

                $oldpwd = $authdb->Execute("SELECT {$this->config->fieldpass} 
                    FROM {$this->config->table} 
                    WHERE {$this->config->fielduser} = '".$this->ext_addslashes($extusername)."'");    

                $salt = substr($oldpwd, 10, 12);

                $extpassword = crypt($extpassword, $salt);

           }else if ($this->config->passtype === 'sha1') {

                $extpassword = sha1($extpassword);

            }.........

```