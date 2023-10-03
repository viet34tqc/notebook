# Ubuntu command

- <https://github.com/jlevy/the-art-of-command-line#everyday-use>
- <https://i.imgur.com/xyGqGui.png>

## Commands

- `man [command]` or `[command] --help`: command --help. Find instruction for command. Ex: `cat --help`
- `cat`: 
  - read the file, combines with `grep`
  - concat files: `cat github-actions.pub >> ~/.ssh/authorized_keys`
- `tail`: reads the last ten (default) rows in the file. Usage: read newest bugs in the log file.
  - `tail -n 5 file_name`: read 5 rows
  - `tail -f file_name`: watch the file
- `grep`: search the file
  - `cat file_name | grep key_word`
- `kill`: shutdown a process
  - `kill [PID]`
  - `kill -9 [PID]`: force to kill
- `top`, `htop`: like task manager on window
- `df -h`: display free space
- `free -h`: display free ram

- `sudo chown -R ubuntu: .`

APT command: Advanced Packaging Tool is a high-level package management tool

- `sudo apt update`: return updated information of all packages. We should run this before upgrade any packages to get the newest packages URL
- `sudo apt upgrade`: real upgrade
- `sudo apt get [package_name] -y`
- `sudo apt remove [package_name] -y`: Then run `sudo apt autoremove` to remove other dependencies of the packages

## Install yarn

```bash
sudo apt remove cmdtest
sudo apt remove yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn -y
yarn install
```

## Key

- `ctrl + a` & `ctrl + e`: go to start and end of line