
Continuous Integration
======================

Check your formulas locally before you create pull request.


Usage
------------------

Update pillars in tests/pillar/\*.sls with test data.
Executed tests with:

.. code-block:: shell

  kitchen list
  kitchen test


Test Kitchen
------------------

Use Travis/Jenkins to spin a kitchen instances in Docker or OpenStack environment.
Default configuration is defined by ``.kitchen.yml`` and ``tests/pillar/*.sls``

Override your specific needs with ``.kitchen.<backend|local>.yml`` that you may load as:
``KITCHEN_LOCAL_YAML=.kitchen.<driver>.yml kitchen <action> <suite>``.

Example: ``KITCHEN_LOCAL_YAML=.kitchen.local kitchen verify server-ubuntu-1404 -t tests/integration``.

Base kitchen-test actions:

1. *create*, provision an test instance (VM, container)
2. *converge*, run a provisioner (shell script, kitchen-salt)
3. *verify*, run a verification (inspec, other may be added)
4. *destroy*


Verifier
~~~~~~~~

The `Busser <https://github.com/test-kitchen/busser>`_ *Verifier* goes with test-kitchen by default.
It is used to setup and run tests implemented in ``<repo>/test/integration``. It guess and installs the particular driver to tested instance.
By default `InSpec <https://github.com/chef/kitchen-inspec>`_ is expected.

You may avoid to install busser framework if you configure specific verifier in ``.kitchen.yml`` and install it kitchen plugin locally:

	verifier:
		name: serverspec

If you would to write another verification scripts than InSpec store them in ``<repo>/tests/integration/<suite>/<busser>/*``.
``Busser <https://github.com/test-kitchen/busser>`` is a test setup and execution framework under test kitchen.


**InSpec**

Implement integration tests under ``<repo>/tests/integration/<suite>/<busser>/*`` directory with ``_spec.rb`` filename
suffix.

Docs:

* https://github.com/chef/inspec
* https://github.com/chef/kitchen-inspec

Requirements
~~~~~~~~~~~~

Use latest stable kitchen-salt and kitchen-test.
Minimal supported version of kitchen-salt is >= v0.0.25.



Jinja templates
---------------
To check jinja templates you may use:

.. code-block:: shell

  cat > check_my_jinja_recursive.py <<-EOF
  import sys
  import os
  from jinja2 import Environment


  def fileList(path, fileTypes):
    matches = []
    for root, dirnames, filenames in os.walk(path):
      for filename in filenames:
        if filename.endswith(fileTypes):
          matches.append(os.path.join(root, filename))
    return matches

  env = Environment()
  for path in sys.argv:
    for template in fileList(path, ('.conf', '.ini', '.jinja2') ):
      with open(template) as t:
        print"Checking:", template
        env.parse(t.read())
  EOF


Intstall Test Kitchen
---------------------
See http://kitchen.ci/ for more details.

To install user side use:

.. code-block:: shell

  # install kitchen
  gem install test-kitchen

  # install additional plugins
  gem install kitchen-docker kitchen-salt
  gem install kitchen-vagrant kitchen-openstack kitchen-inspec busser-serverspec

First you have to install ruby package manager `gem <https://rubygems.org/>`_.

One may be satisfied installing it system-wide right from OS package manager which is preferred installation method.
For advanced users or the sake of complex environments you may use `rbenv <https://github.com/rbenv/rbenv>`_ for user side ruby installation.

 * https://github.com/rbenv/rbenv
 * http://kitchen.ci/docs/getting-started/installing

An example steps then might be:

.. code-block:: shell

  # get rbenv
  git clone https://github.com/rbenv/rbenv.git ~/.rbenv

  # configure
  cd ~/.rbenv && src/configure && make -C src     # don't worry if it fails
  echo 'export PATH="$HOME/.rbenv/bin:$PATH"'>> ~/.bash_profile
  # Ubuntu Desktop note: Modify your ~/.bashrc instead of ~/.bash_profile.
  cd ~/.rbenv; git fetch

  # install ruby-build, which provides the rbenv install command
  git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

  # list all available versions:
  rbenv install -l

  # install a Ruby version
  # maybe you will need additional packages: libssl-dev, libreadline-dev, zlib1g-dev
  rbenv install 2.0.0-p648

  # activate
  rbenv local 2.0.0-p648

  # install test kitchen
  gem install test-kitchen

An optional ``Gemfile`` in the main directory may contain fine tuned dependencies for specific workflows.
To install Gefmfile dependencies run ``gem install bundler`` and then run ``bundler install``.

