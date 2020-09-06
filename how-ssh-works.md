# How does ssh connection happen? how can you say it is secure?

this stackoverflow Answer briefs how to setup and how the connection happen 

[The stack overflow question](https://serverfault.com/questions/935666/ssh-authentication-sequence-and-key-files-explain)

same image of the answer: 
![](https://i.stack.imgur.com/4cZbh.png)


# digital oceans indepth answer [LINK ](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process#)


1. diffie hellman algo is being used to generate a symmetric key first
2. both server and client contribute to creation of such key 

3. pic from wikepedia for diffie hellman 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/46/Diffie-Hellman_Key_Exchange.svg/500px-Diffie-Hellman_Key_Exchange.svg.png)

4. MAC are used to check the authenticity of the messges themselves ( MAC is Message authentication code ) 

5.1. client prvovides its ID ( public key) 
  2. server checks if there is something like that in ~/authorized_keys 
  3. server generaters a random number and encrypt and send it to solve for client 
  4. if client has the private key , it will decrypt it and combine with the secret key ( generated as the symmetric one) and calculates MD5 of it and sends to server
  5. the server matches the value sent after decrypt the user is authenticated 
  
  
  TODO: // understand what exactly is shared during diffie hellman key exchange 
  
  A good place to check the working process is using -vvv
 
  ```
  ssh -vvv  myuser@localhost 
  ```

EXTRA:
https://www.thegeekyway.com/ultimate-guide-how-ssh-works/
