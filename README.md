# 简化Gradle发布Android项目到Jcenter/Maven

**[转载自大神][yanzhenjie]**

这个仓库是因为个人发布项目到Jcenter/Maven时每次需要写maven文件比较麻烦，所以放到github上来方便自己，如果其它人看到之后也喜欢这种方式，不妨也试试。

这个库只是为了简化发布，并不是发布流程，具体的发布流程，请参考链接：[发布讲解][发布讲解]

本库提供的方式可以只发布到Jcenter，或者发布Jcenter和Maven，后者的原理是发布到Jcenter后同步到Maven，所以不能只发布到Maven。  

- [仅发布到Jcenter](./Bintray.md)
- [同时发布到Jcenter和Maven](./Maven.md)

[yanzhenjie]:https://github.com/yanzhenjie/bintray "yanzhenjie"
[发布讲解]:http://blog.csdn.net/yanzhenjie1003/article/details/51672530 "发布流程讲解"