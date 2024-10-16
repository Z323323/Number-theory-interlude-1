# Restarting from CRT
<p>This repo should be read after https://github.com/xyzhyn/Chinese-remainder-theorem. <br>
As already mentioned, I'll try to get the most out https://crypto.stanford.edu/pbc/notes/numbertheory in order to set a good math background to understand https://zcash.github.io/halo2 trying to simplify the path for anyone would want to approach that book. <br>
In the ending section of the CRT repo I wrote that the number of square roots of unity is ~ $z^{congruences}$ where $congruences$ divisors are pairwise coprime; the '~' is because for example if we had $x^{2} \equiv 1 \mod 2$, since $1 \mod 2 = -1 \mod 2$ we would have 1 solution in this case which would break $z^{congruences}$ leading to $z^{congruences - 1}$. Also I wrote $z$ to be more general than '2' but take it with a pinch because there could be corner cases, and also mixed degrees into the starting congruences so just move forward and don't give it too much importance because it's very likely to be broken.<br>
The important thing to understand is that <em>"to describe the solutions of a polynomial f(x) over </em>$Z_{n}$<em> for any n we need to find the roots of f(x) over </em>$Z_{{p}^{k}}$<em> for prime powers </em>$p^{k}$<em>".</em>
</p>

## Wilson's theorem
<p>Things start getting harder here, but I'll try to simplify this expl. as much as possible.<br>
The theorem states that for any integer $p > 1$ we have $(p - 1)! = - 1 \mod p$ if and only if p is prime. To understand this lightning fast just think about this: every number from 1 to p - 1 (mod p) has a multiplicative modular inverse and that inverse is unique for that number with two exceptions: 1 and p - 1 whose inverses are themselves. Then why multiplying them makes the rule to hold? Simply because $1(p - 1) = p - 1 = - 1 (\mod p)$. The hard thing to prove here is the uniqueness and existence of multiplicative modular inverses from 2 to p - 2. Before delving into that or assuming it's true (sometimes is necessary and this time it could be the case) let's understand why this formula is enough to prove that p is prime. If p wasn't prime it should mean that p is composed by 2 or more primes multiplied, then from our previous reasoning about roots of unity the formula wouldn't produce $- 1$ as result, (also) because of roots of unity which are the inverses of themselves. Powers of 2 follow a different reasoning; they don't have 1 root of unity like one might think from $1 \mod 2 = -1 \mod 2$ but they have two of them ($1$ and $p - 1$), does this breaks the rule then? No, because of multiplicative modular inverses from $2$ to $p - 2$: powers of 2, and generally every integer obtained from primes multiplied don't follow this: <em>"uniqueness and existence of multiplicative modular inverses from 2 to p - 2".</em> Let's make 2 simple examples: <br>
$2 \mod 4$<br>
-><br>
'2' is not a multiplicative modular inverse nor a root of unity.<br>
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
Now look at this: <br>
R----I----X----R----X----X----I----I----X----X----R----X----X----R<br>
Where R = root, I = inverse, X = neither one of them.
Some simmetry is individuable here but for the moment we are not interested about this.<br> What we actually care about is that it's clear that whenever there are non-coprimes in the set, there are elements which are not roots nor inverses then it's resonable to assert that Wilson's theorem holds. The problem is about the 'only if' statement inside the theorem which should be proved but I'm moving forward assuming it's true.
</p>

## Units
<p>Now we can easily derive this concept of units and correlate that with Totient function:<br>
if $z \in Z_{n}$ has an inverse then $z$ is a unit. The set of units is denoted by $Z_{n}^{*}$. <br>
From the previous section is clear that $z$ is a unit if and only if $z$ and $n$ are coprime. Here appears the Totient function which is useful to calculate the number of coprimes of an integer. See https://github.com/xyzhyn/Euler-Totient-function-multiplicativity-demonstration if you are interested.
</p>

## Modular exp. and DLP
<p>These are well covered in https://crypto.stanford.edu/pbc/notes/numbertheory/exp.html so I won't delve them. The absence of a structure of the powers of $x \mod p$ is quite magical and it's one of the foundations of cryptography (even if the actual process which destroy the structure is combined with elliptic curves in PLONK). Also notice how $3^{x} \equiv 6 \mod 7$ solving formula decreases the modulo by 1 ($x \equiv 3 \mod 6$), this could be helpful further on.
</p>

## Nonunits
<p>From the previous example is clear that we will always face nonunits every time we deal with some $\mod p$ where $p$ is not prime. It's also clear that we will end up having $ad \equiv 0 \mod p$ where $p$ is some $a * \dots$.<br> $d$ is also easy to find: $d = \frac{a * \dots}{a}$. Here Ben Lynn is not clear about why actually we don't bother about nonunits so I'll provide my view: at least from elliptic curves operations (but I guess this is reflected in the DLP, playing with crypto formulas) we don't want to get a 0 from multiplications because $0$ breaks the unstructured (unpredictable) behaviour of the modular exp. (DLP) and elliptic curves, and since modular multiplicative groups are cyclic we would end up having a lot of zeros around. That's why usually crypto operations use $\mod p$ where p is a very big prime (but it could not be always the case -> the more you know the more you are prepared).
</p>

