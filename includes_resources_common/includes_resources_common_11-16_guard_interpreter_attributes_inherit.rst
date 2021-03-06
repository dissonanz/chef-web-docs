.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


The ``guard_interpreter`` property is set to ``:default`` by default. When the ``guard_interpreter`` property is set to ``:default``, the ``not_if`` or ``only_if`` properties will guard statements that **do not inherit** properties that are defined by the |resource script|-based resource in which the ``not_if`` or ``only_if`` guard statement is defined.

For example, the ``not_if`` guard statement in the following example **does not inherit** the ``environment`` property:

.. code-block:: ruby

   bash 'javatooling' do
     environment {'JAVA_HOME' => '/usr/lib/java/jdk1.7/home'}
     code 'java-based-daemon-ctl.sh -start'
     not_if 'java-based-daemon-ctl.sh -test-started'
   end

and requires adding the ``environment`` property to the ``not_if`` guard statement so that it may use the ``JAVA_HOME`` path as part of its evaluation:

.. code-block:: ruby

   bash 'javatooling' do
     environment {'JAVA_HOME' => '/usr/lib/java/jdk1.7/home'}
     code 'java-based-daemon-ctl.sh -start'
     not_if 'java-based-daemon-ctl.sh -test-started', :environment => {'JAVA_HOME' => '/usr/lib/java/jdk1.7/home'}
   end

To inherit properties, add the ``guard_attribute`` property to the resource block and set it to the appropriate value:

* ``:bash`` for |resource script_bash|
* ``:batch`` for |resource batch|
* ``:csh`` for |resource script_csh|
* ``:perl`` for |resource script_perl|
* ``:powershell_script`` for |resource powershell_script|
* ``:python`` for |resource script_python|
* ``:ruby`` for |resource script_ruby|

For example, using the same example as from above, but this time adding the ``guard_interpreter`` property and setting it to ``:bash``:

.. code-block:: ruby

   bash 'javatooling' do
     guard_interpreter :bash
     environment {'JAVA_HOME' => '/usr/lib/java/jdk1.7/home'}
     code 'java-based-daemon-ctl.sh -start'
     not_if 'java-based-daemon-ctl.sh -test-started'
   end

The ``not_if`` statement now inherits the ``environment`` property and will use the ``JAVA_HOME`` path as part of its evaluation.
