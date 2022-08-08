NGINX Controller Integration Role
==========================

# This repository has been archived. There will likely be no further development on the project and security vulnerabilities may be unaddressed.

Manage integration settings with NGINX Controller.
Integrations describe an endpoint (URI) and credential that NGINX Controller uses to interface with an external system.

Requirements
------------

[NGINX Controller](https://www.nginx.com/products/nginx-controller/)

Role Variables
--------------

### Required Variables

`controller.fqdn` - FQDN of the NGINX Controller instance

`controller.auth_token` - Authentication token for NGINX Controller

`nginx_controller_integration.metadata.name` -  name of the integration

### Template Variables

This role has multiple template related variables. The descriptions and defaults for all these variables can be found in **[vars/main.yml](./vars/main.yml)**

Dependencies
------------

Example Playbook
----------------

To use this role you can create a playbook such as the following (let's name it `nginx_controller_integration.yaml` for the purposes of this example).

```yaml
- hosts: localhost
  gather_facts: no

  vars:
    nginx_controller_user_email: "user@example.com" # Required by nginx_controller_generate_token role
    nginx_controller_user_password: "mySecurePassword" # Required by nginx_controller_generate_token role
    nginx_controller_fqdn: "controller.mydomain.com"
    nginx_controller_validate_certs: false

  tasks:
    - name: Retrieve the NGINX Controller auth token
      include_role:
        name: nginxinc.nginx_controller_generate_token

    - name: Configure the user role
      include_role:
        name: nginxinc.nginx_controller_integration
      vars:
        nginx_controller_integration:
          metadata:
            name:  # the name of the data integration
            displayName:   # a friendly display name for the data integration (spaces and special characters allowed)
            description:  # a description of the data integration
            tags:   # an array of tags
            - tagOne
            - tagTwo
          desiredState:
            type: "GENERIC_INTEGRATION"  # [GENERIC_INTEGRATION, AWS_INTEGRATION] the type of integration being defined
            endpointUri: "https://<>"  # the URI of the target API
            credential:   # the credential necessary to interface with the endpoint
              type: "API_KEY"
              apiKey: "<some guid string>"
```

You can then run `ansible-playbook nginx_controller_integration.yaml` to execute the playbook.

Alternatively, you can also pass/override any variables at run time using the `--extra-vars` or `-e` flag like so `ansible-playbook nginx_controller_integration.yaml -e "nginx_controller_user_email=user@company.com nginx_controller_user_password=notsecure nginx_controller_fqdn=controller.example.local nginx_controller_validate_certs=false"`

You can also pass/override any variables by passing a `yaml` file containing any number of variables like so `ansible-playbook nginx_controller_integration.yaml -e "@nginx_controller_integration_vars.yaml"`

License
-------

[Apache License, Version 2.0](./LICENSE)

Author Information
------------------

[Brian Ehlert](https://github.com/brianehlert)

&copy; [NGINX, Inc.](https://www.nginx.com/) 2020
