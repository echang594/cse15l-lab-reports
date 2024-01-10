## Lab Report 1
### cd (no args)
- Working directory: ~/lecture1
- When the cd command is run with no arguments, the directory is changed to the home directory (~), as seen in the prompt on the next line.
- This is not an error.

![cd no-args](cd-no-args.png)
### cd file
- Working directory: ~/lecture1
- When the cd command is run with a file as an argument, it does not change directory and outputs that the file is not a directory.
- This is an error because cd requires a directory to change to, not a file.

![cd file](cd-file.png)
### cd dir
- Working directory: ~
- When the cd command is run with a directory as an argument, it changes the directory to the given directory.
- This is not an error.

![cd dir](cd-dir.png)
### ls (no args)
- Working directory: ~
- When the ls command is run with no arguments, it outputs the contents of the current working directory.
- This is not an error.

![ls no-args](ls-no-args.png)
### ls file
- Working directory: ~
- When the ls command is run with a file as an argument, it outputs the path of the file.
- This is an error, because this command is used to see the contents of a directory and a file is not a directory.

![ls file](ls-file.png)
### ls dir
- Working directory: ~
- When the ls command is run with a directory as an argument, it outputs the contents of the given directory.
- This is not an error.

![ls dir](ls-dir.png)
### cat (no args)
- Working directory: ~
- When the cat command is run with no args, it seems like it does nothing. But the prompt is gone.

![cat no-args 1](cat-no-args1.png)
- We notice that we can actually type in the command, and when we give some input, it echoes it back!
We see that the command has not completed and allows us to keep giving input and receiving output.

![cat no-args 2](cat-no-args2.png)
- We can return to the prompt by aborting the command using ^C (control C).

![cat no-args 3](cat-no-args3.png)
- In summary, the cat command, when run with no arguments, just outputs the input until it is aborted.
- This is an error because the user most likely does not want to output the input.
### cat file
- Working directory: ~
- When the cat command is run with a file as an argument, it outputs the contents of the given file.
- This is not an error.

![cat file](cat-file.png)
### cat dir
- Working directory: ~
- When the cat command is run with a directory as an argument, it outputs that the path is a directory.
- This is an error because this command is used to output the contents of one or multiple files concatenated together and not directories.

![cat dir](cat-dir.png)
