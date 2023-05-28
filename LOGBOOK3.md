
# Trabalho realizado na Semana #3

**Identificação: descrição geral da vulnerabilidade, incluindo aplicações/sistemas operativos relevantes (max 4 itens com 20 palavras cada)**

* A vulnerability in OpenSSL allows a remote attacker to expose sensitive data through incorrect memory handling in TLS heartbeat extension.

* Compromises the secret keys used to identify the service providers and to encrypt the traffic, the names and passwords of the users and the actual content. 

* It could leak user authentication credentials and secret keys, allowing the attacker to access the system and users' information.

* Some of the affected applications are:
    * Apache and nginx 
    * SMTP, POP and IMAP protocols
    * XMPP protocol
    * SSL VPNs

**Catalogação: o que se sabe sobre o seu reporting, quem, quando, como, bug-bounty, nível de gravidade, etc.**

* Neel Mehta of Google's security team privately reported Heartbleed to the OpenSSL team on 01/04/2014 11:09 UTC.

* Codenomicon discovered Heartbleed two days later, while testing its own software with Safeguard, accelerating the patching of the bug.

* The severity level of this vulnerabilty is 7.5, corresponding to a high risk.

* Although OpenSSL Software Foundation has no bug-bounty program, the Internet Bug Bounty initiative awarded Mehta US$15,000 for his responsible disclosure.


**Exploit: descrever que tipo de exploit é conhecido e que tipo de automação existe, e.g., no Metasploit**

* OpenSSLs allocate a memory buffer for the returned message based on the request's length field. 

* This disregards the request message's payload's size causing the information in the allocated memory buffer to show up.

* By sending requests with small payload and large length field, attackers read up to 64kB of memory used by OpenSSL

* The problem can be fixed by ignoring Heartbeat Request messages that ask for more data than their payload need.



**Ataques: descrever relatos de utilização desta vulnerabilidade para ataques bem sucedidos e/ou potencial para causar danos**

* Some attackers may have exploited the flaw for at least five months before discovery.

* The NSA may have been aware of the flaw since its appearance keeping it secret in order to exploit it.

* An attacker hijacked active user sessions, via requests to the HTTPS server running on a VPN device, obtaining session tokens for authenticated users.

* The Heartbleed vulnerability enabled hackers to steal security keys from Community Health Systems compromising the confidentiality of 4.500.000 patient records.
