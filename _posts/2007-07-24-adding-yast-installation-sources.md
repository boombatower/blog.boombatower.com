---
created: 1185238500
layout: post
redirect_from:
- article/11/
- node/11/
- adding-yast-installtion-sources/
title: Adding YaST Installation Sources
---
First get to the Installation Source screen.

1. Open YaST - ALT + F2 then type _yast_
2. Select Installation Source

    [![](/files/blogger/installationSource.thumbnail.png)](/files/blogger/installationSource.png)
3. Add the desired source. (The following is an example of the Packman source)

    [![](/files/blogger/addSource.thumbnail.png)](/files/blogger/addSource.png)
4. Complete the wizard and the source will have been added.

You will now be able to install software from the added source through _Software Management_. If you are not sure what to enter for the _Server Name_ and _Directory on Server_ fields then consider the following using the Packman url _(as shown in the screenshot)_.

> http://packman.iu-bremen.de/suse/10.2/

_Server Name_: packman.iu-bremen.de _Directory on Server_: /suse/10.2/ In other words the _Server Name_ is the domain and the _Directory on Server_ is the directory in which the source is located on the domain. If given a url you can usually tell what protocol it is. For instance

> http://packman.iu-bremen.de/suse/10.2/

would be _http_. You can tell by the protocol specified at the beginning of the url _http://_. An example of an _ftp_ source would be

> ftp://ftp.suse.com/pub/projects/mozilla/10.2

You can see that the protocal is _ftp_ as specified with _ftp://_. Do not be confused by urls like

> http://ftp.gwdg.de/pub/linux/misc/suser-guru/rpm/10.2/RPMS/

The protocol would still be _http_. Note: A good list for sources is [http://linux.wordpress.com/2006/12/20/opensuse-102-the-most-complete-list-of-repositories/](http://linux.wordpress.com/2006/12/20/opensuse-102-the-most-complete-list-of-repositories/).
