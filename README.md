# Ansible Role: Medusa (for Cassandra Backups)

An Ansible Role that installs [Medusa](https://github.com/thelastpickle/cassandra-medusa) on Linux.

## Installing

```bash
$ ansible-galaxy install nicholasamorim.medusa
```

Or clone this repo inside your `roles` folder.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
    # Set your configuration in the var below.
    # medusa_config:

    # Set this if you want a specific version, otherwise it will install the last one.
    # medusa_version: 0.3.1

    # You can also set this to a github link or whatever pip supports
    medusa_pip_name: cassandra-medusa

    # By default, it will use the same version/pip that is set on Ansible
    # but you change it with this.
    # medusa_pip_executable: pip3.7

    # Another choice, is to set one (or both) vars below.
    # An optional path to a virtualenv directory to install into
    # medusa_pip_virtualenv:

    # The Python executable used for creating the virtual environment
    # medusa_pip_virtualenv_python:

    # if false, will install using the connection user
    medusa_pip_install_as_root: true

    medusa_upload_credentials: false
    # Set the credentials on medusa_credentials if you want it to be uploaded
    # medusa_credentials:
```

## Dependencies

None.

## Example Playbook

```yaml
    - hosts: cassandra
      become: yes
      vars:
        medusa_upload_credentials: true
        medusa_credentials:
          aws_access_key_id: xxxx
          aws_secret_access_key: yyyyy
        medusa_config:
          storage:
            storage_provider: s3
            bucket_name: mybucket
            key_file: /etc/medusa/credentials
      roles:
        - medusa
```

This will install Medusa in all `cassandra` machines, upload credentials, and template the configuration file. It is ready to use after that.

Keep in mind Medusa currently supports Python3.6+ only. It is out of the scope of this role to setup Python, however, we do provide sensible variables to use. Take a look at the `medusa_pip_*` variables above.

Here's an example on installing it using Python3.7 inside a virtualenv:

```yaml
medusa_pip_virtualenv: "/opt/virtualenvs/medusa"
medusa_pip_virtualenv_python: python3.7
```

## License

MIT / BSD

## Author Information

This role was created in 2019 by Nicholas Amorim
