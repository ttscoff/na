`na` is a script designed to make it easy to see what your next actions are for any project, right from the command line. It works with TaskPaper-formatted files (but any plain text format will do), looking for `@na` tags (or whatever you specify) in todo files in your current folder. 

Used with Taskpaper files, it can add new todo items quickly from the command line, automatically tagging them as next actions.

It can also auto-display next actions when you enter a project directory, automatically locating any todo files and listing their next actions when you `cd` to the project (optionally recursive).

### Features

You can list next actions in files in the current directory by typing `na`. By default, `na` looks for `*.taskpaper` files and extracts items tagged `@na` and not `@done`. All of these can be changed in the configuration.

#### Easy matching

`na` features intelligent project matching. Every time it locates a todo file, it adds the project to the database. Once a project is recorded, you can list its actions by using any portion of the parent directories names. If your project is in `~/Sites/dev/markedapp`, you could quickly list its next actions by typing `na dev mark`. It will always look for the shortest match.

#### Recursion

`na` can also recurse subdirectories to find all todo files in child folders as well. Use the `-r` switch to do a recursive search on the current directory. `na -r` with no arguments will recurse from your current location, looking for todo files in all subdirectories. 

Maximum depth for recursion can be controlled in the config (default is `4`). `na -r` can take a path or project title fragments as arguments as well, and will recurse from the matched directory. A configuration option allows you to have the auto-display recurse by default.

#### Adding todos

You can also quickly add todo items from the command line with the `-a` switch. The script will look for a file in the current directory called `todo.taskpaper` (or whatever extension you've set). 

If found, it will try to locate an `Inbox:` project, or create one if it doesn't exist. Any arguments after `-a` will be combined to create a new task in TaskPaper format. They will automatically be assigned as next actions (tagged `@na`) and will show up when `na` lists the tasks for the project.

### Installation

1. Get the script here: <https://github.com/ttscoff/na/blob/master/na>
2. Place `na` in a folder in your path and ensure that it's executable with `chmod a+x /path/to/na`. 

*The cache of used directories is stored in `~/.tdlist`. I haven't made this configurable yet.*

### Usage

-r        recurse 3 directories deep and concatenate all $NA_TODO_EXT files
-a [todo] add a todo to todo.$NA_TODO_EXT in the current dir
-n        with -a, prompt for a note after reading task
-t        specify an alternate tag (default @na)
          pass empty quotes to apply no automatic tag
-p [X]    add a @priority(X) tag (with -a)
-v        search for tag with specific value (requires -t)
-h        show a brief help message

- **Add todos**
	- `na -a ["todo item"]`: add new todo to project's `.taskpaper` file inbox in the current folder
		- If no "todo item" is specified, it will prompt you for input
		- `-n`: used with -a, prompt for a note after reading task
		- `-t`: specify an alternate tag (default @na)
			+ Pass empty quotes to apply no automatic tag
			+ You can add additional @tags in the task description
		- `-p [X]` add a @priority(X) tag
- **List todos**
	- `na` lists all next actions in the current folder's taskpaper file
		- na caches folders it's used in, so you can use an optional argument to match the dirname of another folder (`na marked`)
		- `-t` (without `-a`) search for a specific tag
			+ `-v` search for a tag with a specific value, e.g. `na -t priority -v 5`
		- `-p` (without `-a`) search for items with a specific priority value (shortcut for `na -t priority -v X`)
	- `-r` (recurse and concatenate `@na` in todo files up to 3 levels deep, works with optional argument to list another folder)
	- for `na` and `na -r`, additional arguments are parsed for best (and shortest) project match
- **Auto-list todos when changing directory**
	- only triggers on directory change command (`cd`,`z`,`j`,`g`,`f`)
	- turn off auto-display entirely in the config
	- set whether or not to auto-display recursively in the config
- **Help**
	- `-h` (display help)

#### Examples

- `na`: list next actions in the current directory
- `na -r`: list next actions recursively from the current directory
- `na ~`: list next actions in your home folder
- `na -r ~` list next actions recursively from home
- `na dev mark`: list next actions in a project located in `~/Sites/dev/marked2app`
- `na -a "Update documentation"`: create a new next action in the `Inbox:` project of `todo.taskpaper` in the current folder

### Configuration

You can configure `na` by setting environment variables before you source it. 

Here are the default values, for reference:

	export NA_TODO_EXT=taskpaper
	export NA_NEXT_TAG=@na
	export NA_DONE_TAG=@done
	export NA_MAX_DEPTH=3

### Auto-listing todos on PWD change

You can add a prompt command to your shell to have na automatically list your next actions when you `cd` into a directory. Add the appropriate command to your login file for your shell:

Bash (in ~/.bash_profile):

```bash
if [[ -z "$PROMPT_COMMAND" ]]; then
	PROMPT_COMMAND="eval 'na --prompt'"
else
	echo $PROMPT_COMMAND | grep -v -q "na --prompt" && PROMPT_COMMAND="$PROMPT_COMMAND;"'eval "na --prompt"'
fi
```

Fish (in ~/.config/fish/conf.d/*.fish):

```fish
function __should_na --on-variable PWD
	# function __should_na --on-event fish_prompt
	test -s (basename $PWD)".taskpaper" && na
end
```

Zsh (in ~/.zshrc):

```zsh
chpwd() { na }
```


### Misc

If you have [gum](https://github.com/charmbracelet/gum) installed, na will use it for command line input when adding tasks and notes.
