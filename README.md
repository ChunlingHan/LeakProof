# LeakProof

LeakProof is a tool for privacy-preserving machine learning. By introducing functional encryption to machine learning, the machine learning can learn over users' encrypted data and can only obtain statistic information of users' data, without any privacy violation. 

LeakProof can be widely used in many kinds of machine learning models. Because LeakProof provides a pre-process of encrypted data for machine learning models, and the learning, test and decision phrases will not be affected.

LeakProof has five algorithms, including setup, encryption, key request, key generation and decryption, which included in two modules, Private Key Algebra Fully Homomorphic Encryption and Obliv-C.

Users will encrypt their sensitive data into ciphertexts for the concern of privacy violation, they will encrypt their data by using LeakProof. The Encryption will be conducted in Private Key Algebra Fully Homomorphic Encryption module. The user uses PKAFHE to generate the public parameter PP and the secret key sk.

To use PKAFHE, A NTL library should be intalled firstly. (http://www.shoup.net/ntl/download.html)
To install NTL on Linux, the conmmands are as follows:
   % gunzip ntl-xxx.tar.gz
   
   % tar xf ntl-xxx.tar
   
   % cd ntl-xxx/src
   
   % ./configure 
   
   % make
   
   % make check
   
   % sudo make install
   
You can also follow the tutorial provided by NTL. (http://shoup.net/ntl/doc/tour-unix.html)

Users can run keyGen algorithm as this, in keygen file in PKAFHE, I generate 2048 bits primes, this setting can be adjusted according to the users' requirement.

   % g++ -g -O2 -std=c++11 -pthread -march=native keygen.cpp -o keygen -lntl -lgmp -lm
   % make keygen
   % ./keygen

After generating the secret key and the public parameter, users can encrypted their sensitive data by run enc in PKAFHE. 

   % g++ -g -O2 -std=c++11 -pthread -march=native enc.cpp -o enc -lntl -lgmp -lm
   % make enc
   % ./enc
  
At this point, the users have encrypted their data into ciphertexts, the ciphertexts will be stored in a file named by the user.
  
Then users can provide their encrypted data to a machine learning model, the machine learning model will evalute the ciphertexts using the evaluation algorithms provided by PKAFHE. For example, users' plaintexts are a,b,c, the machine learning model wants to obtain a*(b+c), then it can evalute on the ciphertexts to calculate the ciphertext of a*(b+c). The machine learning model can use the evaluation algorithms in PKAFHE including addition, subtraction, muliplication and division. 
  
Then the machine learning model will set the value obtained from evalution algorithms as functional key request and send it to the user,or a server on behalf of the user. Then the user and the machine learning model will act as the two parties in Obliv-C. 
  
Obliv-C is a secure two party computation protocol using yao's garbled circuits as backend.
  
You should first install Obliv-C and build it. 
The installation tutorial of obliv-C can be found in (https://github.com/samee/obliv-c/blob/obliv-c/README.md).
  
We designed a protocol named as modprime to run the key generation and decryption algorithms in LeakProof. The location of modprime is ：

  .../obliv-c/test/oblivc/modprime
  
Firstly, we should build modprime:
   % make
   
Then the machine learning model and the user as the two parties run the modprime protocol. We assume the machine learning model's input is c and the users' input is p (secret key) :

The machine learning model:
   % ./a.out 1234 -- c &
   
The user runs a.out as :
   % ./a.out 1234 localhost p
   
   The the machine learning model and the user can ontain the result c % p.
   
   Notice that, in modprime protocol, there is a pause, to wait the big number operation results printed on the screen completely.
   
   
  
  
  
  
  
  
  
  
  
