---
title: sails学习
categories:
- nodeJs 

---
## 我对sails的理解
之前用express+mongoose写过一个外包网站,express+mongoose写一些小网站来说确实很快,但当route,service,model这些东西一多起来,整个程序就会显得很乱,很难维护.这时候我们就需要更多的抽象,更多的约束,更多的封装.

sails的设计理念大部分借鉴与非常流行的rails,虽然说sails目前有很多不足的地方(比如对generator的支持(相比koa);使用了自己的waterline而不是大家喜欢的mongoose等等)

## sails如何lift
我画了一个sails.lift的脑图 [sails.lift脑图](http://naotu.baidu.com/file/33ef136563234f5ee4985d864ed40aa5?token=675569bff4c14d45),越是层次结构深的代码,画脑图对你阅读代码的帮助越大.
