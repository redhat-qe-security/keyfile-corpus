Collection of files with keys encrypted in different formats and using
different encryption algorithms.

The names should be mostly self-explanatory, below are some more tricky parts:

 - salt - length of salt used for the algorithm, in bytes
 - iter - number of iterations that the PRF needs to be run, actual number
 - keyLen - for PBKDF2: length of the key to output
 - prf - for PBKDF2: pseudo-random function, default is hmacWithSHA1
 - bf-cbc - Blowfish cipher in CBC mode
 - IV - length of initialisation vector used, in bytes
 - none - when a specific entry in file does not use encryption, it's specified
   as none
 - pass - the parameter is the name of the password used for both integrity
   (MAC) and encryption
 - pass-mac - name of password used for integrity check
 - pass-cipher - name of password used for integrity check
