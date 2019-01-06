> 参考文档：https://stackoverflow.com/questions/51726203/installing-python3-6-alongside-python3-7-on-mac/51727268

emmmmm 当然可以使用docker和virtualenv来实现多种python版本。

这个骚操作是我从stackoverflow中看到的，忽然发现很适合我这种懒人。


Try using `brew` for example if already using Python 3:

```
$ brew unlink python
```

Then [install python 3.6.5](https://stackoverflow.com/a/51125014/1135424):

```
$ brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/f2a764ef944b1080be64bd88dca9a1d80130c558/Formula/python.rb
```

To get back to python `3.7.0` use:

```
$ brew switch python 3.7.0
```

And if need 3.6 again switch with:

```
$ brew switch python 3.6.5_1
```
