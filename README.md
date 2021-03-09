# Nextcloud with LibreOffice Online

This docker configuration help with setting up Nextcloud with LibreOffice Online

## Preparations

To get started, you need one virtual machines 

Install docker and docker-compose on it.

Then clone this repo. 

## Setup guide

copy the `.env.sample` to `.env` and set the environment variables accordingly.
Note that the database host is called 'db'.

* `NEXTCLOUD_DOMAIN` must be set to the domain where your nextcloud is running, e.g. `yourhost` in this example.
* `LO_ONLINE_USERNAME` and `LO_ONLINE_PASSWORD` are used to access the LibreOffice Online dashboard (not absolutely necessary).

Run `docker-compose up -d`.
This will setup Nextcloud and the database and libreoffice for you.


Main tutorail doc can be found on http://linuxlearn.org/nextcloud-libreoffice

Now configure Nextcloud in your web browser. It's available on https://yourhost
You can change the port in docker-compose.yml.

When nextcloud is set up, install the App "Collabora Online". Then go to Configuration->Collabora Online
and enter the domain name of your  VM, e.g. http://yourhost:9999


### Configuration changes

The configuration is stored in a docker volume. This way LibreOffice Online can be configured on the host system
and the config will not be overwritten when updating the docker container.

The volume is created by docker-compose. You can find the volume name via: `docker volume ls` (eg. `libreoffice-online_config-volume`). Finally, find the location of the config file with `docker volume inspect <volume_name>`.
You should see the mount point, for example: `/var/lib/docker/volumes/libreoffice-online_config-volume/_data`. The file `loolwsd.xml` in that directory is the main config file.

Edit it to your liking, then restart the container: `docker-compose restart`

## Using it

You should now be able to edit documents in your nextcloud with LibreOffice online.
Create a document using the '+' button or upload some.

To access the LibreOffice Online admin dashboard, visit http://yourhost:9999/loleaflet/dist/admin/admin.html and use the credentials from your .env file.

To test just LibreOffice Online without using Nextcloud, visit http://yourhost:9999/loleaflet/dist/loleaflet.html?file_path=file:///opt/libreoffice/share/template/common/internal/idxexample.odt .

