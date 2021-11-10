Change url from Github.com to Github.dev

Saves changes in localhost - can discard changes (VSCode action)
Can't use terminal - need Codespace for that

You can use sudo without password

Can make ports exposed to everyone in org or on the internet

Codespaces actually run in Docker - everything that can run in it will run on codespaces (for example native dependencies)
Codespaces is assigned to one user at a time.

Coderpad - can we have sharing the screen? Port sharing, Live share extension

Codespaces stop after 0.5h

Devcontainer - customize what CLI tools are installed in codespace. The changes needs to be rebuilt after changing config

We can change remoteUser - other than codespaces (no sudo) or root
We can commit this config so that everyone has the same environment
Extensions can be added to dev container config as well so that everyone has the same extension installed

Prebuild container - was added recently

Codespaces secrets, dotfiles, editor preference (desktop or web) - https://github.com/settings/codespaces

https://github.com/codespaces - change codespace into a branch

Org settings - allow selected users to run codespaces, which respos can run it
Repo settings - env secrets, repo secrets, org secrets, codespaces secrets (we can add secrets in those places)

Put secret in settings, run extra commands to pull changes from APIs to be ready to run codespace when setup

Settings sync - local VScode settings are synced to the codespace as well
GPU support is comming soon, no other OS are in plans
