# post-merge git hook
This sample demonstrates the use of the post-merge git hook by displaying a message in the console when a `git pull` is invoked locally on master and the contents of `requirements.txt` have changed.

## Sample usage
To get a reminder to update a docker image when Python requirements change, do the following:

1. Add a file named `post-merge` to the `.git/hooks` directory
2. Make the file executable with `chmod +x .git/hooks/post-merge`
3. In that file add the following content:
```bash
#!/bin/sh

red=$'\e[1;31m'
changed_files="$(git diff-tree -r --name-only --no-commit-id ORIG_HEAD HEAD)"

file_modified() {
echo "$changed_files" | grep -E --quiet "$1" && eval "$2"
}

signal_docker_update() {
    echo $red
    echo "+-----------------------------------+"
    echo "|    PYTHON REQUIREMENTS UPDATED    |"
    echo "+-----------------------------------+"
    echo ""
}

file_modified requirements.txt signal_docker_update
```

Now every time you `git pull`, you'll see the message above when the `requirements.txt` file has been updated.