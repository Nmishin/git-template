## What is git-template

This hook creates predefined commit message when you type `git commit`. You can modify it if needed.
It renders the branch name and gets the necessary parameters (such as the Jira issue and commit type)
If the parameters are not available, default parameters will be used.

For example, for branch name `fix/JIRA-2296_cdn`, commit message will looks like:
```
feat: [JIRA-2296] Test commit message

https://jira.example.com/browse/JIRA-2296
```

For branch name, `best_change`, commit message will hve default values:
```
feat: [JIRA-0000] Add the best change
```

## How-to install

Copy this repo to the preffered location (for example ~/.git-template):
```
git clone git@github.com:Nmishin/git-template.git ~/.git-template
```

After that, you need to add a couple of lines to the global git config (usually ~/.gitconfig) for use with this repo:
```
[commit]
        template = /Users/username/.git-template/.gitmessage
[init]
        templateDir = /Users/username/.git-template
        template = /Users/username/.git-template/.gitmessage
```

For now, every newly cloned repo will be preconfigured with these templates.

To add these templates to the existing repos, run the script:
```
#!/bin/sh

set -ue

# Set the path to the updated prepare-commit-msg script
PREPARE_COMMIT_MSG_SCRIPT=$(git config --global init.templateDir)/hooks/prepare-commit-msg

# Find all Git repositories under the current directory
repos=$(find . -name ".git" -type d -prune -exec dirname {} \;)

# Loop through each repository and copy the updated script to the hooks directory
for repo in $repos; do
    hooks_dir="$repo/.git/hooks"
    if [ -d "$hooks_dir" ]; then
        echo "Updating prepare-commit-msg hook in $repo"
        cp "$PREPARE_COMMIT_MSG_SCRIPT" "$hooks_dir/prepare-commit-msg"
    fi
done
```

## Notes
Please note: this `templatedir` will override the default template directory, and if you use some templates from the default directory, you need to copy them from `/Library/Developer/CommandLineTools/usr/share/git-core/templates` (for macOS) or `/usr/share/git-core/templates` for Linux.


## Limitations
This method doesn't work if you add commit messages in a non-interactive manner, such as `git comit -m "commit message"`.

Only the interactive Git commit command `git commit` is supported.


## Links:
[Git Smart: Streamlining Your Workflow with the prepare-commit-msg Hook](https://dev.to/chaz8080/git-smart-streamlining-your-workflow-with-the-prepare-commit-msg-hook-432p)
[Automatically Add Issue URL to Commit Message](https://jasonmfry.wordpress.com/2019/11/13/automatically-add-issue-url-to-commit-message/)
