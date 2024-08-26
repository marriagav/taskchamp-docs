# Taskchamp

Use [Taskwarrior](https://taskwarrior.org/), a simple command line interface to manage your tasks from you computer, and a beautiful native app to manage them from your phone.

## Installation

Taskchamp can work as a standalone iOS app, but it's recommended to use it with Taskwarrior. To install Taskwarrior, follow the instructions [here](https://taskwarrior.org/download/).

> Taskchamp is only compatible with Taskwarrior 3.0.0 or later.

To install Taskchamp, download the latest release from the App Store.

## Setup with Taskwarrior

> Taskchamp uses iCloud Drive to sync tasks between your computer and your phone. This is described on the Taskwarrior docs [here](https://man.archlinux.org/man/extra/task/task-sync.5.en#ALTERNATIVE:_FILE_SHARING_SERVICES).

> The following instructions have only been tested on macOS, but let me know if you are able to set it up on other systems.

To setup Taskchamp with Taskwarrior, follow these steps:

1. Make sure to have an iCloud account signed in on your phone and computer. Also make sure to have iCloud Drive enabled.
2. Open the Taskchamp app on your phone. This will create a folder in iCloud Drive called `taskchamp`, this is where your tasks database file will live.

> Sometimes it may take some time for the folder to appear on the finder and files app, but you can access it via terminal.

3. Open the taskwarrior configuration file, usually located at `~/.taskrc`, and add the following line:

```bash
data.location=~/Library/Mobile Documents/iCloud~com~mav~taskchamp/Documents/task
```

- This will tell Taskwarrior to use the `taskchamp` folder in iCloud Drive to store the tasks database file.
- This path might be a bit different depending on your system, but you can find the correct path by navigating to the `taskchamp` folder in iCloud Drive and copying the path from the finder, or accessing the `Library/Mobile Documents/iCloud~com~mav~taskchamp/Documents/task` directory from your terminal.
- If you want to use an existing taskwarrior database, you can copy your existing `taskchampion.sqlite3` file to the `~/Library/Mobile Documents/iCloud~com~mav~taskchamp/Documents/task
` folder in iCloud Drive, replacing the existing file. You can do the same for your `hooks` folder if you have any hooks you want to use.

4. iCloud syncing has proven to be less reliable when syncing changes from your iPhone to your computer (works flawless the other way aroung). For this, a shell script has been created to sync the database, ensuring tasks edited on your phone are accurately reflected on your computer.

> I recommend running this script whenever you have made changes on your phone that you want to see on your computer.

The script is very simple:

```bash
#!/bin/bash

task_file="<PATH TO DB FILE>/taskchampion.sqlite3"

# Read the file to trigger the sync
echo "Syncing iCloud file: ${task_file}"
cat "${task_file}" > /dev/null

# Wait for sync
sleep 5
cat "${task_file}" > /dev/null

# Wait for sync
sleep 5
cat "${task_file}" > /dev/null
```

> Make sure to set the correct Path to the taskchampion.sqlite3 file

5. Save this script to a file, for example `sync.sh`, and make it executable by running `chmod +x sync.sh`.

6. Run the script whenever you want to sync your tasks. You can also create a cron job to run the script every few minutes. Another option is to create an alias for the script, so you can run it from the command line easily.

7. Your tasks should now be synced between your computer and your phone. You can add tasks from the command line using Taskwarrior, and they will appear on Taskchamp.
