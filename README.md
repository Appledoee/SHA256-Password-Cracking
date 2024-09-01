# SHA256-Password-Cracking

# Description

As the project name suggests, our task in this activity is to crack the SHA-256 hash that matches the password from our wordlist file.

**`password_hash`**: This is the SHA-256 hash of the current password from the wordlist that your script is checking. Essentially, your script takes each password from the wordlist, hashes it using SHA-256, and stores the result in `password_hash`.

**`wanted_hash`**: This is the target SHA-256 hash that you want to crack. It’s the hash that was given to you (in a challenge or hacking scenario), and your goal is to find which password from the wordlist, when hashed, matches this `wanted_hash`.

A script using ‘sys’ and ‘pwn’ which automates the process of trying many different passwords to see which one matches a given SHA-256 hash.

I will be using a wordlist file less than 10 passwords and including my own password to test the code.

# Features

- pwn library
- sys library
- wordlist file

# Steps

<b> 1. Importing modules </b>

* Import <code>pwn</code> module and <code>sys</code> module
  
```python
import pwn import *
import sys
```

* <code>pwn</code> can be used to manage the connection to a remote system directly from the terminal
* <code>sys</code> module helps to manage how the script runs and interact with the environment

<b> 2. Making sure the correct number of arguments when running the script </b>

```python
if len(sys.argv) !=2:
	print("Invalid arguments!")
	print(">> {} <sha246sum>".format(sys.argv[0]))
	exit()
```

* <code>sys.argv</code> is used to pass information from the command-line into your script, allowing you to specify which SHA-256 hash you want to crack without having to hard-code it into your script.
* <code>len(sys.argv)</code> specifies how many total numbers of items
* <code>!= 2</code> means <code>!=</code> not equal, so the code is saying if the input is not equal to 2, then show an error where it prints Invalid arguments!

