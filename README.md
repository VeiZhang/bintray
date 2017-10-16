# 简化Gradle发布Android项目到Jcenter/Maven

## 使用bintray-release插件上传

个人感觉bintray-release比gradle-bintray-plugin要方便很多，故使用bintray-release发布依赖。

### 仅发布到JCenter

参考[novoda][novoda]

在Library的Module里build.gradle文件，添加的配置

```
publish {
    userOrg = 'veizhang'
    groupId = 'com.excellence'
    artifactId = 'permission'
    publishVersion = '1.0.0'
    desc = 'Android权限管理'
    website = 'https://github.com/VeiZhang/Permission'
}
```


### 同时发布到Jcenter和Maven

参考[WiKi][Add-support-for-syncing-to-maven-central]

* 在项目根目录的build.gradle文件，添加配置
	```
	ext {
	    ARTIFACT_ID = 'permission'
	    VERSION_NAME = '1.0.0'
	    VERSION_CODE = 1 //your version
	
	    DESCRIPTION = 'Android权限管理'
	
	    SITE_URL = 'https://github.com/VeiZhang/Permission'
	    GIT_URL = 'git@github.com:VeiZhang/Permission.git'
	    GROUP_NAME = 'com.excellence'
	    COMPILE_SDK = 25
	    BUILD_TOOLS = '25.0.0'
	
	    MODULE_NAME = 'PermissionLibrary'
	
	    LICENSE_NAME = 'The Apache Software License, Version 2.0'
	    LICENSE_URL = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
	
	    DEVELOPER_ID = 'VeiZhang'
	    DEVELOPER_NAME = 'Tiimor'
	    DEVELOPER_EMAIL = 'tiimor@qq.com'
	
	    IS_UPLOADING = project.getGradle().startParameter.taskNames.any { it.contains('bintrayUpload') }
	}
	
	subprojects {
	    group = GROUP_NAME
	    version = VERSION_NAME
	
	    if (IS_UPLOADING && project.name in [MODULE_NAME]) {
	        println project.name
	        apply plugin: 'maven'
	
	        gradle.taskGraph.whenReady { taskGraph ->
	            taskGraph.getAllTasks().find {
	                it.path == ":$project.name:generatePomFileForMavenPublication"
	            }.doLast {
	                file("build/publications/maven/pom-default.xml").delete()
	                println 'Overriding pom-file to make sure we can sync to maven central!'
	                pom {
	                    //noinspection GroovyAssignabilityCheck
	                    project {
	                        name "$project.name"
	                        artifactId ARTIFACT_ID
	                        packaging project.name == 'compiler' ? 'jar' : 'aar'
	                        description DESCRIPTION
	                        url SITE_URL
	                        version VERSION_NAME
	
	                        scm {
	                            url SITE_URL
	                            connection GIT_URL
	                            developerConnection GIT_URL
	                        }
	
	                        licenses {
	                            license {
	                                name LICENSE_NAME
	                                url LICENSE_URL
	                            }
	                        }
	
	                        developers {
	                            developer {
	                                id DEVELOPER_ID
	                                name DEVELOPER_NAME
	                                email DEVELOPER_EMAIL
	                            }
	                        }
	                    }
	                }.writeTo("build/publications/maven/pom-default.xml")
	            }
	        }
	    }
	}
	```

* 在Library的Module里build.gradle文件，添加的配置
	```
	publish {
	    userOrg = 'veizhang'
	    groupId = 'com.excellence'
	    artifactId = 'permission'
	    publishVersion = '1.0.0'
	    desc = 'Android权限管理'
	    website = 'https://github.com/VeiZhang/Permission'
	}
	```

**注意**：根目录里的配置VERSION_NAME与Module里的配置publishVersion等信息一致。


## 使用gradle-bintray-plugin插件上传

**[转载自大神][yanzhenjie]**

这个仓库是因为个人发布项目到Jcenter/Maven时每次需要写maven文件比较麻烦，所以放到github上来方便自己，如果其它人看到之后也喜欢这种方式，不妨也试试。

这个库只是为了简化发布，并不是发布流程，具体的发布流程，请参考链接：[发布讲解][发布讲解]

本库提供的方式可以只发布到Jcenter，或者发布Jcenter和Maven，后者的原理是发布到Jcenter后同步到Maven，所以不能只发布到Maven。  

- [仅发布到Jcenter][Bintray.md]
- [同时发布到Jcenter和Maven][Maven.md]

[novoda]:https://github.com/novoda/bintray-release#simple-usage
[Add-support-for-syncing-to-maven-central]:https://github.com/novoda/bintray-release/wiki/Add-support-for-syncing-to-maven-central
[yanzhenjie]:https://github.com/yanzhenjie/bintray "yanzhenjie"
[发布讲解]:http://blog.csdn.net/yanzhenjie1003/article/details/51672530 "发布流程讲解"
[Bintray.md]:https://github.com/VeiZhang/bintray/blob/master/Bintray.md
[Maven.md]:https://github.com/VeiZhang/bintray/blob/master/Maven.md
