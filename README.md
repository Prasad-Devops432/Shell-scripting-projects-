# Shell Scripting Projects

Small Bash scripts for Linux administration and AWS and GitHub automation. Each
script solves one task and is written to run on a standard Linux system.

## Scripts

| Script | What it does | Requirements |
|--------|--------------|--------------|
| `User_Automation_Script` | Creates Linux users with random passwords that must be changed at first login. Passwords go to a restricted file, not the terminal. | Root or sudo, `openssl` |
| `github-api` | Lists the users with read access to a GitHub repository. | `curl`, `jq`, a GitHub token in `GITHUB_TOKEN` |
| `arrays` | Lists the VPC IDs in several AWS regions. | AWS CLI with `ec2:DescribeVpcs` permission |

## Usage

```bash
# Create users (run as root or with sudo)
sudo ./User_Automation_Script alice bob
# Passwords are saved to ./new_user_credentials.txt (permissions 600).
# Deliver that file securely, then delete it.

# List read access on a repository
export GITHUB_TOKEN=<your personal access token>
./github-api octocat Hello-World

# List VPCs across regions
./arrays
```

## Security notes

- `User_Automation_Script` writes passwords to a file. Treat that file as sensitive.
  A better long-term approach is an identity provider or a secrets manager.
- `github-api` reads the token from the environment instead of from the script.
  Use a token with the smallest permission that works.
- Do not commit tokens, passwords or generated credential files to Git.

## Changes from earlier versions

- `User_Automation_Script` no longer prints passwords to the terminal, and it checks
  for existing users with `id` rather than `grep` on `/etc/passwd`.
- `github-api` now reads its credentials from `GITHUB_TOKEN` (earlier versions used
  undefined variables) and has its syntax errors fixed.
- `arrays` corrects the region `ap-south-11` to `ap-south-1`.

## Testing

Each script was checked for syntax with `bash -n`. They have not yet been run
against a live AWS account, GitHub account or Linux user database.
