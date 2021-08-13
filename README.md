# Ansible - Active Directory

Install Active Directory (AD) for demo or labbing purposes on a
Microsoft Windows Server 2012 R2 Standard.

## Requirements for VM

- VM with Windows Server 2012 R2 Standard
- Proper DNS (important for AD)
- Allow SSH for Ansible

## Requirements on Ansible controller

Ensure required packages are present.

```shell
# Ubuntu 20.04
sudo apt update
sudo apt install -Vy vim git screen wget telnet python3-pip
sudo apt install -Vy python3.8-venv
mkdir python-virtual-environments && cd python-virtual-environments
python3 -m venv ansible
source ~/python-virtual-environment/ansible/bin/activate
pip3 install ansible-lint ansible
```

Ensure your BASH is recent on MacOS, otherwise globstar does not work.

```bash
brew install bash
chsh -s /usr/local/bin/bash
sudo bash -c 'echo /usr/local/bin/bash >> /etc/shells'
ln -s /usr/local/bin/bash /usr/local/bin/bash-terminal-app
```

- `pip install ansible-lint` Linting used in combination with Git pre-commit hooks
- `pip install pre-commit` to run Git pre-commit hooks
- (optional) If using Python virtual environment (venv), then run `source ~/venvs/ansible/bin/activate`

Keep in mind, `pre-commit run` (with no additional arguments) **only runs** against currently staged files.

## Run Playbooks

MacOS has some Python bug, which results in the following output when
calling an Ansible playbook with `win_ping`, for example.

```bash
[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called
```

Export the following environment variable before running the playbook.

```bash
# Might be needed on MacOS due to a Python bug
export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

Playbooks are run against the Windows machine.
Make sure you meet the above requirements.
Append `-u gcp_user_for_windows` to below `ansible-playbook` commands.

```bash
ansible-playbook win_deploy_ad.yml
ansible-playbook win_ad_users_groups.yml
ansible-playbook win_ssh_server.yml
ansible-playbook win_ping.yml
```
