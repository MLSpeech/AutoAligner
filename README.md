# cmd_line
The library wraps simple functios that handles input for shell for C++ projects.
# Exmaple
We define an argument like this:
```
  cmdline.add("-i", "your integer", &i, 73);
```

First, we indicated the tag that precedes our arguemnt (`-i`), then we added a description (`"your integer"`), and finally we used a default value of `73`.
We then have to call the parse command which handles the input:
```
   int rc = cmdline.parse(argc, argv);
```
In this example, `rc` will hold the number of arguments given.
We can check what arguments we recieved:
```
  if (rc < 1) {
    cmdline.print_help();
    return EXIT_FAILURE;
  }
```
  
Then, the value entered by the user will then be stored in `i`
```
   std::cout << "i=" << i << std::endl;
```