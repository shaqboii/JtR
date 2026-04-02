# Intro to John the Ripper
This repo demonstrates the functionality of the password cracking tool, John the Ripper.

John the Ripper is a widely used open‑source password‑cracking tool designed to help security professionals test the strength of passwords. It works by taking hashed passwords and attempting to recover the original text using methods like dictionary attacks, brute force, and rule‑based transformations. Because it supports many hash types and is highly customizable, it’s a popular choice in penetration testing and cybersecurity training to identify weak or easily guessable passwords so they can be strengthened.

To demonstrate how JtR works, let's practice cracking a password with a popular wordlist, rockyou.txt.

Use openSSL to generate a hash of your password. To do this, run ```openssl passwd -1 password123 > hash.txt```. OpenSSL is a powerful command-line tool used to create keys, certificates, hashes, and other cryptographic operations. ```passwd``` is a subcommand of OpenSSL used to tell it to create a hash suitable for being stored in a password file. ```-1``` signals the hashing algorithm used. 1 uses MD5, but OpenSSL also supports other algorithms. A flag of 5 uses SHA-256, and a flag of 6 uses SHA-512. ```password123``` is the password being hashed, and by using ```> hash.txt```, we feed the hash into a new file named hash.txt. So what the command is telling us in simple terms is to: Use OSSL to create a hash for password123 that can be stored in a password database, and then feed that hash into file hash.txt.

Check the contents of hash.txt.

<img width="279" height="49" alt="image" src="https://github.com/user-attachments/assets/02c2aa1f-6589-4648-a52d-67021e25c34a" />

If you do this repeatedly, you will find that the hash is always different, even if you use the same password and hashing algorithm. This is because OSSL uses salting when generating hashes, so that the output always remains different. What's even crazier, however, is that the hashing algorithm used and the salt used is right there for you in plain sight. The ```$1``` indicates the MD5 algorithm being used, and the salt ```$e2pS0Sts``` is used. Despite this, this is completely fine to store on a password database. This follows Kerckhoff's principle, which states that any cryptographic system should remain secure, even if everything but the secret key is public knowledge. 

Now let's obtain a powerful wordlist, rockyou.txt. Rockyou.txt contains millions of common passwords and is popularly used in CTFs and password cracking challenges. To obtain it, clone the repo containing the file by running ```git clone https://github.com/dw0rsec/rockyou.txt.git```. Unzip the .txt by navigating to the directory and use the unzip command.

Once you have obtained the wordlist, let's get cracking! Run ```john --wordlist=rockyou.txt/rockyou.txt hash.txt.```. This initiates John the Ripper, tells it what wordlist to use, and what the target hash is.

<img width="742" height="217" alt="image" src="https://github.com/user-attachments/assets/0ed7a452-5ae2-48c8-8b7d-b61f6210e841" />

Nice! John took less than a second to crack the password, but that's because we had a very reliable wordlist, paired with a hash that was created with an old, weaker algorithm. However, the virtue remains the same. If you have many passwords on the hash.txt file, you can check to see which ones John cracked by running ```john --show hash.txt```. This will list the cracked passwords.

<img width="283" height="86" alt="image" src="https://github.com/user-attachments/assets/96f18c8e-49c3-4a1b-be83-665d5352c707" />

So that's that. You have successfully created a hash using OpenSSL, obtained a powerful wordlist, and used John the Ripper to crack a password against the wordlist. Nicely done. Obviously, this shows that algorithms such as MD5 are especially weak as it took less than a second to crack my password, but thankfully it isn't used by organizations today to store passwords. In today's day and age, there are far more intensive algorithms out there that create password hashes that require exhaustive hardware use to intentionally slow down password cracking tools like John. These are far more secure and better suited for hosting passwords on the database. That is for another time, however. With that, congratulations on learning how to use John the Ripper.
