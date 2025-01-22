# Number theory interlude 1

<p>
  This article collapses some theorems and subjects necessary to complete the cryptography course at [https://crypto.stanford.edu/pbc/notes/numbertheory/].
</p>

## Wilson's theorem
<p>
The theorem states that for any integer $p > 1$ we have $(p - 1)! = - 1 \mod p$ if and only if $p$ is prime. To understand quickly just think about this: every number from $1$ to $p - 1 (\mod p)$ has a multiplicative modular inverse and that inverse is unique for that number with two exceptions: $1$ and $p - 1$ whose inverses are themselves (indeed they are always roots of unity). Then why multiplying them makes the rule to hold? Simply because $1(p - 1) = p - 1 = - 1 (\mod p)$. One of the 'hard' things to prove here is the uniqueness and existence of multiplicative modular inverses from $1$ to $p - 1$ which I proved in the final section of [https://github.com/Z323323/From-Fermat-to-the-group-theory]. Now let's understand why this formula is enough to prove that $p$ is prime. If $p$ wasn't prime there would necessarily exist some numbers which are not coprime, hence by the same reasoning made in the first part of [https://github.com/Z323323/Group-theory-elements] we would end up having a result $\mod n$ ($n$ non-prime) which could not be $1(p - 1)$. To get this quickly just note that the non-coprimes will produce a number which is not coprime with $n$ hence $product \not\equiv 1, - 1 \mod n$. Also note that $Z_{n}^{*}$ is not a multiplicative group because the existence of inverses for every element is a requirement for the definition of multiplicative group. Indeed when papers refer to $Z_{n}^{*}$ they refer to $Z_{\phi(n)}^{*}$, or better, when I refer to $Z_{\phi(n)}^{*}$ I refer to $Z_{n}^{*}$ because of everything said into [https://github.com/Z323323/Group-theory-elements]. Also we can't use this theorem as primality test because computing $(p - 1)! = - 1 \mod p$ is too computationally heavy.

### Some ex.s
  
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
  When we deal with multiplicative groups, we don't normally care a lot about non-units, since every reasoning made in the former sections and everything said into [https://github.com/Z323323/Group-theory-elements]. Nonetheless it's important to consider them because they exist (and they are a problem).
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

## Fermat's primality test

<p>
  To fully understand this section it's preferable that you have read [https://github.com/Z323323/Group-theory-elements].
  
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

  fooling this test. Now the actual reason why these $xs$ will produce $1$ at the $560$ degree is not because they are the solutions of the CRT system, but is because $560$ is divisible by $2, 10, 16$, hence the multiples of those exponents will always produce $1$ (using the same bases which are mapped by the CRT) because of the cyclicness of subgroups (proved into [https://github.com/Z323323/Group-theory-elements]). Now let's see why is not always the case, and therefore why $561$ is somehow a 'special' number.

  $x^{2} \equiv 1 \mod 3$<br>
  $x^{6} \equiv 1 \mod 7$<br>
  $\not\equiv$<br>
  $x^{20} \equiv 1 \mod 21$

  Remember that applying the CRT to the first two congruences, means to find the number of solutions (the numbers) $\mod 21$ which satisfy those two congruences, i.e. to find the numbers which produce $1$ being elevated both at the $2nd$ power and at the $6t\lambda$ power (and multiples of them). Now $20$ is not divisible by $6$, which means that the solutions of 

  $x^{20} \equiv 1 \mod 21$

  do NOT map the solutions of the first two congruences. Indeed there are only $4$ solutions to this last congruence where $2$ of them are obviously $1$ and $-1$, while the solutions of the system using the CRT are are $12$.<br>

  To conclude, there exist some numbers called the 'Carmichael numbers' like $561$ where every $x$ which is coprime with $n$ ($n$ is a Carmichael number) produce $1$ being elevated at $p - 1$, hence these end up fooling the test. Non-units (aka non-coprimes) won't follow the same behaviour, but the point is that every $x$ of the previous groups generating $Z_{561}^{\ast}$ will be solutions for $x^{560} \equiv 1 \mod 561$. You may be asking yourself where the remaining $560 - 3 \times 11 \times 17$ are. Well the fact is that $x \equiv 0$ does exist ;).

</p>

## Carmichael numbers

<p>
  Refer to [https://crypto.stanford.edu/pbc/notes/numbertheory/carmichael.html].

  The linked resource is quite self-explainatory, but I'll provide a couple clarifications since it's not simple. The whole first section aims to prove that Carmichael numbers must be composed by at least $3$ distinct primes not squared, and the proof comes along with probabilities of (different formed) $n$ to fool the Fermat test. Remember that if we choose a random number $a \mod n$ which fools the Fermat test doesn't necessarily mean that $n$ is a Carmichael number, that is, we need every possible random $a$ [coprime with $n$] to fool the Fermat test in order to define $n$ as Carmichael number, while it's true that every Carmichael number fools the Fermat test, thus we must consider this condition first and then derive our conclusions. Initially Ben proves they can't be $2$ primes multiplied. The whole reasoning about $d$ is because we know that taken a random $a$ (in order to match Carmichael numbers property [remember that $n$ is the number tested not $a$]), it will need to be such that $a^{q - 1} \equiv 1 \mod p$ (clarified later). This means that $q - 1$ will need to be such that $q - 1 | p - 1$ because in order to have $1$ as residue, $a$ will be a generator for a subgroup of $Z_{p}^{\ast}$ of order $o$ such that $o | q - 1$ (it turns out that $o = q - 1$ under the best case) and $o | p - 1$ (everything follows from Lagrange's Theorem). This in turn means that if we consider the $gcd$ we can set a 'best case scenario' because the number of solutions will be the highest possible for the $gcd$, and
  
  $gcd(q - 1, p - 1) = d$<br>
  $->$<br>
  $a^{d} \equiv 1 \mod p$

  Following the 'best case scenario' reasoning, I have to say that I think that if we follow the $gcd$ reasoning this could be a little bit misleading since it's not enough to only consider the $gcd$, we will need $gcd(p - 1, q - 1) = q - 1$ (clarified later). This will mean that since $d$ can be at most $d = (p - 1)/2 (= q - 1)$, because $p > q$, we will have at most only half of the solutions which fool the Fermat test (and the other half will be spotted). To better understand this, just think at the CRT constructions. The result about half of the solutions is really a 'best case scenario' but it could be easily considered a fantasy one, indeed in general there will be many fewer solutions fooling the Fermat test (remember it could be easily the case that $q - 1 \nmid p - 1$). This whole reasoning proves that even under the best case scenario, $n$ can't be a Carmichael number, hence $n$ must be of a different form. Now before going over, let's clarify the process that brought us to $a^{q - 1} \equiv 1 \mod p$, because understanding it will help us understand everything better. We have that (for $n = qp$)

  - $n - 1 = q(p - 1) + q - 1$
  - $n - 1 = p(q - 1) + p - 1$
  
  Every congruence below will need to be verified in order to fool the Fermat test. Remember this doesn't mean that $n$ could be a Carmichael number. In order to be a Carmichael number, we will need every possible $a$ (coprime with $qp$) to fool the test, which means that after solving the system below, we will need to end up having a result which permits to have such condition fulfilled.
  
  Let $p > q$ then
  
  - $a^{q(p - 1) + q - 1} \equiv 1 \mod p$
  - $a^{q(p - 1) + q - 1} \equiv 1 \mod q$
  - $a^{p(q - 1) + p - 1} \equiv 1 \mod p$
  - $a^{p(q - 1) + p - 1} \equiv 1 \mod q$
  - $->$
  - $a^{q(p - 1) + q - 1} \equiv 1 \mod p = a^{q(p - 1)}a^{q - 1} \equiv 1 \mod p = a^{q - 1} \equiv 1 \mod p$ iff $a$ is a generator for a subgroup of order $o | q - 1$ and $o | p - 1$
  - $a^{q(p - 1) + q - 1} \equiv 1 \mod q = a \equiv 1 \mod q$ iff $a$ is a generator for a subgroup of order $o | p - 1$ and $o | q - 1$
  - $a^{p(q - 1) + p - 1} \equiv 1 \mod p = a \equiv 1 \mod p$ iff $a$ is a generator for a subgroup of order $o | q - 1$ and $o | p - 1$
  - $a^{p(q - 1) + p - 1} \equiv 1 \mod q = a^{p(q - 1)}a^{p - 1} \equiv 1 \mod q = a^{p - 1} \equiv 1 \mod q$ iff $a$ is a generator for a subgroup of order $o | p - 1$ and $o | q - 1$
  - $->$
  - $o = q - 1 | p - 1$ is the case which has the most number of solutions

Let's now consider the case $n = qlp$. We can recycle Ben's structure for this case and consider $ql = r$, then $n = pr$. We have that

$a^{n - 1} = a^{pr - 1} = a^{r(p - 1) + r - 1}$<br>
$and$<br>
$a^{n - 1} = a^{pr - 1} = a^{p(r - 1) + p - 1}$

thus, in order to have

$a^{r(p - 1) + r - 1} \equiv 1 \mod p$

we will have $p - 1 | r - 1$ and $r - 1 > p - 1$ in order to match Carmichael property. Also

$a^{p(r - 1) + p - 1} \equiv 1 \mod p$

is satisfied too having $a$ as generator for a subgroup of order $o = p - 1 | r - 1$. Now the important thing to understand is that this construction for $r - 1 > p - 1$ doesn't constrain us having an order which is less than $p - 1$. Now if $p > q > l$, $p - 1 | ql - 1$, $q - 1 | lp - 1$ and $l - 1 | qp - 1$, this would mean that the conditions to be a Carmichael number are satisfied because

$p - 1 | r - 1 = p - 1 | ql - 1$

and

$ql - 1 \nmid qlp - 1$

but

$qlp - 1 = p(ql - 1) + p - 1$<br>
$->$<br>
$a^{p(ql - 1)} \equiv 1 \mod p$ iff $p - 1 | ql - 1$ which implies $p - 1 | qlp - 1$.

Also

$a^{p(r - 1) + p - 1} \equiv 1 \mod q$<br>
$->$<br>
$a^{p(l(q - 1) + l - 1) + p - 1} \equiv 1 \mod q = a^{p(l(q - 1))}a^{p(l - 1)}a^{p - 1} \equiv 1 \mod q = a^{p(l - 1)}a^{p - 1} \equiv 1 \mod q = a^{pl - 1} \equiv 1 \mod q$

thus if $q - 1 | lp - 1$, $qlp - 1 = q(lp - 1) + q - 1$ hence $q - 1 | qlp - 1$.

$a^{p(r - 1) + p - 1} \equiv 1 \mod l$<br>
$->$<br>
$a^{p(q(l - 1) + q - 1) + p - 1} \equiv 1 \mod l = a^{p(q(l - 1))}a^{p(q - 1)}a^{p - 1} \equiv 1 \mod l = a^{p(q - 1)}a^{p - 1} \equiv 1 \mod l = a^{pq - 1} \equiv 1 \mod l$

thus if $l - 1 | qp - 1$, $qlp - 1 = l(qp - 1) + l - 1$ hence $l - 1 | qlp - 1$.

Summing all up we have that under

- $n = qlp, p > q > l$ where $q, l, p$ are odd primes (because if $l = 2$ then $qlp - 1$ would be odd and $p - 1, q - 1, l - 1$ wouldn't be able to divide it, note also that this reasoning holds for $n$ being composed by more than $3$ primes)
- $p - 1 < ql - 1$, and $p - 1 | ql - 1 | qlp - 1$
- $q - 1 | lp - 1 | qlp - 1$ ($q - 1 < lp - 1$ implied)
- $l - 1 | qp - 1 | qlp - 1$ ($l - 1 < qp - 1$ implied)

$n$ is a Carmichael number, because the constraints implied by $a^{n - 1} \equiv 1$ allow to have a CRT construction which is satisfied by $p - 1, q - 1, l - 1$ solutions respectively.
  
 Now, $n = p^{k}r$ can't be a Carmichael number because $\phi(p^{k}) \neq p^{k} - 1$.

 The upper bound probability of $1/4$ is because if we consider $p = 2$ we get

 $(p - 1)/(p^{k} - 1) = 1/3$

 but
 
 $\phi(2^{k}) = 2^{k - 1} \nmid 2^{k}r - 1$

 always. And then we can consider $p \geq 3$ for which Ben formula holds, while remembering that for $p = 2$, $n$ will never fool the Fermat test.
 
 This means that $n$ will be squarefree and the product of at least three distinct primes.

</p>

 
