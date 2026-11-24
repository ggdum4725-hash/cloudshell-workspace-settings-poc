# Cloud Shell workspace-settings proof of concept (benign)

Security research proof of concept prepared for a Google Cloud VRP report.
Tested only against accounts and projects belonging to the author.

## What it demonstrates

Opening this repository in Google Cloud Shell's editor makes Code OSS run a command that
this repository chooses. The repository's own `.vscode/settings.json` defines a terminal
profile and makes it the default; Cloud Shell opens the folder as a trusted workspace
without prompting, and the editor starts an integrated terminal when the workspace opens,
so the command runs with no further interaction.

This also happens when the repository is cloned through an "Open in Cloud Shell" link with
the **Trust repo** box left unticked, which is the point of the report: that dialog strips
credentials and makes the session ephemeral, but it does not stop the repository's settings
from being honoured.

## What the command does

It appends one timestamped line to `$HOME/CLOUDSHELL_VRP_MARKER.txt` and then execs `bash`,
so the terminal behaves normally afterwards. It reads nothing, sends nothing over the
network, and changes nothing else. The entire payload is the one `args` array in
`.vscode/settings.json` and is visible there.

## Removing the marker

    rm -f "$HOME/CLOUDSHELL_VRP_MARKER.txt"
