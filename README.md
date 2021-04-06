# protostar-stack-5


In this challenge, the execution of arbitrary code is required. Such code, written in machine language with hexadecimal encoding is injected via input.  
A common choice is to obtain the execution of a shell.  A machine code that executes a shell is called a shellcode.

The shellcode we want to execute is the following:

execve("/bin/sh");
exit(0);


The shellcode.s file contains the shellcode in assembly. Compile shellcode.s to obtain the shellcode.o object file with the following command: gcc –m32 –c shellcode.s –o shellcode.o

Disassemble shellcode.o to get hex-encoded instructions: objdump --disassemble shellcode.o 

payload.py is used to generate the input to pass to stack5.

1. Transfer the payload.py and shellcode.s to the virtual machine
2. Print the output of the script to a file with the following command: python stack5-payload.py > payload
3. Run gdb -q /opt/protostar/bin/stack5
  ⁃ If present, we delete global variables LINES and COLUMNS:
      - unset env LINES
      - unset env COLUMNS
  ⁃ write: disassemble main
  ⁃ Set the breakpoint just before the leave instruction: b * 'istruction_leave'
  ⁃ r < /tmp/payload
  ⁃ Print the contents of the pointer esp using the command x/a $esp
  ⁃ Edit the python script by inserting the obtained value (es. 0xbffffca0) and run it again to get back the correct payload with the right address
      - nano stack5-payload.py
      - edit script
  ⁃ exit from gbd: q
4. Execute python stack5-payload.py > payload
5. Write: (cat /tmp/payload; cat) | /opt/protostar/bin/stack5                          (*)
6. F I N I S H




(*)We use this command because otherwise the new shell would be closed immediately because it would find an EOF due to the fact that all the contents of STDIN have been consumed by the gets.
