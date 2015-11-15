# Corrupted (375)

## Problem

I'm not an anime person, but I have weeaboo friends. One of them uses [this](https://www.easyctf.com/static/problems/yandere/yuno.jpg) as her profile pic. Unfortunately, she sent me a corrupted version. Can you find out what she's hiding?

## Hint

Who is Yuno Gasai? From what I hear, she's a scary yandere.
On a more serious note, stop trying to fix the image. It's not broken -- it's hiding stuff.

## Write-Up

We are given this image:
![Corrupted Yuno](https://www.easyctf.com/static/problems/yandere/yuno.jpg)

We should probably find the original picture. We search up "Gasai Yuno" on Google, and set the image size to 130x130 - the same as the picture we were given. We soon see the original picture can be found [here](http://cs623226.vk.me/v623226328/12faa/xLiegfGaFhA.jpg).

![Actual Picture](http://cs623226.vk.me/v623226328/12faa/xLiegfGaFhA.jpg)

Let's look for differences! After opening up both of them in my favorite hex editor - HxD - and comparing differences, we see that there are a few bytes off by one. This seems to be a generic steg, in which individual colors are changed a little and the changes in bytes form some message. I wrote a quick python script to find the differences:

```python
a = open('yuno.jpg','rb').read()
b = open('yunoOriginal.jpg','rb').read()
o = []
for i in range(len(a)):
    o.append(abs(a[i]-b[i]))

print(''.join([str(i) for i in o]).strip('0'))
```

and it prints out:
`110010101100001011100110111100101100011011101000110011001111011011110010111010101101110011011110110111100110000011011110110111101101111011011110110111101101111011011110110111101101111011011110110111101101111001100000110111101111101`. We try converting to ascii, but all we get is gibberish. But wait - what if we pad zeroes in front?! We add another zero to the beginning of our string, and get: `easyctf{yunoo0oooooooooooo0o}`

Flag: `easyctf{yunoo0oooooooooooo0o}`
