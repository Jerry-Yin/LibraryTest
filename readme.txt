关于android studio倒入第三方类库的一些实践说明

本实例工程中
添加了一个第三方库 libray

步骤：
  1.在工程根目录下面新建一个 libs 文件夹；
  2.将 library 库复制到文件libs夹里面；
  3.打开工程，打开settings.gradle并修改；
      修改前：
            include ':app'
      修改后：
            include ':app'
            include ':libs:library'
            
            或者：
            include ':app', ':library'
            project(':library').projectDir = new File('libs/library')
  
  4.修改app中的 build.gradle 文件：（添加了一行编译代码）
        修改前：
            dependencies {
              compile fileTree(dir: 'libs', include: ['*.jar'])
              compile 'com.android.support:appcompat-v7:22.2.0'
            }
        修改后：
            dependencies {
              compile fileTree(dir: 'libs', include: ['*.jar'])
              compile 'com.android.support:appcompat-v7:22.2.0'
              compile project(":libs:library")
            }
  5.修改library的编译器版本，sdk版本 等各种配置文件的版本，需要跟当前工程的版本一致；
  6.编译成功，不出意外的话可以运行了；
  
意外：
  运行时出现  ....finished with non-zero values 2  的错误；
  原因：library中出现了和本工程中相同的jar包，特别是support－v4包；
  解决：在 app 的build.gradle里面最后添加：
        configurations {
          all*.exclude group: 'com.android.support', module: 'support-v4'
        }
      
  
            
