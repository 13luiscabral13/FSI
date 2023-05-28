**Trabalho Realizado na Semana #12**

**Lab Tasks**

**3.1 Task 1: Becoming a Certificate Authority (CA)**

For this task first we need to create a new openssl.cnf file to house all of the configurations taht we will need.

Then we need to create a root certificate for us, we can do that with the command:
* openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \ -keyout ca.key -out ca.crt 

Then we can onput our parameters and 2 files are created ca.crt ca.key

![](img/lab11/cakeyComand.png)
![](img/lab11/caC.png)
![](img/lab11/caPK.png)

To see the decoded contents of the files we just need to use the commands:
* openssl x509 -in ca.crt -text -noout
* openssl rsa -in ca.key -text -noout

![](img/lab11/caCde.png)
![](img/lab11/caPKde.png)

![](img/lab11/info.png)

Looking at the information in the last screenshot we can see that this is a CA certificate by the line "CA:TRUE", and we can see that the certificate is self signed because the key identifiers of the subject and the authority are the same.

The values of the public and private exponents are:
![](img/lab11/pExponent.png)

The value of the modulos is:
![](img/lab11/module.png)

And the values of the two secret numbers are:
![](img/lab11/primes.png)

**3.2 Task 2: Generating a Certificate Request for Your Web Server**

To generete a certificate for our web server we need to run the command;
* openssl req -newkey rsa:2048 -sha256 \-keyout server.key -out server.csr \-subj "/CN=www.bank32.com/O=Bank32 Inc./C=US" \-passout pass:dees 

This creates two files serfer.key and server.csr we can see the contetns of this files with the commands:
* openssl req -in server.csr -text -noout
* openssl rsa -in server.key -text -noout

![](img/lab11/serverC.png)
![](img/lab11/serverPK.png)

We can add alternative names adding
* openssl req -newkey rsa:2048 -sha256 \-keyout server.key -out server.csr \-subj "/CN=www.bank32.com/O=Bank32 Inc./C=US" \-passout pass:dees -addext "subjectAltName = DNS:www.bank32.com, \ DNS:www.bank32A.com, \ DNS:www.bank32B.com"
At the end of the command to create a certificate.

![](img/lab11/addNames.png)
![](img/lab11/seeNames.png)

**Task 3: Generating a Certificate for your server**

To generete a certificate for our server we need to run the following command:

* openssl ca -config openssl.cnf -policy policy_anything \
-md sha256 -days 3650 \
-in server.csr -out server.crt -batch \
-cert ca.crt -keyfile ca.key

![](img/lab11/createCert.png)

After this we can see that a new file server.crt was created.
To be able to see the contents of the file we need tochange our openssl.cnf file and uncomment this line:
* copy_extensions = copy

After this we can run:
* openssl x509 -in server.crt -text -noout
To see the contents of the file

![](img/lab11/servernCer.png)


**3.4 Task 4: Deploying Certificate in an Apache-Based HTTPS Website**

For this task we need to change the the dockerfile and the config file for site to the following

![](img/lab11/dockerfile.png)
![](img/lab11/config.png)

We also need to add the certificates for the site and the ca in the certs folder.

After this we need to enable the ssl module in the apache server with this two commands:

* a2enmod ssl 
* a2ensite bank32_apache_ssl

But after this we get a warning when we accesse the site

![](img/lab11/site.png)

To remove this we need to make our web browser trust the our CA we can do this by adding our certificate to the truted authorities of the browser.

![](img/lab11/site2.png)

**3.5 Task 5: Launching a Man-In-The-Middle Attack**

For this task first we need to change the config file of our server to change hte nameserver to our target site in this case www.reddit.com and add "10.9.0.80        www.reddit.com" to the hosts file in our machine to simulate a DNS poisening.

With this we suceffuly laucnhed a MIM attack although the browser warnes us that the site is not secure.

![](img/lab11/siteReddit.png)


**3.6 Task 6: Launching a Man-In-The-Middle Attack with a Compromised CA**

For this task we need to simulate that the CA was compromised so we can create certficates for our own malicious website. TO do this we just need to follow the steps in the tasks 2 and 3 to create a certificate and load that certificate in to the server.

After this we can browse the malicious website without the browser giving us any warning.

![](img/lab11/siteReddit2.png)


# CTF

**Semana 12 & 13 - Desafio 1**


Firstly, we analysed the code given to us in the file template.py.

![](https://i.imgur.com/waUzD0e.png)

We decided to create a function that would give us the first prime number after 2**512 and 2**513, using python.

![](https://i.imgur.com/9AInnW8.png)

Getting as value:
13407807929942597099574024998205846127479365820592393377723561443721764030073546976801874298166903427690031858186486050853753882811946569946433649006084171

![](https://i.imgur.com/HBfFzQc.png)

Getting as value: 26815615859885194199148049996411692254958731641184786755447122887443528060147093953603748596333806855380063716372972101707507765623893139892867298012168351


All left to do was substitute in the template.py file given to us the correct numbers 

![](https://i.imgur.com/x1hqNyb.png)

We decided to use the function `inverse` from sympy to get the modular inverse in an easier way



We substituted the enc_flag variable by the one outputted in the console (000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000078509a0f21b27deea9d502ca9fb305d14a6bcdf1c77d618534cc6b23c012718f7d410ad9d83361930ea9c51b8456c955b7f516258bf083d82d2cfe928b579de0b5da388ed2053b2355e7d01b22bd5c0c52ede09c0072b74fa40d8768cb0ee4b4fd77aa04418aaa06488d61f29099de6c4b1bf4346ab9f3f59d8e107bbe994fcb)
and, when we ran the program, the correct flag was outputted

![](https://i.imgur.com/9IWrbNa.png)

All left to do was submit it!


**Semana 12 & 13 - Desafio 2**

The second one was a harder challenge so, we needed to dig deep into researching it.
We came across an article discussing what they called the RSA - Common Modulus attack: https://infosecwriteups.com/rsa-attacks-common-modulus-7bdb34f331a5

So, we decided to update their code and to try to solve the challenge with something similar.

We ran `nc ctf-fsi.fe.up.pt` and got the following output:

![](https://i.imgur.com/Nv5Wsrk.jpg)

So, we designed our code, substituting each message by the outputted one:

![](https://i.imgur.com/YvackUp.png)

In order to invert the multiplication described in the article, we had to import gmpy2

We ran our code and got...

![](https://i.imgur.com/h9GStB6.png)

This... But this isn't a flag... We decided to try to convert the output into bytes using the python function `to_bytes` to check if we got something...

![](https://i.imgur.com/1FJr7Yu.png)

And there it is! By using the to_bytes function with the second attribute as 'big', we got, as the output, the flag in the end... We copied it and pasted it in the CTF Challenges platform and it was deemed... Correct!
