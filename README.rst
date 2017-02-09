
===========
Git formula
===========

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

Sample pillars
==============

Simplest GIT setup

.. code-block:: yaml

    git:
      client:
        enabled: true

GIT with user setup

.. code-block:: yaml

    git:
      client:
        enabled: true
        user:
        - user:
            name: jdoe
            email: j@doe.com

Reclass with GIT with user setup

.. code-block:: yaml

    git:
      client:
        enabled: true
        user:
        - user: ${linux:system:user:jdoe}

Read more
=========

* http://git-scm.com/
* http://git-scm.com/book/en/Customizing-Git-Git-Configuration
* https://github.com/nesi/puppet-git/tree/master/manifests

Documentation and Bugs
======================

To learn how to install and update salt-formulas, consult the documentation
available online at:

    http://salt-formulas.readthedocs.io/

In the unfortunate event that bugs are discovered, they should be reported to
the appropriate issue tracker. Use Github issue tracker for specific salt
formula:

    https://github.com/salt-formulas/salt-formula-git/issues

For feature requests, bug reports or blueprints affecting entire ecosystem,
use Launchpad salt-formulas project:

    https://launchpad.net/salt-formulas

You can also join salt-formulas-users team and subscribe to mailing list:

    https://launchpad.net/~salt-formulas-users

Developers wishing to work on the salt-formulas projects should always base
their work on master branch and submit pull request against specific formula.

    https://github.com/salt-formulas/salt-formula-git

Any questions or feedback is always welcome so feel free to join our IRC
channel:

    #salt-formulas @ irc.freenode.net

Testing
=======

`Salt formula testing <https://salt-formulas.readthedocs.io/en/latest/develop/testing-formulas.html>`_ and
automated CI is covered in the main `documentation <https://salt-formulas.readthedocs.io/en/latest/develop/testing.html>`_.

To run a local smoke test run:

.. code-block:: shell

  $ make test

To run a local multiplatform rehearsal test run:

.. code-block:: shell

  $ kitchen list

  Instance                    Driver   Provisioner  Verifier  Transport  Last Action

  client-single-ubuntu-1404   Docker   SaltSolo     Inspec    Ssh        Verified
  client-single-ubuntu-1604   Docker   SaltSolo     Inspec    Ssh        Converged
  client-single-centos-71     Docker   SaltSolo     Inspec    Ssh        <Not Created>

  $ kitchen converge [instance]
  $ kitchen verify [instance]

