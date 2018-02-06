# Password Crack

## Password 1: bCveijDSLGBvaXEoeNuzHYVcsnJf

Ran ./mystrings password_1 and found "bCveijDSLGBvaXEoeNuzHYVcsnJf" tried it as the password and it worked.


## Password 2: any palindrome greater than 9 characters in length

First I disassembled the main method, I noticed a call to 3 functions; c, p, and s. I then disassembled the "c" function, in there I see 3 separate calls to the "s" function. Because of this I decided to decompile the "s" function, it is apparent that the "s" function returns the size of password that it was given and returned it in %eax. After the "s" function is called in the main method the program will jump to the end of the main if the value at %eax (the size of the given password calculated in "s") is less than or equal to 9.

Now I disassembled the "p" function to see what was happening in there. I was unable to determine what was happening just by looking at the assembly code so I fired up gdb and set a break at p. Tracing through the lines of code, the function first finds the last character of the password and stores it in %edi. From there it jumps down to <p+52> and determines the length of the password then divides it in half and stores it in %eax. It then jumps back up to <p+31> and stores the first character of the password into %eax. Then checks the first character of the password stored in %eax against the last character of the password stored in %edi and if they are not equal it jumps to the end of the function. If they are equal however they essentially look at the next inner character and test to see if they are the same. I was able to determine that this is essentially checking to see if the password is a palindrome. After learning what the functions do I was able to determine that the password has to be a palindrome that is greater than 9 characters in length.

Examples of passwords: abcdeedcba, aaaaaaaaaa, racecarracecar, neveroddoreven, damnitimmad


## Password 3: Password longer than 9 characters, first 10 characters must be combination of characters 1-6

First I tried to disassemble the main method, unfortunately their was no main method defined. Therefore I used objdump -D password_3 > 3_disassembled.txt to disassemble the executable and place it into a text file so I could print it. Knowing that the code for the executable would be in the .text section with almost no idea about what this program runs I decided to set breakpoints at all of the jump statements, using those and the ni function I tried to establish the what the basic flow of the program is.

The first thing that I noticed was that when the program first gets input it executes a loop between 0x804843b and 0x804844f that counts the size of the input, character by character, if the input is less than or equal to 9 it jumps to the beginning of the loop prompting the user for more input if necessary.

The second loop I found I have determined is the 'main' function. It loops between 0x8048462 and 0x8048485 in this loop what it seems to do is read in one character from the input, from there itsubtracts 0x31 from it and compares the difference with 0x5, if the difference is not greater than 5 it skips over an increment statement -0x10(%ebp), regardless of weather or not it jumps, the function then increments the counter -0xc(%ebp) and checks if that value is greater than or equal to 0xa. Once the loop is done looping it checks that the counter -0x10(%epb) is equal to 0xa if it is not it jumps and displays the "Sorry! Not correct!" message. However if it is equal to 0xa it doesn't jump and displays the "Congratulations!" message.

From these two loops I have been able to determine that the password must be at least 10 characters long, when the 10 characters are subtracted by 0x31 (49 in decimal) it must be less than or equal to 5. With a quick look at the ASCII table I found that the only characters that this is true for is numbers 1-6.

Therefore the password is any string where the first 10 characters are any combination of numbers 1-6.

Examples of passwords: 1111111111, 1234561234, 6666666666,
1234512345ExtraCharactersDontMatter
