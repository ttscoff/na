`na` is a bash function designed to make it easy to see what your next actions are for any project, right from the command line. It works with TaskPaper-format files (but any plain text format will do), looking for @na tags (or whatever you specify) in todo files in your current folder. It can also auto-display next actions when you enter a project directory, automatically locating any todo files and listing their next actions when you `cd` to the project (optionally recursive).

### Features

You can list next actions in files in the current directory by typing `na`. By default, `na` looks for ".taskpaper" files and extracts items tagged "@na" and not "@done". All of these can be changed in the configuration.

#### Easy matching

`na` features intelligent project matching. Every time it locates a todo file, it adds the project to the database. Once a project is recorded, you can list its actions by using any portion of the parent directories names. If your project is in "~/Sites/dev/markedapp," you could quickly list its next actions by typing `na dev mark`. It will always look for the shortest match.

#### Recursion

`na` can also recurse subdirectories to find all todo files in child folders as well. Use the `-r` switch to do a recursive search on the current directory. `na -r` with no arguments will recurse from your current location, looking for todo files in all subdirectories. Maximum depth for recursion can be controlled in the config (default is 4). `na -r` can take a path or project title fragments as arguments as well, and will recurse from the matched directory. A configuration option allows you to have the auto-display recurse by default.

#### Adding todos

You can also quickly add todo items from the command line with the `-a` switch. The script will look for a file in the current directory called todo.taskpaper (or whatever extension you've set). If found, it will try to locate an "Inbox:" project, or create one if it doesn't exist. Any arguments after `-a` will be combined to create a new task in TaskPaper format. They will automatically be assigned as next actions (tagged "@na") and will show up when `na` lists the tasks for the project.

### Installation

 1. Place na.sh on your disk. You can put it in your home folder, but the location doesn't matter, as long as you adjust the path in the next step accordingly
 2. Add this line to your `~/.bash_profile`
		 
	[[ -s "/Users/ttscoff/scripts/na.sh" ]] && source "$HOME/na.sh"

### Usage

* List todos with the `na` function
  * use an argument to match the dirname of another folder
	* -a (add new todo to todo.taskpaper inbox in current folder)
	* -r (recurse and concatenate @na in todo files up to 3 levels deep)
	* -h (display help)
	* for `na` and `na -r`, additional arguments are parsed for best (and shortest) project match
* Auto-list todos when changing directory
	* only triggers on directory change command (cd,z,j,g,f)
	* turn off auto-display entirely in the config
	* set whether or not to auto-display recursively in the config

#### Examples

* `na`: list next actions in the current directory
* `na -r`: list next actions recursively from the current directory
* `na ~`: list next actions in your home folder
* `na -r ~` list next actions recursively from home
* `na dev mark`: list next actions in a project located in `~/Sites/dev/markedapp`
* `na -a "Update documentation"`: create a new next action in the Inbox: project of `todo.taskpaper` in the current folder

### Configuration

* Edit at the top of `na.sh`
* **NA_TODO_EXT**: (string) extension of your todo files (default "taskpaper")
* **NA_NEXT_TAG**: (string) tag for "Next Action" (default "@na")
* **NA_DONE_TAG**: (string) tag for completed actions (default "@done")
* **NA_MAX_DEPTH**: (int) how many directories deep should -r recurse (default 4)
* **NA_AUTO_LIST_FOR_DIR**: (int, 1 or 0) auto-list a directory's todo file when cd'ing to it? 1 to enable or 0 to disable. (default 1)
* **NA_AUTO_LIST_IS_RECURSIVE**: (int, 1 or 0) should auto-list be recursive? (default 0)