For example, if you run ‘python3 [sha256.py](http://sha256.py) hash’ now you have 2 arguments then the program will continue, meanwhile  ‘python3 sha256.py’ which only gives out one argument then it will stop running which is why <code>exit()</code> is for

So, why should there be 2 arguments?

Because, the script expects 2 items in ‘sys.argv’

A. The script name itself which is <code>sha256.py</code> which refers to sys.argv[0]
B. The SHA-256 hash that you want to crack <code> hash </code> which refers to sys.argv[1]

* <code>>></code> is just a decorative element, you can put with or without it.


<b> 3. Crack a given SHA-256 hash using the passwords stored in <code>rockyou.txt</code>: Assigning strings to variables </b>

```python
wanted_hash = sys.argv [1]
password_file = "rockyou.txt"
attempts = 0
```

* Assigning <code>wanted_hash</code> variable is set to <code>sys.argv[1]</code> string which previously sys.argv[1] is the hash argument that you have set, so wanted_hash contains ‘381923902301csbdans’
* Assigning <code>password_file</code> variable to <code>rockyou.txt</code> which is a password wordlist file that has millions of passwords. It’s often used in password cracking. In this case, I made my own password wordlist.
* <code>attempts = 0 </code> is used to keep track of how many passwords the script has tried so far

<b> 4. Crack a given SHA-256 hash by comparing it against a list of potential passwords stored in <code>rockyou.txt</code> </b>

```python
with log.progress("Attempting to crack: {}!\n".format(wanted_hash)) as p:
```
* <code>with</code> statement will help us in opening and closing the file and it makes sure the file is closed properly, you can use without it but you have to do it manually.
* <code>log.process</code> is a function provided by <code>pwn</code> module, which creates a progress bar or status messages
* <code> Attempting to crack: {}!\n".format(wanted_hash)</code> creates a string that includes the value of <code>wanted_hash</code> in place of the placeholder {}.

For example:

your SHA256 is ______________ and your script:

```python
"Attempting to crack: {}!\n".format(wanted_hash)
```

The output:

```python
Attempting to crack: Attempting to crack:5d41402abc4b2a76b9719d911017c592!
```

* <code>!</code>  before <code>\n</code> is a decorative element, you can put without it because the message will print out with <code>!</code> in it.
* <code>\n</code> is a newline character

```python
	with open(password_file, "r", encoding='latin-1') as password_list:
```

* <code>with</code> statement will help us in opening and closing the file and it makes sure the file is closed properly, you can use without it but you have to do it manually.
* **`open(password_file, "r", encoding='latin-1')`**: Opens the file in read mode (`"r"`) and specifies the encoding as `'latin-1'` to properly handle non-ASCII characters in the file.
* We use <code>encoding= ‘latin-1’</code> to specify that we support non-ASCII characters for example passwords that contain `é`, `ñ`, and `ç` or non-English characters.
* **`as password_list`**: This assigns the opened file to the variable `password_list`, which can then be iterated over.

```python
for password in password_list:
```

* <code>for</code> statement is a loop to execute a block of code repeatedly for each item in the sequence

<b> 5. Calculate the SHA256 of the current password </b>
   
```python
password = password.strip("\n").encode('latin-1')
					password_hash = sha256sumhex(password)
```

* <code>password = password.strip("\n").encode('latin-1')</code> prepares the password for hashing
* <code>password.strip(”\n”)</code> removes the newline character from the end of each password
* <code>encode(’latin-1’)</code> is used to support non-ASCII characters
* <code>password_hash = sha256sumhex(password)</code> is used to calculate the SHA-256 hash of the current password.
* <code>sha256sumhex(password</code> is provided by <code>pwntools</code> that generate the SHA256 hash of the byte-encoded password(which means you take a password (a piece of text), convert it into a series of bytes (which is how computers store text), and then use the SHA-256 algorithm to create a unique, fixed-length string of characters that represents that password.
* <code>sha256sumhex(password</code> is used to convert SHA-256 hashing to hexadecimal(hex) format because SHA-256 hashing is a binary string and converting to hex format will make the binary data int a readable strings

<b> 6. Updating the progress message to show the current attempt </b>
   
```python
p.status("[{}] {} == {}".format(attempts, password.decode('latin-1'), password_hash))
```

* <code>p.status</code> is a function from the <code>pwn</code> module, specifically from the <code>log.progress</code> class
* **`log.progress()`**: This function is used to create a progress indicator when performing a long-running operation in a script. It helps keep track of the process by displaying a status message that updates as the script progresses.
* **`p.status`**: Inside the context of `log.progress()`, `p.status` is used to update the current status of the operation. You can call it repeatedly with new messages to inform the user of what’s happening.
* This line has three placeholders {}, {}, and {}, and it provides three arguments: attempts, password, and password_hash. This seems correct too.


<b> 7. Checking to see if the hash of the current password <code>password_hash</code> matches the SHA256 hash password given <code>wanted_hash</code> </b>

```python
if password_hash == wanted_hash:
						p.success("Password hash found after {} attempts! {} hashes to {}!".format(attempts, password))
						exit()
					attempts += 1
				p.failure("password hash not found")
```

* <code>if</code> statement is used for Python to make decisions
* <code>password_hash</code> is the hash of the current password (that you have)
* <code>wanted_hash</code> The password that you are trying to crack (sha256 password that you receive)
* so <code>if password_hash == wanted_hash:</code>
* <code>p.success</code> is a function from the <code>pwn</code module to indicate success
* <code>exit()</code> will stop the program immediately when both the SHA256 password hash and the password that you are trying to crack matches.
* <code>attempts +=1</code> keeps track of each attempt of the passwords in the wordlist
* <code>p.failure(”password hash not found”)</code> uses <code>p.failure</code> from <code>pwn</code> module
* otherwise, if the loop hasn’t found its match then it will print "password hash not found"
  
<b> 8. Before testing it out, generate your current password to SHA256 password. For example, my password is ‘kali’ </b>

```python
echo -ne kali | sha256sum
```
* <code>echo -ne (insert your own password) | sha256sum </code>

<b> 9. Pass the SHA256 hash of your current password to the argument </b>

```python
python3 sha256.py
```
* <code>python3 sha256.py (your own generated SHA256 password)</code>

# Output
![Screenshot 2024-09-02 at 1 46 30 AM](https://github.com/user-attachments/assets/8483d565-6d27-467f-9c61-a77a74d3ca8c)

* In my wordlist file, I inserted 'kali' as one of the passwords and it matches with the SHA256 hash password.
