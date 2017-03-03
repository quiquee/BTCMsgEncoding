# BTCMsgEncoding
Research for sending msgs over BTC network using well known lists of words

The initial list is taken from https://github.com/first20hours/google-10000-english

Being N the number of words in a list, we need to allocate at least n bits so that 

      2^n > N  =>  n > log(2) N , being n integer
      
 It is easy to put this as n = int ( 0.5 + log(2) N ) 
 i.e. for 10.000 words , we need to allocate 14 bits to refer to a word:
 
      14 =  int ( 0.5 + log(2) 10000 )
      
Bitcoin public addresses are 160bit long. We have to allocate first bits to signal the length of n , so we can use lists of different size. n will be between 8 and 16 so we need 4 bits for this. Lets reserve another 4 to signal the language. This makes 8 bits in total that we cannot use , this leaves 152 bits available. We will be able to refer int (152 / 14) = 10.8 words with a bitcoin address using a 10000 word list

This assumes there is no number / url or alike in the list

What can you say with 10 words:

    10 Words: I am late tonight because I have a work event
    10 Words: Please heat yourself the pizza it is in the oven
    10 Words: I love you darling please dont be mad at me

So this looks like a reasonable payload for the cost.

Lets realise as well that if we use the most common words, we should be able to compress the message. It is likely that the real address range needed is below 14, which will give us the opportunity to fit more words.

This is what we will try to demonstrate using a simple program that encodes on the fly a sentence to a base58 bitcoin address. This conversion will be called BTC Message Encoding or BTCME ;)

