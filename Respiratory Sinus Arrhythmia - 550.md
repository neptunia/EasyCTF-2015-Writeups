# Respiratory Sinus Arrhythmia (550)

## Problem

Zob is stupid and modified his RSA program [here](https://www.easyctf.com/static/problems/rsa3/rsa3.java). He left after we fired him after the mess we had with his bad matrix cipher, but he kept the server password encrypted with this program (we put it in the program for you). Can you help us recover the password?

## Hint

Fractions :D

## Write-Up

After staring at it for a while, we see that this encryption suffers from a small private key, and apparently it's vulnerable to fractions. This means we should use Wiener's attack. I used this program: https://github.com/pablocelayes/rsa-wiener-attack. It doesn't really find anything, except if I get rid of the discriminant part (since that only works for 2-prime RSA and we have 3 primes), it shoots out 3 possible values for d: 7, 15, and 2065659454658019741780522570419376267931036082571377113532943424886952853219885592888546058433700094658894057336807857079282524074810812659298259543680548665395629065595040507999387715499956304724045136225056327421882545091174374570023424535112498454573479876181424784110131185483172182567718771705357890630390524098272037132047386754987122041548608712507049648368618233039815189112679401751673818053814414216405396571717867682135903220899506431110321. Unfortunately, when we attempt to decrypt this it doesn't work. What's the problem?

BTW, have a small script I wrote to decrypt the message to reverse their encryption:

from decimal import Decimal
dec = pow(encrypted,d,modulus)
thing = Decimal(str(dec ^ blargh))
o = Decimal(str(modulus-phi-1))
a = thing/o
  if a > 0:
  hd = hex(int(a)).lstrip('0x')
  hd2 = hex(int(dec)).lstrip('0x')
  message = [chr(int(hd2[i*2:i*2+2],16)) for i in range(len(hd2)//2)]
  print(''.join(message))

Anyways, we try running the original program in Java, and we see the problem: the message is greater than modulus! Well, that's not hard to fix. We just add modulus to message and attempt decryption. If it doesn't work, we add modulus again. Soon, we get a flag!


Flag: `easyctf{c0N+1nU3d_fr4c+10n5_pH1}`

P.S. I forgot to save my code and i don't know where it went. sorry :3

