**(This version of the repository is archived and unmaintained.)**

This is a set of scripts to build an Odoo installation. It clones the repositories of Odoo and its addons, installs the required Python packages, and generates a configuration file.

It replaces buildout, the system we used at Therp before.

To get started, run `bin/build`. Alternatively, if you want to install with a particular Python version, run for example `bin/build python3.5`.

You can then run `bin/odoo` to launch Odoo.

# Configuring repositories

[`gitaggregate`](https://github.com/acsone/git-aggregator) is used to fetch repositories and merge branches.

Its configuration is written in `repos.yml`. An entry might look like this:

```yaml
parts/server-tools:
  remotes:
    oca: https://github.com/OCA/server-tools
    gfcapalbo: https://github.com/gfcapalbo/server-tools
  merges:
    - oca $ODOO_VERSION 52dfc1f
    - gfcapalbo ${ODOO_VERSION}-mig-letsencrypt
```

`ODOO_VERSION` is a branch name specified in the `versions` file. In this example we'll assume its value is `12.0`.

The entry then tells gitaggregate to build the directory `parts/server-tools` from two git branches:

- Branch `12.0` of repository `https://github.com/OCA/server-tools`, pinned to the commit `52dfc1f`
- Branch `12.0-mig-letsencrypt` of repository `https://github.com/gfcapalbo/server-tools`

The pin `52dfc1f` is optional. It says to use a fixed commit instead of the latest version of the branch.

# Configuring Odoo

After running `bin/build` you'll find three files in the `etc/` directory:

- `odoo.template.cfg` is the default configuration. It's tracked by git, so it should have options that apply to everyone.
- `odoo.override.cfg` is for your own configuration. It's not tracked by git. This is a good place to specify `db_name` and `dbfilter`.
- `odoo.cfg` is generated from the other two files when you run `bin/generate_config` (or `bin/build`). This is the configuration file that's actually used when you run `bin/odoo`.

# Other features

For more information, try reading the scripts in the `bin/` directory. They've been kept as simple as possible.
