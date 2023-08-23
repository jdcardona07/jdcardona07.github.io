---
layout: single
title: Hack The Box
date: 2018-11-18
classes: wide
header:
  teaser: /assets/images/htb.png
categories:
  - slae
  - infosec
tags:
  - slae
  - assembly
  - tcp bind shellcode
---
A bind shellcode listens on a socket, waiting for a connection to be made to the server then executes arbitrary code, typically spawning shell for the connecting user. This post demonstrates a simple TCP bind shellcode that executes a shell.

The shellcode does the following:
1. Creates a socket
2. Binds the socket to an IP address and port
3. Listens for incoming connections
4. Redirects STDIN, STDOUT and STDERR to the socket once a connection is made
5. Executes a shell

