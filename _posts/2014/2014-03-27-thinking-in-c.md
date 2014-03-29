---
---

 need some explaination how this specific line works. I know that this function counts the number of 1's bits, but how exactly this line clears the rightmost 1 bit?

int f(int n) {
    int c;
    for (c = 0; n != 0; ++c) 
        n = n & (n - 1);
    return c;
}
Can some explain it to me briefly or give some "proof"?

Thanks in advance, BBLN.

Any number 'n' will have the following last k digits: One followed by (k-1) zeroes: 100...0 Note that k can be 1 in which case there are no zeroes.

(n - 1) will be of this format: Zero followed by (k-1) 1's: 011...1

n & (n-1) will therefore be 'k' zeroes: 100...0 & 011...1 = 000...0

Hence n & (n - 1) will eliminate the rightmost '1'. Each iteration of this will basically remove the rightmost '1' digit and hence you can count the number of 1's.



---


up vote
17
down vote
accepted
It's figuring out if n is either 0 or an exact power of two.

It works because a binary power of two is of the form 1000...000 and subtracting one will give you 111...111. Then, when you AND those together, you get zero, such as with:

   1000 0000 0000 0000
&&  111 1111 1111 1111
   ==== ==== ==== ====
 = 0000 0000 0000 0000
Any non-power-of-two input value will not give you zero when you perform that operation.

For example, let's try all the 3-bit combinations:

      <----- binary ---->
 n     n    n-1   n&(n-1)
---   ---   ---   -------
 0    000   111     000 *
 1    001   000     000 *
 2    010   001     000 *
 3    011   010     010
 4    100   011     000 *
 5    101   100     100
 6    110   101     100
 7    111   110     110
You can see that only 0 and the powers of two (1, 2 and 4) result in a 000/false bit pattern, all others are non-zero or true.

See the full Bit Twiddling Hacks document for all sorts of other wonderful (or dastardly, depending on your viewpoint) hackery.

