## 前言
一个网页是如何在浏览器上进行缓存的,其中涉及了哪些机制,有哪些技术可以采用,如何才能最大化利用缓存.个人觉得这些知识点都是一个前端工程师必须了解的.曾经在面试的时候被问过一个问题,"有没有不返回304,同时又使用了缓存的情况?".当时因为对缓存机制了解不深,所以答不上来.下面我总结一下这方面的知识.

## 通过 HTTP 报头

### Cache-Control,Expires(响应报头)
这两个响应报头是浏览器缓存机制的第一道坎.当你发起一个 GET 请求时,服务器返回的响应报头里含有这两个报头,那么你下一次发起请求时(以某种方式, F5刷新除外),在你设置的资源过期时间前浏览器都不会再次请求服务器,而是直接使用已经缓存的版本.并且 status code 是200.这也是我上面说的不是只有返回了304才会使用缓存

![clipboard.png](https://segmentfault.com/img/bVqjrp)
在 chrome 里我们可以清楚的看到 status code 旁边写的是(from cache).响应报头大致的形式也就是像上面那张图一样.

### Last-Modified,ETag(响应报头);If-Modified-Since,If-None-Match(请求报头)
这几个报头可以说是浏览器缓存机制里的第二道坎.当你请求某个资源时,如上面的图,响应报头会设置 Last-Modified 和 ETag,Last-Modified 是这个资源的最后修改时间, ETag 是该资源的唯一标识符.这两个值会在你再一次请求该资源时以 If-Modified-Since 和 If-None-Match 的形式附在请求头里发给服务器由服务器判读该资源是否过期.如果没过期,返回304,如果过期了,返回带有响应主体也就是 content并且status code为200的响应.请求报头大致如下.这个过程称为服务器再认证.

![clipboard.png](https://segmentfault.com/img/bVqjrT)

### 这两种报头的作用区间

![clipboard.png](https://segmentfault.com/img/bVqjxa)
借用一张简洁的图来展示它们的作用区间的区别

### GET与 POST
只有GET请求才会触发浏览器的缓存机制.这也是 GET和 POST 的区别之一. GET 的目的应该是请求资源并且不会对服务器上的数据产生改动,是幂等的.而 POST 的目的就是为了修改数据.所以对于一些GET 方法的Ajax也可以使用响应报头的方法进行缓存,  jQuery 里的 ajax 方法里的 cache=false 就只对 GET 方法有效,原理是在 url 后加上一个时间戳类似于 http://www.test.com?a=b_201510300000,以防止浏览器缓存请求.


## 通过 manifest(html5)
manifest 是 一种新的缓存方式,通过 manifest 可以让网页离线使用.下面我总结下在使用 manifest 的时候一些要注意的地方,应该工作上没有使用 manifest 的场景,所以不做过多的深入

 1. manifest文件 的 content-type必须是'text/cache-manifest',不然浏览器会不识别这是缓存文件.
 2. 引入 manifest 的方式是<html lang="en" manifest="hello.manifest">,默认的引入的 html 文件也会被缓存.
 3. manifest 文件的格式类似
```javascript
CACHE MANIFEST
bundle.js
```
 4.一旦浏览器缓存成功后,下一次浏览器会先使用缓存的文件,然后再发请求检查 manifest 文件是否有变化.所以在下次打开网页前,用户看到的还是旧的文件.你可以使用 window.applicationCache 来控制当发现 manifest 文件过期后自动刷新,这样来保证用户能看到新的内容.
 5.manifest 文件的检查是根据类似于文件 md5的方式来检查的,也就是说如果你在 manifest 文件里加了一个新空行,浏览器也会认为 manifest 文件有更新,然后重新下载缓存文件以备下次使用.

## 通过 webStorageApi(html5)
 webStorageApi 我主要讲一下 localStorage, 因为在工作中有使用场景.在开发新后台主页面的时候,左侧是一个很长的多级菜单,右面是 主内容.大概是这样

![图片描述][1]
当左侧菜单拉长时,点击链接跳到新页面,这时左侧菜单的滑动条也要保持在上个页面的滑动位置,这时可以监听点击菜单事件,通过 localStorage 来保存滑动条高度
```javascript
    if(target.attr('href') !== "javascript:void(0);"){
        if(window.localStorage){
            localStorage.setItem("siteScroll", scrollTop);
        }
    }else{
        $('.nav-secondary').scrollTop(target.offset().top - menuOffsetTop);
    }
```
下次页面打开的时候再获取并设置滑动条的位置
```javascript
   if(window.localStorage){
        var scrollTop = localStorage.getItem("siteScroll");
        $('.nav-secondary').scrollTop(scrollTop);
    }
```
> 因本人水平有限,总结过程中难免出错,还望大家能够指出,谢谢!

查阅应用了以下书籍和内容
> http://www.alloyteam.com/2012/03/web-cache-2-browser-cache/
> html5高级程序设计


  [1]: https://segmentfault.com/img/bVqFvn