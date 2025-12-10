mavenCanter中'com.arthenica:ffmpeg-kit-https:6.0-2' 已经没有发布了，
所以只能使用mavenLocal()
步骤：
1. 安装maven: 
   1. (Mac)  brew install maven 
   2. (Windows) 
      1. 访问 Maven官网 
      2. 下载并解压到指定目录 
      3. 配置环境变量 MAVEN_HOME 和 PATH
   3.Linux: 使用包管理器安装
      4. sudo apt-get install maven    # Ubuntu/Debian
          sudo yum install maven        # CentOS/RHEL
   4.验证安装: `mvn -v`
2. 安装成功后，再回到包含 ffmpeg-kit-https-6.0-2.aar 的目录运行之前的命令：
cd /Users/xxx/DolphineMedia/txm_flutter_run/ffmpeg_kit_flutter/android/libs

mvn install:install-file -Dfile=ffmpeg-kit-https-6.0-2.aar \
   -DgroupId=com.arthenica \
   -DartifactId=ffmpeg-kit-https \
   -Dversion=6.0-2 \
   -Dpackaging=aar
3. 最后在flutter/android/build.gradle中添加`mavenLocal()`
4. 安装成功后，你的本地maven仓库中（ ~/.m2/repository ）会有 以下文件存在：
   ~/.m2/repository/com/arthenica/ffmpeg-kit-https/6.0-2/ffmpeg-kit-https-6.0-2.aar
   ~/.m2/repository/com/arthenica/ffmpeg-kit-https/6.0-2/ffmpeg-kit-https-6.0-2.pom
5. 然后再重新构建项目：
   flutter clean
   flutter pub get
   flutter build apk
6. 这里虽然放了一个libs文件夹，但是是不用本地依赖的，这个依赖打包会报错；至于为什么报错，请看下边的解释。
7. 所以只能用本地maven仓库的依赖打包才可以。
8. 但是本库还少了implementation("com.arthenica:smart-exception-java:0.2.1")，好在这个maven仓库里还有，可以直接依赖

---
ps:为什么不能自己把aar文件发不到maven官网的仓库里去？
不可以，你不能将 [ffmpeg-kit-https-6.0-2.aar](file:///Users/goals/DolphineMedia/txm_flutter_run/ffmpeg_kit_flutter/android/libs/ffmpeg-kit-https-6.0-2.aar) 发布到 Maven 官方仓库。

## 原因如下：

1. **版权问题**
    - [ffmpeg-kit-https-6.0-2.aar](file:///Users/goals/DolphineMedia/txm_flutter_run/ffmpeg_kit_flutter/android/libs/ffmpeg-kit-https-6.0-2.aar) 是第三方库文件，属于 Arthenica 公司的知识产权
    - 未经授权发布他人的库文件构成版权侵犯

2. **Maven Central 发布要求**
    - 需要是开源项目且有合法的许可证
    - 需要项目源代码和构建配置
    - 需要经过严格的审核流程

## 建议的替代方案：

1. **使用官方依赖**
   ```gradle
   implementation 'com.arthenica:ffmpeg-kit-https:6.0-2'
   ```

2. **本地 Maven 仓库**
    - 安装到本地 `~/.m2/repository` 目录
    - 仅供本地开发使用

3. **私有 Maven 仓库**
    - 如果企业有私有 Nexus 或 Artifactory
    - 可以部署到内部仓库供团队使用

4. **直接放在 libs 目录**
    - 仅用于应用模块，不用于库模块
    - 适用于快速原型开发
