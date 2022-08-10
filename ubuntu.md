# Ubuntu command

<https://github.com/jlevy/the-art-of-command-line#everyday-use>

## Commands

- `man [command]` or `[command] --help`: command --help. Find instruction for command. Ex: `cat --help`
- `cat`: 
  - read the file, combines with `grep`
  - concat files
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

- `sudo apt get [package_name] -y`
- `sudo apt remove [package_name] -y`: Then run `sudo apt autoremove` to remove other dependencies of the packages

## Key

- `ctrl + a` & `ctrl + e`: go to start and end of line