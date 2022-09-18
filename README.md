# How to use git as generator of twtxt.txt file ?

First you need to create you own git repository and git init it.

## Tweet
First when you commit you should use the following syntax:
```
git commit --allow-empty -m "$(git rev-parse --abbrev-ref HEAD) Hello World"
```
Of course you replace Hello World by your message.

## Reply
Then if you want to answer a Thread, you will be doing it in a branch, so you have to:

1 - Create a Branch with the Thread Name
```
git -b checkout "(#REPLY_HASH)"
```

2- Commit your response
```
git commit --allow-empty -m "$(git rev-parse --abbrev-ref HEAD) YOUR_RESPONSE"
```

Don't forget to come back to the master branch if you want to continue talking outside a specific Thread.

## Generate twtxt.txt file
```
git log --all --reverse --date=format:'%Y-%m-%dT%H:%M:%SZ' --format=format:'%cd %h %s' | awk -F ' ' '{ if($3 == "HEAD" || $3 == "master") {printf $1 " (#" $2 ") ";} else printf $1 " " $3 " "; {for (i=4; i<NF; i++) printf $i " ";print $NF}}' > twtxt.txt
```

### Example of the generation on that Repository

```
2022-09-18T01:41:27Z (#183c5e8) Hello World
2022-09-18T01:41:54Z (#64d069c) How are the clouds today ?
2022-09-18T01:42:10Z (#4c035fc) I love Birds
2022-09-18T01:43:04Z (#4c035fc) I also love Butterflies
2022-09-18T01:43:21Z (#4c035fc) And of course tweet and git
2022-09-18T01:43:42Z (#4f22cac) Enjoy;
```
