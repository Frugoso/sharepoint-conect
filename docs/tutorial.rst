========
Tutorial
========

On Premises Authentication
==========================
Getting started is easy.  Just create some credentials you will use to connect to SharePoint with HttpNtlmAuth and pass the url and credentials to the Site object. ::

    from shareplum import Site
    from requests_ntlm import HttpNtlmAuth

    cred = HttpNtlmAuth('Username', 'Password')
    site = Site('https://mysharepoint.server.com/sites/MySite', auth=cred)

Office 365 Authentication
==========================
For Office 365 Sharepoint is just as easy. The Office365 class grabs a login token from Microsoft's login servers then It logins the Sharepoint site and uses the cookie for Authentication. Make sure to put just the root url for the site in Office365 and add Https:// at start. ::

    from shareplum import Site
    from shareplum import Office365

    authcookie = Office365('https://abc.sharepoint.com', username='username@abc.com', password='password').GetCookies()
    site = Site('https://abc.sharepoint.com/sites/MySharePointSite/', authcookie=authcookie)


Access REST API
================
You can access aditional features by utilizing the SharePoint REST API in SharePlum.  To access this API you must specify a SharePoint version higher than 2013 when creating your Site object.


::

    from shareplum import Site
    from shareplum import Office365
    from shareplum.site import Version

    authcookie = Office365('https://abc.sharepoint.com', username='username@abc.com', password='password').GetCookies()
    site = Site('https://abc.sharepoint.com/sites/MySharePointSite/', version=Version.v2016, authcookie=authcookie)

The REST API gives you access to the Folder class. ::

    folder = site.Folder('Shared Documents/This Folder')
    folder.upload_file('Hello', 'new.txt')
    folder.get_file('new.txt')
    folder.check_out('new.txt')
    folder.check_in('new.txt', "My check-in comment")
    folder.delete_file('new.txt')


SharePoint Versions
====================
The available version options are given below:

::

    Version.v2007 (default)
    Version.v2010
    Version.v2013
    Version.v2016
    Version.v2019
    Version.v365

There are currently only 2 actual versions implemented: 2007 and 365.  v2010 is an alias for v2007 and v2013, v2016,  and v2019 are aliases for v365.

Add A List
==========

You can easily create a new list for your site. ::

    site.AddList('My New List', description='Great List!', templateID='Custom List')

Upload Data
===========

Upload content to your list with the UpdateListItems method. ::

    new_list = site.List('My New List')
    my_data = data=[{'Title': 'First Row!'},
                    {'Title': 'Another One!'}]
    new_list.UpdateListItems(data=my_data, kind='New')

Download Data
=============

Retrieve the data from a SharePoint list using GetListItems. ::

    sp_data = new_list.GetListItems()

Retrieve the data from your list by specifiying a SharePoint View, ::

    sp_data = new_list.GetListItems('All Items')

or specifying the fields you want. ::

    sp_data = new_list.GetListItems(fields=['ID', 'Title'])


SharePlum will automatically convert the name of the column that is displayed when you view your list in a web browser to the internal SharePoint name so you don't have to worry about how SharePoint stores the data.

Update List Data
================

You can update data in a SharePoint List easily as well.  You just need the ID number of the row you are updating. ::

    update_data = [{'ID': '1', 'Title': 'My Changed Title'},
                   {'ID': '2', 'Title': 'Another Change'}]
    new_list.UpdateListItems(data=update_data, kind='Update')

