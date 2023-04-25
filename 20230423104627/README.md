# How to give your bash script tab completion (20230423104627)

## Example Snippet
```bash

#!/usr/bin/bash

declare -a commands=(functionone functiontwo mythirdfunction)
if [[ -n $COMP_LINE ]]; then
    for c in "${commands[@]}"; do
        [[ ${c:0:${#2}} == "${2,,}" ]] && echo "$c"
    done
    exit
fi

```

## Notes
Explaining the reasoning behind this approach and walkthrough the lines of the code example.

- Make sure that your script is available in your `$PATH`. I keep mine in `~/.local/bin/`
- This assumes a script with multiple callable functions.
    - You can see an example of a script that uses this approach in my dotfiles repo. Linked below.
- COMP_LINE is available when hitting tab after a command, for completion.
- I keep the completion snippet at the top of the script file itself.
- The snippet below explained:
    1. `declare -a commands=(functionone functiontwo mythirdfunction)`
        - Declare the functions that you wish to expose in your script.
        - These are just handles - they don't actually do anything.
    2. `if [[ -n COMP_LINE ]]; then`
        - Detect if we have the COMP_LINE variable available (i.e. tab was pressed for completion)
        - If so, enter the `if` block.
    3. `for c in "${commands[@]}"; do`
        - Loop through the commands array that we declare at the start of the script
        - for each loop, assign the currently looped-on command to the letter `c`
    4. `[[ ${c:0:${#2}} == "${2,,}" ]] && echo "$c"` checks if what was typed is matched against commands available (even if just typing the first characters of desired function)
        - `${c:0:${#2}}` is a substring match
            - Take the variable `c`, which is one of our available `commands`.
            - Extract a partial string starting at letter `0` (first letter)
            - Extract until it's reached the character that is the number of letters in the 2nd parameter passed to the script.
                - The first parameter `$1` is the name of the script itself
                - The second parameter `$2` is the thing you type after (the thing to be completed)
            - Check against the text passed in as the second argument `$2`
                - `"${2,,}"` will force whatever is passed as that second parameter to be lower case.
                - It's a nice to have.
            - For the example completion code above, if you entered `myscript func` and hit the TAB key twice, youd get two results below where you tabbed. 
            - If only one of your functions matches what is requested, i.e. typing `myscript mythird` TAB, then the completion will complete on the same line as the script is typed.


In your `.bashrc` file, you'll need to add the following: 
```bash

completion -C myscript myscript

```
- This tells bash to look in the `myscript` script for the `myscript` completions.
- You could put completions in a separate file, but this way keeps everything self-contained and easily portable.

## Links
- [Example script with working completion](https://github.com/davidpeach/.dotfiles/blob/main/bin/.local/bin/zet)
- [Notes on Bash auto-completion on Linux](https://echorand.me/posts/linux_shell_autocompletion/)
