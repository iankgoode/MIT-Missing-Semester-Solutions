# Solutions for Lecture 1 (The Shell)

1. Create a new directory called `missing` under `/tmp`.

   **Solution:** We can use `mkdir /tmp/missing` 

2. Look up the `touch` program. The `man` program is your friend.

   **Solution:** We can use `man touch`.

3. Use `touch` to create a new file called `semester` in `missing`.

   **Solution:**

   We can run the following commands in your terminal.

   ```bash
   cd /tmp/missing
   touch semester
   ```

   Or use a single command.

   ```bash
   touch /tmp/missing/semester
   ```

4. Write the following into that file, one line at a time:

   ```bash
   #!/bin/sh
   curl --head --silent https://missing.csail.mit.edu
   ```

   The first line might be tricky to get working. It's helpful to know that
   `#` starts a comment in Bash, and `!` has a special meaning even within
   double-quoted (`"`) strings. Bash treats single-quoted strings (`'`)
   differently: they will do the trick in this case. See the Bash
   [quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
   manual page for more information.

   **Solution:**

   Run the following commands in your terminal.

   ```bash
   echo '#!/bin/sh' > semester
   echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
   ```
   Remember that `>` allows you to rewire the output of a program to a specific file and `>>` allows you to rewire and append code to a file.

5. Try to execute the file, i.e. type the path to the script (`./semester`)
   into your shell and press enter. Understand why it doesn't work by
   consulting the output of `ls` (hint: look at the permission bits of the
   file).

   **Solution:**

   Run the following commands in your terminal.

   ```bash
   ./semester
   ```

   And the terminal returns the following error.

   ```bash
   permission denied: ./semester
   ```


6. Run the command by explicitly starting the `sh` interpreter, and giving it
   the file `semester` as the first argument, i.e. `sh semester`. Why does
   this work, while `./semester` didn't?

   **Solution:** Because the current user of the terminal does not have executable permissions to this file. However the current user has the permissions to use the `sh` command to run the file and the filename is used as an argument of `sh` command.

7. Look up the `chmod` program (e.g. use `man chmod`).

   **Solution:** We can use `man chmod`.

8. Use `chmod` to make it possible to run the command `./semester` rather than
   having to type `sh semester`. How does your shell know that the file is
   supposed to be interpreted using `sh`? See this page on the
   [shebang](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) line for more
   information.

   **Solution:** We can use `chmod u+x semester` to make the file to be executable for the user.

9. Use `|` and `>` to write the "last modified" date output by
   `semester` into a file called `last-modified.txt` in your home
   directory.

   **Solution:**

   Run the following commands in your terminal.

   ```bash
   ./semester | grep -i last-modified | cut -d " " -f 2- > last-modified.txt
   ```
   What is happening here is the output of the executable `./semester` is being piped into `grep` with the argument "last-modified" which outputs the line containing the last modified information. From there we pipe that data into the `cut` command and specify our delimiter `-d` as whitespace (" ") and the flag `-f` as `2-` to have the program to cut everything before and including the second whitespace. Finally the output of that concatenated sequence is written to the file `last-modified.txt`.

10. Write a command that reads out your laptop battery's power level or your
    desktop machine's CPU temperature from `/sys`. Note: if you're a macOS
    user, your OS doesn't have sysfs, so you can skip this exercise.

    **Solution:**

    Run the following command to check the power level.

    ```bash
    cat /sys/class/power_supply/BAT0/capacity
    ```

    Run the following command to check the CPU temperature.

    ```bash
    cat /sys/class/thermal/thermal_zone0/temp
    ```
