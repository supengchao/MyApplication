# MyApplication


测试上传自己的代码到jcenter服务器，使用compile引用
参考文章
http://my.oschina.net/weiCloudS/blog/384865

核心代码

    buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.1.0'
        classpath 'com.github.dcendents:android-maven-plugin:1.2'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
    }
    allprojects {
    repositories {
        jcenter()
    }
    }

##---------------------------------------------------------------------

    apply plugin: 'com.android.library'
    apply plugin: 'com.github.dcendents.android-maven'
    apply plugin: 'com.jfrog.bintray'
    // 提交到仓库中的版本号
    version = "1.0.0"
    android {
    compileSdkVersion 21
    buildToolsVersion "21.1.2"
    defaultConfig {
        minSdkVersion 14
        targetSdkVersion 21
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    }
    dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:21.0.2'
    }
    def siteUrl = 'https://github.com/fyales/tagcloud'      // 项目的主页
    def gitUrl = 'https://github.com/fyales/tagcloud.git'   // Git仓库的url
    group = "com.fyales.android"
    install {
    repositories.mavenInstaller {
        pom {
            project {
                packaging 'aar'
                name 'Android TagCloud'    //项目描述
                url siteUrl
                licenses {
                    license {
                        name 'The Apache Software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }
                developers {
                    developer {
                        id 'fyales'        //填写的一些基本信息
                        name 'fyales'
                        email 'fyales@gmail.com'
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl
                }
            }
        }
    }
    }
    task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
    }
    task javadoc(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
    }
    task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
    }
    artifacts {
    archives javadocJar
    archives sourcesJar
    }
    Properties properties = new Properties()
    properties.load(project.rootProject.file('local.properties').newDataInputStream())
    bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")
    configurations = ['archives']
    pkg {
        repo = "maven"
        name = "TagCloud"    //发布到JCenter上的项目名字
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = ["Apache-2.0"]
        publish = true
    }
    }

#注意
gradle2.4或以上的版本，一定要引用1.3版本的maven，否则报找不到maven错
classpath "com.github.dcendents:android-maven，-gradle-plugin:1.3"
