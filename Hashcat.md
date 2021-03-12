#Hashcat 

Presented by Fortunate Eze, Xinying Zhou & Kshitija Patel
&nbsp;


##Part 1: Background
###What is Hashcat?
Hashcat is a password recovery tool which was released in 2009. It provides services for Linux, Windows and Mac platforms. With Hashcat, we can easily crack some weak passwords encrypted using hash algorithms such as MD5, SHA, etc. 

However, some other excellent password crack softwares like John the Ripper (released in 1996) can support many hashing algorithms as well. Why people need Hashcat? Let's see what hashcat can do. 
###What Hashcat can do?
Hashcat is an awesome password crack tool which allows us to use GPU to speed up the cracking process[1]. Due to some processes can be accelerated by GPU, many weak passwords can be cracked in a shorter time[2]. How does Hashcat do after the simple commands? There are some knowledges we should know about the Hashcat.

---


##Part 2: Some knowledges behind the Hashcat
###Hashing Algorithms
Among all the backend supports by the hashcat, the first interesting thing you may want to know is what is hash? Or how can the hashcat "see" my password? These questions lead us to the hashing algorithms field. 
####What is hash?  
Hash is a mapping function. For passwords of an indefinite length that you enter, the hash function can map your password to a fixed-length hash. This is what exactely a hashing algorithm like MD5, SHA can do.

Using these hashing algorithms, our passwords will be encrypted and more difficult to break than simply storing the plain passwords on your computer. 

Here are some common hashing algorithms:
* MD4
* MD5
* SHA-512

####How to crack the password?
Knowing the basic idea about the hash algorithms, let's move on the next question. My password has already stored in a complex version that even I can't read. How can hashcat "read" it?

In Hashcat, there are two main ways to crack a hash: 
* Brute-Force Attack 
* Straight Attack

a)Brute-Force Attack attempts to guess the password with all possible combinations[3]. If you have a 6-digital password, the work that brute force attack does is to try all the possible passwords from 000000 to 999999. Your password must be included in this range(000000-999999).
![alt text](https://raw.githubusercontent.com/CincChou/Markdown-Photos-For-Hacking/main/brute_force_attack.jpg )

b)Straight Attack is also known as dictionary attack. The main idea of dictionary attack is using a wordlist file which includes many common used passwords. We should know that many hashing algorithms now are un-convertable.So, it's hard for us to directly reverse the hash through some methods. And how to find the correct password from the wordlist? We should convert the wordlist into hash using the same hashing algorithm as the target hash. Next, we can compare the hash values of the words in the dictionary with the target hash values in turn, and finally get the plain text password of the target hash. 

![alt text](https://raw.githubusercontent.com/CincChou/Markdown-Photos-For-Hacking/main/dictionary_attack.jpg)



###Attack modes(-a)
There are some modes for users to choose:
* 0 = Straight
* 1 = Combination
* 2 = Toggle-Case
* 3 = Brute-force
* 4 = Permutation
* 5 = Table-Lookup




---


##Part 3: Demos to crack password
###1. Wordlist attack (-a 0)
Wordlist attack(Dictionary attack) is a simple method of password attack. The main targets of wordlist attacks are those who are accustomed to using very common passwords. By encrypting the plaintext password in the wordlist with a specific type of hash algorithm, hashcat has the possibility to obtain the password by comparing it with the original hash value.

**a)Wordlist attack without rule**

Two files are required in this mode. One is the hash file which stores the hash you want to crack and the other is a wordlist file which stores many common used password.

For this demo, we copied the hash from shadow file:

![alt text](https://raw.githubusercontent.com/CincChou/Markdown-Photos-For-Hacking/main/linux_hash.jpg)

Then, we can use the attack command to crack the hash:

```
hashcat -a 0 -m 1800 hashes.txt wordlist.txt
```

Here is the result:

![alt text](https://raw.githubusercontent.com/CincChou/Markdown-Photos-For-Hacking/main/dic_attack_res1.jpg)

**b)Wordlist attack with rule**

###2. Brute-force attack(-a 3)
The brute force is a method of trying all possible password combinations by using the high-speed computing power of the computer. In other words, the longer the passwords set by the user, the more complex the composition of the passwords, and the lower the possibility of brute-force cracking.

In this demo, we used 5 MD5 hashes as the password:

![alt text](https://raw.githubusercontent.com/CincChou/Markdown-Photos-For-Hacking/main/hashes1.jpg)

After storing the hash, we can use the following command to guess the password from 1 character to 7 characters:
```
#-i is the increment option 
#will try the password from 1 character to the length you choose
hashcat -a 3 -m 0 hashes1.txt -i ?a?a?a?a?a?a?a
```


###3. Combination attack(-a )

###4. Office word file attack(-a 3)
Locking documents is a way for office to secure files. Hashcat also have the ability to crack locked documents.

Since hashcode is the target file for hashcat, the first step we should do is to find the hashcode of the locked documents.
* use wget to download a python file office2john.py. This python file can extract the hash of the locked document.
```
wget https://raw.githubusercontent.com/openwall/john/bleeding-jumbo/run/office2john.py
```
* grant -x permission to the python file so that we can execute it.
```
chmod +x office2john.py
```
* execute the office2john file to extract the hash and store it in hashes.txt.
```
./office2john.py Hashcat.docx > hashes.txt
```
The content of hashes.txt file is like this:
```
$office$*2013*100000*256*16*220071b7e428e8046dab68fcf3a82b15*1378f615c50b3f89777b0d1eb80437b6*107cc478ac1741d119462f0a0afa774786e33b99fb52b222836f1839e9d91390 
```

Then, we can use hashcat to crack it! 
Assuming that we know that the document password is a combination of **6 digits**, the hashcat code using the brute force is as follows: 
```
hashcat -a 3 -m 9600 hashes.txt --force ?d?d?d?d?d?d

```
The hashcat result is as follows:

![alt text](https://github.com/CincChou/Markdown-Photos-For-Hacking/raw/main/office_document_res.jpg )

---


##Part 4: Some lessons we can learn 
donot use weak password....



---


##References
- [1] [Hashcat](https://hashcat.net/forum/thread-5559.html) Hashcat. Hashcat project. 29 June 2016.
- [2] [Developments in Password Cracking](https://www.schneier.com/blog/archives/2012/09/recent_developm_1.html) Passwords. Bruce Schneier. 19 September 2012.
- [3] [Brute Force Attack](https://www.kaspersky.com/resource-center/definitions/brute-force-attack) Brute-Force-Attack. Kaspersky. 


