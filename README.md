# Setup:

## EC2 instance for jenkins:
- Install `jenkins` and it's dependencies.
- Create new `freestyle project`, add your `git repo`.
- Add a build step, Execute shell:
    -   ```shell
        source path/to/your/env
        ansible-playbook path/to/your/playbook
        ```
- Make sure to add `AWS_ACCESSKEY_ID` and `AWS_SECRET_ACCESS_KEY` as environment variables for the ansible inventory to work.

## In your github repo:
- Repository settings > Webhooks > Add webhook
- Payload URL: `http://<jenkins_urls>/github-webhook`

## Ansible playbooks:
- There are two playbooks here:
    - ec2.create.yml
    - deploy.yml
- ec2.create.yml: creates an ec2 for the django application to be deployed
- deploy.yml:
    - This ensures the dependencies like python, nginx and git are installed or not.
    - Clones the django application repository to the right location.
    - Creates virtual environment and installs the project dependencies in it.
    - Creates `nginx.conf` and systemd service for gunicorn from templates.
    - Starts or restarts nginx and gunicorn in the end.
