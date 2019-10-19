## nginx-using-ansible

#### *This repo has a playbook used for running nginx in ubuntu versions using the below modules*

*Modules used for this playbook are*

* __*yum*__ 
    * used to download nginx using yum packet manager
    ```
    - yum:
        name: nginx
        state: present
    ```
    
* __*copy*__
    * create an index.html from the local and copies to a location on the remote machine with appropriate permission to file
    
    ```
    - copy:
        content: <h1> it works </h1>
        dest: "/var/www/{{ domain }}/index.html"
        owner: www-data
        group: www-data
    ```
    * create conf directory for nginx on the remote machine and giving appropriate permissions
    ```
    - file:
        path: "/var/www/{{ domain }}"
        state: directory
        mode: '0755'
    ```
    
* __*template*__
    * used to create a virtual host file by using variable {{ domain }} and copy to conf file of nginx
    ```
    - template:
        src: devopsarju.ml.conf.tml
        dest: "/etc/nginx/conf.d/{{ domain }}.conf"
    ```
    
* __*file*__
    * used to delete defaullt_site by specifying its path to avoid duplicating port 80 conflict while running with current domain
    ```
    - file:
        path: /etc/nginx/site-enabled/default
        state: absent
    ```
