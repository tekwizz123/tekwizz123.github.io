---
layout: post
title: PyCommands Tutorial
#subtitle: A look into the vulnerability disclosure process and basic fuzzing scripts
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [PyCommands, Immunity Debugger]
---

# Intro
Hey guys so I have been playing around a lot with the Grey Hat Python book and just been walking through the book chapter by chapter so I thought I might share some of the stuff that I have learned so that a) it stays in my mind and b) you guys can also learn more.

Without further ado, lets start!

# Brief Overview of Immunity Debugger and PyCommands
Okay before I start I would like to provide a very brief intro into what I'm about to cover for those of you who might not know what PyCommands are.

Essentially PyCommands are commands (or more specifically, Python scripts) that you can run from within Immunity Debugger that allow you to essentially control various parts of the debugger while it is running. This essentially means that Immunity Debugger is what is known as a "scriptable debugger", and if you don't know what that means, please use your good friend Google who will kindly tell you a much better answer than I can provide here :)

# Getting Started
Okay first off we are going to need to get Immunity Debugger, which can be downloaded from:

[https://debugger.immunityinc.com/register.html](https://debugger.immunityinc.com/register.html)

Please note that the registration info is not needed. In fact you can just click next and continue on.

Next, take a look at the API help guide. While its not much it will help you to make future programs so go ahead and take a look though what functions Immunity Debugger provides for you. It might not make sense right now, but it will give you an idea of the power that is involved.

Access it by doing the following:

1. Open up Immunity Debugger
2. Go to Help, then select "Select API help file"
3. Go to the Documentation file under the Immunity Debugger installation file
4. Select IMMLIB.HLP

Also you can take a look at the online documentation located at [https://debugger.immunityinc.com/Documentation/](https://debugger.immunityinc.com/Documentation/) (this address is new, some old books may reference a different address but this is the one that works atm)

# Running A PyCommand

To run a command, make sure that it is in you PyCommands folder in Immunity Debugger and then find that white box at the bottom and enter:

`!(name of the script goes here) (arg1) (further arguments)`

And then once done, check the Log window (if you don't know where that is, then just go to View and then select Log)

Also please note that all of the scripts that we will be viewing/writing today are based on Python 2.x, not Python 3.x. Make sure you remember that as there are slight differences between the two, and thus this might affect your results. Just saying...feel free to use 3.x if you REALLY want to (hehe).

# Basic Structure of A PyCommand Script
Okay what follows is what you need at a minimal outline to create PyCommand script:

```python
#!/usr/bin/env python
from immlib import *

def main(args):
  # Instantiate a immlib.Debugger instance of the current debugger process that is running right now.
  imm = Debugger()
  
  # Insert your commands to execute here.
  
  # This command is not required, its only so that the command returns input to the user so that they
  # know that the script is done (this is displayed below the white box where you enter the commands)
  return "\[\*\] PyCommand Executed!"
```


# A Couple of Simple Commands + Bits of Information

```python
imm.log("String goes here %d" % <A number. If you want, instead of the code displaying 0xBADF00D, it will display the memory address you place here (note that the decimal value you place here will be converted into a hex value>)
```

This writes the given message to Immunity Debuggers logs (which can be viewed via `View->Logs`). If an address is specified, then that is used in place of the address `0xBADF00D`.

```python
imm.updateLog()
```

Normally when a script runs, the log will only be updated after the script has finished running. If you want the logs to update at a certain spot instead of waiting till the end, then place this after your calls to `imm.log()` to have Immunity's logs update themselves right then and there. Useful if you doing CPU intensive processes and you want to notify the user of what's going on, however note that this will cause the process to take a bit longer to run than if you didn't use the updateLog() call.

```python
imm.createTable('Name', ['name of column one', 'name of column two', 'and so on'])
```

Creates a table with the name (in this case `Name`) and then adds the columns with their given names to the table.

```python
table.add(0 , ["column one's value", "column two's value", "etc"])
```

Pretty self explanatory, 0 is just the number (starting from 0) of the table, and then we add the given column values to that table as specified in the array passed as the second argument.

```python
imm.assemble(*insert your variable here*)
```

Transforms your variable into assembly code.

```python
imm.search(*insert variable here*)
```

Searches all of the memory in the current process for your selected variable (which must be in assembly code) and returns a list of addresses where it found that variable in memory.

```python
imm.readMemory(address, size)
```

Read `size` bytes from the address located in `address` and return them.

```python
imm.writeMemory(address, buf)
```

Write the assembly buffer `buf` into the address located at `address`.

```python
imm.ps()
```
List all the running processes and return (according to the docs) a "list of tuples with process information (pid, name, path, services, tcp list, udp list)"

```python
imm.searchCommands(cmd)
```

Search in all loaded executables for a given command (the command must be given as a string) with newlines represented by `\n`.

```python
imm.getRegs()
```

Returns a Dictionary of all of the x86 registers

```python
imm.setRegs(reg, value)
```

Sets the register represented by the string `reg` to the DWORD value `value`.

```python
imm.getPEB()
```

Get the PEB of the current process.

```python
imm.getPEBaddress()
```

Returns the DWORD address of the current process's PEB.

```python
imm.getAddress(expression)
```

Gets the address of expression (which is a string)
Ex: imm.getAddress("kernel32.Process32FirstW") -> returns the address of `Process32FirstW` which is used to iterate though available processes currently running on the system in case your interested ;)

Okay that should be enough for you to get most scripts done. **If you need to write to a file at any time, the process is the same as you would do in a normal python script.**

# Some Example Scripts

## badchar.py

I claim no credit for this one, as its entirely the work of Justin Setiz from Grey Hat Python and all credit goes to him for creating this snippet, but I thought it was worth sharing.

```python
from immlib import *

def main(args):
  imm = Debugger()
  bad_char_found = False

  # First argument is the address to begin our search
  address = int(args[0],16)

  # Shellcode to verify
  shellcode = "<INSERT SHELLCODE HERE>"
  shellcode_length = len(shellcode)

  debug_shellcode = imm.readMemory(address, shellcode_length)
  debug_shellcode = debug_shellcode.encode("HEX") # We need to encode the debug_shellcode 
                                                    # from assembly into hex format so that we can compare it to
                                                    # the shellcode we inserted just above here.

  imm.log("***************************************")
  imm.log("Address: 0x%08x" % address)
  imm.log("Shellcode Length: %d" % shellcode_length)

  # Begin a byte by byte comparison of the two shellcode buffers
  count = 0
  while count < shellcode_length:
    if debug_shellcode[count] != shellcode[count]:
      imm.log("Bad Char Detected at offset %d" % count)
      bad_char_found = True
      break
    count += 1
  if bad_char_found:
    imm.log("[*****]")
    imm.log("Bad char found %s" % debug_shellcode[count])
    imm.log("Expected to find: %s" % shellcode[count])
    imm.log("[*****]")
  else:
    imm.log("[*****]")
    imm.log("No bad characters found :) Your good to go!")
    imm.log("[*****]")
  return "[*] !badchar finished, check Log window."
```

## Defeat IsDebuggerPresent (for use against malware)
This is the only piece of code that is mine. The creator of this technique was Damian Gomez of Immunity.

```python
from immlib import *

def main(args):
  imm = Debugger()

  # This is the initial test to see what it was before:
  result = imm.readMemory( imm.getPEBAddress() + 0x2, 1 )

  # At PEB + 0x2 in a process is the BeingDebugged variable which is what
  # the function IsBeingDebugged returns. If we set this to 0 then
  # IsBeingDebugged is tricked into thinking that the process is not being
  # debugged because hey, thats what the process is reporting right?
  # And thats one of many ways to get around anti-debugging techniques.
  result = result.encode("HEX")
  imm.log("Before the change: %s" % result)

  # Make the change
  imm.writeMemory( imm.getPEBAddress() + 0x2, "\\x00" )

  # And print the new results
  result = imm.readMemory( imm.getPEBAddress() + 0x2, 1 )
  result = result.encode("HEX")
  imm.log("Result after change: %s" % result)
  return "!defeatDebuggerPresent is done."
```

## findantidep.py

Included by default in Immunity Debugger. This is a GUI program that takes you through the process of disabling DEP on a specific process you are debugging and then returning execution to the area of code your  shellcode is in. I will let you figure out how to use this because your probably smart enough to figure it out yourself if you have a need to use it.


# Conclusion
Thats all folks. Hopefully you can continue to use the examples given above in combination with the API guide to create some awesome scripts. And if you create something really awesome, let me know and if its good I'll post it up on here along with your name :)