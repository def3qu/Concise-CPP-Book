<!-- nav -->
[Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Appendix B →](appendix-b-useful-linux-commands.md)

# Appendix A - Connecting to the Server

We will be doing our work on an Ubuntu Linux server called ludwig. This server has command line access only. You can reach ludwig from anywhere you have internet service. How you connect to ludwig will vary depending on the type of system you are connecting from.

No matter how you connect, the address of the server is:

```text
ludwig.mcs.uvawise.edu
```

and the connection needs to use ssh on port 22.

## Mac OS

For the Mac, you can open up a terminal window and type in the following command:

```bash
ssh username@ludwig.mcs.uvawise.edu
```

You can open a terminal window by clicking the magnifying glass and typing in “terminal.”

You can also use a program called **Warp**. You can download it freely from `warp.dev`. Warp works just like the terminal to connect.

## Linux OS

Similar to the Mac, open a terminal window and type in:

```bash
ssh username@ludwig.mcs.uvawise.edu
```

## Windows OS

Windows does not have the ability to directly connect from a terminal window. Traditionally, students have used the free **Putty** app, available at `putty.org`.

There is now a version of **Warp for Windows** that you can download from `warp.dev`.

## IOS

You can use an app called **Termius** to connect to the server.

## Username and Password

I have created accounts for all of the students enrolled in the class. The username will be the first part of your email, i.e. the bit before the @. Your initial password will be set to **qwe123**

Note that when you are logging in, the password characters will not show on the screen.

As soon as you successfully log in the first time, you need to change your password by typing in:

```bash
passwd
```

This command will first ask for your old password, and then ask you to type in a new one. Ubuntu does a basic check and if your password is a bad one, it will prompt you to choose another.

## Creating a Directory for Class Work

To turn in a program, you have to save it in the proper directory. For this class, you will need to create a directory called CSC1180. Here are the steps to create this directory. It is assumed that you are correctly logged into ludwig.

1. `cd ~` - to ensure you are in your home directory.
2. `mkdir CSC1180` - creates the CSC1180 directory
3. `cd CSC1180` - moves to the CSC1180 directory

Once the directory is created, you need to do a cd CSC1180 each time you log on to ludwig.

See Appendix B for some useful Linux commands and Appendix C for help on editing and compiling files.

---

[← Previous: Chapter 12](12-exception-handling.md) | [Table of contents](https://def3qu.github.io/Concise-CPP-Book/) | [Next: Appendix B →](appendix-b-useful-linux-commands.md)
