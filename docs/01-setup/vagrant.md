# Vagrant and CoolOrg

If you prefer to use Vagrant for your development environment, this will help you get set up. Otherwise follow the instructions in [the backend setup](./backend.md).

## Development Environment

### First time

* [Install Vagrant](http://vagrantup.com)

* [Install Virtualbox](http://virtualbox.org)

* [Install Ansible](http://ansible.com) (make sure you are using >= 2.0)

* [Install ansible-redis](https://github.com/DavidWittman/ansible-redis)

  * `ansible-galaxy install DavidWittman.redis`

* run `vagrant up --provision` from `coolorg/ansible` directory


* connect to the vagrant machine: `vagrant ssh`

* Activate the virtual env: `source ~/.virtualenv/coolorg/bin/activate_edu`

* Create a super user: `./manage.py createsuperuser`

* Run the server: `./manage.py runserver 0.0.0.0:8000`

* Hit: [http://localhost:8000/](http://localhost:8000) in your browser



### Coming back to the code

* Start the vagrant instance again `vagrant up` from the `coolorg/ansible`

* Connect to the vagrant machine: `vagrant ssh`

* Activate the virtual env: `source ~/.virtualenv/coolorg/bin/activate`

* Run the server: `./manage.py runserver 0.0.0.0:8000`


### Installed a fresh DB backup

* Remove the file `db_backup.sql` from the project root directory

* run `vagrant reload --provision` from the host machine
