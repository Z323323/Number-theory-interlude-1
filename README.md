# Number theory interlude

<p>
  This article collapses some theorems and subjects necessary to complete the cryptography course at [https://crypto.stanford.edu/pbc/notes/numbertheory/].
</p>

## Wilson's theorem
<p>
The theorem states that for any integer $p > 1$ we have $(p - 1)! = - 1 \mod p$ if and only if p is prime. To understand this lightning fast just think about this: every number from $1$ to $p - 1 (\mod p)$ has a multiplicative modular inverse and that inverse is unique for that number with two exceptions: $1$ and $p - 1$ whose inverses are themselves (indeed they are always roots of unity). Then why multiplying them makes the rule to hold? Simply because $1(p - 1) = p - 1 = - 1 (\mod p)$. One of the 'hard' things to prove here is the uniqueness and existence of multiplicative modular inverses from $1$ to $p - 1$ which I proved in the final section of [https://github.com/Z323323/From-Fermat-to-the-group-theory]. Now let's understand why this formula is enough to prove that $p$ is prime. If $p$ wasn't prime it should mean that $p$ is composed by $2$ or more primes multiplied, then from our reasoning about roots of unity the formula wouldn't produce $- 1$ as result, because of roots of unity which are the inverses of themselves, and because $Z_{n}^{*}$ do not have inverses for every element, thus their multiplication wouldn't produce $1$. All these motivations are not enough to prove that the theorem always work, but I'm not writing a proof for this. Also note that $Z_{n}^{*}$ is not a multiplicative group because the existence of inverses for every element is a requirement for the definition of multiplicative group. Indeed when papers refer to $Z_{n}^{*}$ they refer to $Z_{\phi(n)}^{*}$. <br>
  
$x \mod 4$<br>
-><br>
$1$ is a root of unity.<br>
$2$ is not a multiplicative modular inverse nor a root of unity.<br>
$3$ is a root of unity.<br>

$x \mod 15$<br>
-><br>
$1$ is a root of unity.<br>
$2$ and $8$ are multiplicative modular inverses.<br>
$3$ is neither a root nor an inverse.<br>
$4$ is a root of unity.<br>
$5$ is neither a root nor an inverse.<br>
$6$ is neither a root nor an inverse (it has 3 in it).<br>
$7$ and $13$ are multiplicative modular inverses.<br>
$9$ is neither a root nor an inverse (it has 3 in it).<br>
$10$ is neither a root nor an inverse (it has 5 in it).<br>
$11$ is a root of unity.<br>
$12$ is neither a root nor an inverse (it has 3 in it).<br>
$14$ is a root of unity.<br>

R----I----X----R----X----X----I----I----X----X----R----X----I----R<br>

Where R = root, I = inverse, X = neither one of them.

It's clear that whenever there are non-coprimes in the set, there are elements which are not roots nor inverses then it's resonable to assert that Wilson's theorem holds. Also, if we remove all the non-coprimes-with-modulo elements, our group will become very similar to $Z_{p}^{*}$ where $p$ is prime. Now, the problem about the Wilson's Theorem is the 'only if' statement inside the theorem which should be proved but I'm moving forward assuming it's true.<br>

Before moving forward let's see another example with a prime modulo:<br>

$x \mod 17$<br>
-><br>
$1$ is a root of unity.<br>
$2$ and $9$ are multiplicative modular inverses.<br>
$3$ and $6$ are multiplicative modular inverses.<br>
$4$ and $13$ are multiplicative modular inverses.<br>
$5$ and $7$ are multiplicative modular inverses.<br>
$8$ and $15$ are multiplicative modular inverses.<br>
$10$ and $12$ are multiplicative modular inverses.<br>
$11$ and $14$ are multiplicative modular inverses.<br>
$16$ is a root of unity.<br>

R----I----I----I----I----I----I----I----I----I----I----I----I----I----I----R<br>
</p>

## Units

<p>
  If $z \in Z_{n}$ has an inverse then $z$ is a unit. The set of units is denoted by $Z_{\phi(n)}^{*}$ (papers refer to it as $Z_{n}^{*}$). <br>
  From the previous section is clear that $z$ is a unit if and only if $z$ and $n$ are coprime.
</p>

## Modular exp. and DLP

<p>
  These are well covered in [https://crypto.stanford.edu/pbc/notes/numbertheory/exp.html] so I won't delve them. The absence of a structure of the powers of $x \mod p$ is quite magical and it's one of the foundations of cryptography 
  (even if the actual process which destroy the structure is combined with elliptic curves into PLONK).
</p>

## Nonunits

<p> 
  From the previous example is clear that we will always face nonunits every time we deal with some $\mod n$ where $n$ is not prime. It's also clear that if $n = p^{k}$ for some prime $p$, we will face non-units for every $dp \mod n$ 
  term (for any $d \geq 1$).<br> 
  When we deal with multiplicative groups, we don't normally care a lot about non-units, since every reason we seen in the former sections and everything said into [https://github.com/Z323323/Group-theory-elements]. Nonetheless it's important to consider them because they exist.
</p>

## Fermat's Little Theorem and Euler's Theorem
<p>
  Check [https://github.com/Z323323/From-Fermat-to-the-group-theory]. It contains every necessary proof of both theorems.
</p>

## RSA

<p>
  
  First, consider this [https://crypto.stanford.edu/pbc/notes/numbertheory/order.html].<br>
  I give you $3$ more tips:<br>
  1. to better understand why $ed = k(\phi(N)) + 1$ -> $d = e^{-1} \mod \phi(N)$, just note that the first formula means that $ed$ divided by $\phi(N)$ must be $k + 1$, which in turn means that $ed \equiv 1 \mod \phi(N)$, and the second formula follows after this (I know everything is quite magical in this field).
  2. $a$ (which is our message to encrypt) must be coprime with $N$, indeed this algorithm is only used to share a second key (which will be coprime with $N$), but this generation will not be performed by the owner of the secret key; instead the $2nd$ peer will use the public key to encrypt his AES key, and send it to the owner of the secret key, which will decrypt this second key and use it to start the exchange of encrypted session using AES. <br>
  3. the whole computational problem introduced by the algorithm is to find $e^{-1} \mod \phi(N)$ where $\\{e, N\\}$ is our public key (we can't multiply the message $a$ by $e$ because $e$ is calculated $\mod \phi(N)$ not $\mod N$, hence the inverses wouldn't match $\mod N$ thus not giving back the message $a$).

Since everything said, the problem of breaking RSA is to find $\phi(N)$, which reduces the problem to fast factorization, which is too much computationally heavy to be performed in a reasonable amount of time. After factoring $\phi(N)$ we could proceed to calculate $e^{-1} \mod \phi(N)$ using the EEA.
     
</p>

## Primality tests

### Fermat test
<p>
  This is the most intuitive while probably the worst primality test.
  
  By Fermat's Little Theorem:
  
  $a^{p - 1} \equiv 1 \mod p$

  Hence taking a random integer $n$ (the number which we want to prove the primality) and a random $a < n$ if:

  $a^{n - 1} \equiv 1 \mod n$

  is verified, should we conclude that $n$ is prime? Well, it's not always the case, hence the answer is 'maybe'.<br>
  Let's fire a counter-example to show that it's not always the case: 
  
  $561 = 3 * 11 * 17$

  hence it's clearly not a prime. But:

  $a^{560} \equiv 1 \mod 561$

  is verified by every $a$ which is coprime with $561$ (and therefore with $3, 11, 17$). To understand why is that, let's consider the CRT, by the CRT all the solutions of this system will be mapped on $Z_{561}^{*}$:
  
  $x^{2} \equiv 1 \mod 3$<br>
  $x^{10} \equiv 1 \mod 11$<br>
  $x^{16} \equiv 1 \mod 17$

  Now, since these congruences are exactly wrote to match the Fermat's Little Theorem, there will be $2 * 10 * 16$ solutions mapped into $Z_{561}^{*}$, and all these solutions (which are the coprimes of $561$) will produce $1$ at:

  $x^{560} \equiv 1 \mod 561$

  fooling this test. Now the actual reason why these $xs$ will produce $1$ at the $560$ degree is not because they are the solutions of the CRT system, but is because $560$ is divisible by $2, 10, 16$, hence the multiples of those numbers will always produce $1$ (using the same bases which are mapped by the CRT) because of the cyclicness of subgroups (proved into [https://github.com/Z323323/Group-theory-elements]). Now let's see why is not always the case, and therefore why $561$ is somehow a 'special' number.

  $x^{2} \equiv 1 \mod 3$<br>
  $x^{6} \equiv 1 \mod 7$<br>
  $\equiv ?$<br>
  $x^{20} \equiv 1 \mod 21$

  Remember that applying the CRT to the first two congruences, means to find the number of solutions (the numbers) $\mod 21$ which satisfy those two congruences, i.e. to find the numbers which produce $1$ being elevated both at the $2nd$ power and at the $6th$ power. Now $20$ is not divisible by $6$, which means that the solutions of 

  $x^{20} \equiv 1 \mod 21$

  do NOT map the solutions of the first two congruences. Indeed there are $4$ solutions to this last congruence where $2$ of them are obviously $1$ and $-1$. While the solutions of the CRT are are $12$.<br>

  To conclude, there exist some numbers called the 'Carmichael numbers' like $561$ which produce $1$ for a lot of $xs$, hence they almost always fool the test. But even without considering Carmichael numbers we can't be $~100%$ sure that our $n$ will be prime using this test.

### Miller-Rabin test

every odd number $n$ (primes too), $n - 1$ is representable as:

$n - 1 = 2^{S}q$

where $q$ is an odd number. This is actually a very fast process, i.e.:

$n$ -> $n - 1$ -> $q_{?} = \frac{n - 1}{2}$

Now if $q_{?}$ is even we can reiterate $q_{?} = \frac{n - 1}{2}$ and find $S$. 

  

  








  
</p>

 
