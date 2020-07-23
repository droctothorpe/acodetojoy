---
title: "Explain in the Membrane"
date: "2019-07-22"
hideReadMore: true
Description: An exploration of explainshell and cheat.sh, two excellent bash productivity enhancement tools.
---
## Explainshell
I stumbled into this brilliant tidbit in a [post](https://news.ycombinator.com/item?id=13994923) by LostCharacter on ycombinator.

Here's the actual code:

```bash
explain () {
    if [ "$#" -eq 0 ]
    then
        while read -p "Command: " cmd
        do
            curl -Gs "https://www.mankier.com/api/explain/?cols="$(tput cols) --data-urlencode "q=$cmd"
        done
        echo "Bye!"
    elif [ "$#" -eq 1 ]
    then
        curl -Gs "https://www.mankier.com/api/explain/?cols="$(tput cols) --data-urlencode "q=$1"
    else
        echo "Usage"
        echo "explain                  interactive mode."
        echo "explain 'cmd -o | ...'   one quoted command to explain it."
    fi
}
```


It interacts with [www.mankier.com](https://www.mankier.com/) (an awesome website in its own right) to explain what a specified bash command does, and most compellingly, what the specified flags do.

[Explainshell](https://www.explainshell.com/) offers a similar service, but staying in the terminal ecosystem is preferable because it alleviates the need to switch contexts and justifies a degree of technical elitism. Browsers are for newbs, after all. You're reading this via Lynx I assume.

Here's an example of usage and corresponding output:

```bash
❯ explain 'rsync -a'
rsync(1)
  a fast, versatile, remote (and local) file-copying tool
-a (-A, --ARCHIVE)
    This is equivalent to -RLPTGOD. It is a quick way of saying you want recursion and want to preserve almost everything (with -H being a
    notable omission). The only exception to the above equivalence is when --FILES-FROM is specified, in which case -R is not implied.
    
    Note that -A DOES NOT PRESERVE HARDLINKS, because finding multiply-linked files is expensive. You must separately specify -H.
                                                                                                             https://www.mankier.com/1/rsync
```

Just add the function to your respective shell profile, and revel in the glory of bashlightenment.

## Cheat.sh
[Cheat.sh](https://cheat.sh/) is very useful. The command line interface can be installed in [two steps](https://github.com/chubin/cheat.sh#installation):

```bash
curl https://cht.sh/:cht.sh | sudo tee /usr/local/bin/cht.sh
chmod +x /usr/local/bin/cht.sh
```

Once installed, just type `cheat.sh <any question>`. It runs a stack overflow search and returns the response with the highest upvotes, circumventing the need to context switch to your browser.

Here's an example:

```bash
❯ cht.sh python random int
#  python - Generate random integers between 0 and 9
#
#  Try:
from random import randint
print(randint(0, 9))
#  More info:
#  https://docs.python.org/3/library/random.htmlrandom.randint
#
#  [kovshenin] [so/q/3996904] [cc by-sa 3.0]
```

## Story Time

Since we're (sort of) on the subject, I heard a funny story from a friend recently.

He complimented a man wearing this t-shirt:
![](images/shell_script_tshirt.jpg)

"Do you know who I am?" Asked the man.

"No," replied my friend.

"I'm [David Korn](https://en.wikipedia.org/wiki/David_Korn_(computer_scientist)). I invented the Korn shell. When I say I can replace you with a shell script, I mean it."