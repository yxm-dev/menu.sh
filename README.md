About
=======

[menu.sh](https://github.com/yxm-dev/menu.sh) is a bash function that makes easy to create a pure
[bash](https://www.gnu.org/software/bash/)
interactive menu in CLI applications, avoiding additional dependencies as `dialog` or `zenity`. 

* it is based on an
[answer](https://askubuntu.com/questions/1705/how-can-i-create-a-select-menu-in-a-shell-script/1386907#1386907)
by [Guss](https://askubuntu.com/users/6537/guss) to a
[question](https://askubuntu.com/questions/1705/how-can-i-create-a-select-menu-in-a-shell-script) in Ask
Ubuntu. The answer, in turn, was based on [another answer](https://askubuntu.com/questions/1705/how-can-i-create-a-select-menu-in-a-shell-script/563226#563226)
by [user360154](https://askubuntu.com/users/360154/user360154). The later pointed to [his
webpage](https://web.archive.org/web/20180130222805/http://pro-toolz.nset/data/programming/bash/Bash_fancy_menu.html)
where more detailed on the construction was given (accessed in 06/05/23).

Install
=======

Just clone/download the directory and add the file `menu.sh` as part of your `bash` project.

Usage
=======

Single Menu
--------------

In your bash project:

1. source the file `menu.sh`;
2. declare and create an array named `menu_options` or `menu_opt` with the options to be selected;
3. declare and create an associative array `menu_commands` or `menu_cmd` with the commands to be passed to the options;
4. call the function `menu_sh` with `selected_menu_option` or `sel_menu_opt` as a variable.

Explicitly, do something like that:

```bash
    #! /bin/bash
    
    source /path/to/menu.sh
    
    # other code you want
    
    ....
    
    # interactive menu
    declare -a menu_options
    menu_options[0]="option A"
    menu_options[1]="option B"
    ...
    
    declare -Aa menu_commands
    menu_commands["option A"]="command A"
    menu_commands["option B"]="command B"
    ...

    menu.sh "Select an option:" selected_menu_option "${menu_options[@]}"
    exec ${menu_commands[$selected_menu_option]}
```

Multiples Menus
---------------

In the case of using multiples interactive menus in a single project, we suggest to

1. create a file `menu_opt` in your project folder;
2. add to it pairs of numbered arrays `menu_opt_1` and `menu_cmd_1`, `menu_opt_2` and `menu_cmd_2`, etc.,
   corresponding to the ordering that the menus will appear in the project;
3. source both files `menu.sh` and `menu_opt` in your project;
4. call the function `menu_sh` multiple times, each time with `sel_menu_opt_1`, `sel_menu_opt_2`, etc., as a
   variable.
   
* in the case of following this pattern, the arrays need not to be declared. Otherwise, declare them.
   
Explicitly, the `menu_opt` file should be something like that:
   
```bash
# options for menu 1
    declare -a menu_opt_1
    menu_opt_1[0]="option_1 A"
    menu_opt_1[1]="option_1 B"
    ...

# commands for menu 1
    declare -A menu_cmd_1
    menu_cmd_1["option_1 A"]="commands_1 A"
    menu_cmd_1["option_1 B"]="commands_1 B"
    
# options for menu 2
    declare -a menu_opt_2
    menu_opt_2[0]="option_2 A"
    menu_opt_2[1]="option_2 B"
    ...

# commands for menu 2
    declare -A menu_opt_2
    menu_cmd_2["option_2 A"]="commands_2 A"
    menu_cmd_2["option_2 B"]="commands_2 B"

...

```

Furthermore, your project file should be like that:

```bash
    #! /bin/bash
    
    source /path/to/menu.sh
    source /path/to/menu_opt

    # some piece of code
    ...
    
    # menu 1
    
    menu.sh "Select an option:" sel_menu_opt_1 "${menu_opt_1[@]}"
    exec ${menu_cmd_1[$sel_menu_opt_1]}
    
    # other piece of code
    
    ...
    
    # menu 2
    
    menu.sh "Select an option:" sel_menu_opt_2 "${menu_opt_2[@]}"
    exec ${menu_cmd_2[$sel_menu_opt_2]}
    
    ...
    
```


