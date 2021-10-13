Project 1 (Cryptanalysis of a class of ciphers):

This cryptanalysis project consists of a software implementation of an algorithm that tries to decrypt an L-symbol challenge ciphertext computed using a specific cipher. Informally speaking, your program's goal is to find the plaintext used to compute this ciphertext within a reasonable amount of time. Specifically, your program should:

print on screen something like "Enter the ciphertext:",
obtain the ciphertext from stdin,
apply some cryptanalysis strategy and
output on screen something like "My plaintext guess is:" followed by the plaintext found by your strategy.
In its cryptanalysis strategy, your program is allowed access to:

The ciphertext (to be taken as input from stdin).
A plaintext dictionary (to be posted on top of this web page), containing a number q of plaintexts, each one obtained as a sequence of space-separated words from the English dictionary
Partial knowledge of the encryption algorithm used (to be described below).
In its cryptanalysis strategy, your program is not allowed access to:

The key used by the cipher.
Part of the encryption scheme (to be detailed below).
The plaintext is a space-separated sequence of words from the English dictionary; thus, each symbol is either a space or one of the 26 lower-case letters from the English alphabet and cannot be a special character, punctuation symbol or upper-case letter; the sentence may not be meaningful, for sake of simplicity.

The key is a sequence of t numbers between 0 and 26.  

The ciphertext looks like a sequence of symbols from {<space>,a,..,z}.

A text file plaintext_dictionary_test1 containing a number u of L-symbol candidate plaintexts will be provided to you (as an attachment at the top of this page), and you should feel free to use its content as part of your code.

A text file plaintext_dictionary_test2 containing a number v of English words will be provided to you (as an attachment at the top of this page), and you should feel free to use its content as part of your code.

Your program will be run using different parameters (e.g., L=500, u=5, v=40, and t between 1 and 24), and on a number of challenge ciphertexts, each computed using a potentially different variant of the encryption scheme.

Your program should return as output a guess for which L-symbol plaintext was encrypted.

Each ciphertext will be computed from a plaintext selected in one of the following two ways:

randomly and independently choosing one of the L-symbol plaintexts in Dictionary1 or
concatenating words randomly and independently chosen from Dictionary2 (any two words being separated by a space, until one has an L-symbol plaintext).
All the encryption schemes used have the following common features:

The message space is the set {<space>,a,..,z}^L. In other words the message m can be written as m[1],...,m[L], where each m[i] is in {<space>,a,..,z}.
The ciphertext space is the set {<space>,a,..,z}^{L+r}, for some r>=0. In other words the the ciphertext c can be written as c[1],...,c[L+r], where each c[i] is in {<space>,a,..,z}.
The key space is the set {0,..,26}^t. In other words the key k can be written as k[1],...,k[t], where each k[j] is in {0,..,26}, for j=1,..,t.
The encryption algorithm computes the next ciphertext symbol c[i] from the next message symbol m[i'] in two possible ways:
as a random value in {<space>,a,..,z}, the idea being that of including a random value in the ciphertext to confuse the eavesdropper
as the (lexicographic) shift of m[i'] by k[j(i')] positions, where the computation of each j(i') is left unspecified and may depend on i',t,L. In other words, each ciphertext symbol c[i] is the shift of the plaintext symbol m[i'] by a number of position equal to one of the key symbols, which symbol being chosen according to an undisclosed, and not key-based, "key character scheduling" algorithm that is a function of i', t and L. Note that i (the ciphertext position pointer) and i' (the message position pointer) will be more and more different as more random ciphertext symbols are inserted in the ciphertext. 
which of these two ways happens is left unspecified and and may depend on i',t,L. In other words, whether the ciphertext symbol is a shift of plaintext symbol or just a random value is being chosen according to an undisclosed, and not key-based, "coin generation" algorithm that is a function of i', t and L, and returns a coin value in [0,1] which is compared with a pre-set parameter prob_of_random_ciphertext
A pseudocode description of the encryption algorithm could go as follows:
Input: key k=k[1],...,k[t] and message m=m[1],...,m[L]
Instructions:
ciphertext_pointer = 1 
message_pointer = 1
num_rand_characters = 0
prob_of_random_ciphertext = 0.05
Repeat
let coin_value = coin_generation_algorithm(ciphertext_pointer,t,L)  // coin_value is a real number in [0,1]
if prob_of_random_ciphertext < coin_value <= 1 then
set j = key_character_scheduling_algorithm(message_pointer,t,L)
set c[ciphertext_pointer] = character obtained after forward shifting m[message_pointer] by k[j] positions on the cycle {<space>,a,..,z}
message_pointer = message_pointer + 1
if 0 <= coin_value <= prob_of_random_ciphertext then
randomly choose a character c from {<space>,a,..,z}
set c[ciphertext_pointer] = c
num_rand_characters = num_rand_characters + 1
ciphertext_pointer = ciphertext_pointer +1   
Until ciphertext_pointer > L + num_rand_characters
Return c[1]...c[L + num_rand_characters]

Note that, for instance, the poly-alphabetic substitution cipher is a particular example of this cipher where the key character scheduling computes “j(i') = (i' mod t) + 1” and where no random ciphertext symbols are used.

Your program will be scored based on two tests.

In the first test, your program will be run a few times (e.g., 10), each time on a new ciphertext, computed using the above encryption scheme and a plaintext randomly chosen from the plaintext_dictionary_test1, and a potentially different choice of the prob_of_random_ciphertext, the key character scheduling algorithm and the coin generation algorithm. In this test we will likely choose L=500, and a plaintext dictionary with u=5 plaintexts.

In the second test, your program will be run a few times (e.g., 10), each time on a new ciphertext, computed using a plaintext obtained as a space-separate sequence of words that are randomly chosen from a subset of the set of all English words (specifically, a few words randomly taken from word_dictionary_test2) and the above encryption scheme, and a potentially different choice of the prob_of_random_ciphertext, the key character scheduling algorithm and the coin generation algorithm. In this test we will likely choose L=500 and v=40 words.

To test your strategy, you may use a random key character scheduling algorithm (i.e., returning a random value between 1 and t), a random coin generation algorithm (i.e., returning a random value in [0,1]) and increasing values for prob_of_random_ciphertext (e.g., 0.05, 0.10, 0.15, etc).

Your executable file should be named "<last name1>-<last name2>-<last name3>-decrypt" (assuming a 3-person team here). Upon execution, it should obtain the ciphertext from stdin, and finally return the output plaintext on stdout within x minutes (or else it will be declared to default to an incorrect guess); most likely, we will choose x = 1 on test 1 and x = 3 on test 2.

If you want to propose more than one cryptanalysis approach, you need to clarify that in your report, and each of your approaches will be tested. You cannot pick more than one approach per team member. Your overall team score will be an average across the success of the various approaches.
