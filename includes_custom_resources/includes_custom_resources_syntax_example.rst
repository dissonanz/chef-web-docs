.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


For example, the ``site.rb`` file in the ``exampleco`` cookbook could be similar to:

.. code-block:: ruby

   property :homepage, String, default: '<h1>Hello world!</h1>'

   load_current_value do
     if File.exist?('/var/www/html/index.html')
       homepage IO.read('/var/www/html/index.html')
     end
   end

   action :create do
     package 'httpd'

     service 'httpd' do
       action [:enable, :start]
     end

     file '/var/www/html/index.html' do
       content homepage
     end
   end

where

* ``homepage`` is a property that sets the default HTML for the ``index.html`` file with a default value of ``'<h1>Hello world!</h1>'``
* the (optional) ``load_current_value`` block loads the current values for all specified properties, in this example there is just a single property: ``homepage``
* the ``if`` statement checks to see if the ``index.html`` file is already present on the node. If that file is already present, its contents are loaded **instead** of the default value for ``homepage`` 
* the ``action`` block uses the built-in collection of resources to tell the |chef client| how to install |apache|, start the service, and then create the contents of the file located at ``/var/www/html/index.html``

Once built, the custom resource may be used in a recipe just like the any of the resources that are built into |chef|. The resource gets its name from the cookbook and from the file name in the ``/resources`` directory, with an underscore (``_``) separating them. For example, a cookbook named ``exampleco`` with a custom resource named ``site.rb`` is used in a recipe like this:

.. code-block:: ruby

   exampleco_site 'httpd' do
     homepage '<h1>Welcome to the Example Co. website!</h1>'
     action :create
   end
