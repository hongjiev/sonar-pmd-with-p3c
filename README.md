## 在 sonar-pmd 项目上增加 p3c 规则

你当然也可以直接 build 我这个工程，打出来的包直接用。

但是我之后不会维护这个 repo，所以大家自己掌握制作方法，当前时间：2022-04-24，当前 sonar-pmd 版本 3.2.0-SNAPSHOT，支持 java 15，p3c 规则 56 条。

请按照下面的步骤，边做边对比 p3c 的 repo: https://github.com/alibaba/p3c 阿里只提供了 p3c-pmd 的源码，没有提供 sonar 插件。

1. 克隆 sonar-pmd 最新的版本，之后基于它进行修改。不过如果你的 sonarqube 不是最新版本的话，需要关注下兼容性问题，我用的 sonarqube 是 v9.4

2. sonar-pmd-plugin/pom.xml 添加依赖

   ```xml
       <dependency>
         <groupId>com.alibaba.p3c</groupId>
         <artifactId>p3c-pmd</artifactId>
         <version>2.1.1</version>
       </dependency>
   ```

3. 修改 pmd.properties，增加 p3c 的 56 条规则：见 /resources/org/sonar/l10n/pmd/rules/pmd.properties。当然可能你现在看到的 p3c 有更多的规则也不不一定。

4. 增加 rules-p3c.xml，见 /resources/org/sonar/plugins/pmd/rules-p3c.xml

5. 增加规则描述 html 目录，见 /resources/org/sonar/l10n/pmd-p3c

6. 修改 pmd-model.xml，见 /resources/com/sonar/sqale/pmd-model.xml，搜索一下 p3c 就明白了

7. 修改 PmdRulesDefinition.java，见 66 行

8. 没了，你可以 mvn clean package 了，然后把打出来的包放到 sonarqube 的 extensions/plugins 目录下

---
# SonarQube PMD Plugin [![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.sonarsource.pmd/sonar-pmd-plugin/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.sonarsource.pmd/sonar-pmd-plugin) [![Build Status](https://api.travis-ci.org/jborgers/sonar-pmd.svg?branch=master)](https://travis-ci.org/jborgers/sonar-pmd) [![SonarStatus](https://sonarcloud.io/api/project_badges/measure?project=org.sonarsource.pmd%3Asonar-pmd&metric=alert_status)](https://sonarcloud.io/dashboard?id=org.sonarsource.pmd%3Asonar-pmd) [![SonarStatus](https://sonarcloud.io/api/project_badges/measure?project=org.sonarsource.pmd%3Asonar-pmd&metric=coverage)](https://sonarcloud.io/dashboard?id=org.sonarsource.pmd%3Asonar-pmd)
Sonar-PMD is a plugin that provides coding rules from [PMD](https://pmd.github.io/).

For a list of all rules and their status, see: [RULES.md](https://github.com/jborgers/sonar-pmd/blob/master/docs/RULES.md)

## Installation
The plugin is available in the SonarQube marketplace and should preferably be installed from within SonarQube (Administration -->  Marketplace --> Search _pmd_).

Alternatively, download the [latest JAR file](https://github.com/jborgers/sonar-pmd/releases/latest), put it into the plugin directory (`./extensions/plugins`) and restart SonarQube.

## Usage
Usage should be straight forward:
1. Activate some PMD rules in your quality profile.
2. Run an analysis.

### Troubleshooting
Sonar-PMD analyzes the given source code with the Java source version defined in your Gradle or Maven project.
In case you are not using one of these build tools, PMD uses the default Java version - which is **1.6**.  

If that does not match the version you are using, set the `sonar.java.source` property to tell PMD which version of Java your source code complies to. 

Possible values: 
- 1.4
- 1.5 or 5 
- 1.6 or 6 
- 1.7 or 7 
- 1.8 or 8
- 9
- 10
- 11
- 12
- 13
- 14
- 15
- 16
- 17

## Description / Features
PMD Plugin|2.0|2.1|2.2|2.3|2.4.1|2.5|2.6|3.0.0|3.1.x|3.2.x|3.3.x|3.4.x (to release)
-------|---|---|---|---|---|---|---|---|---|---|---|---
PMD|4.3|4.3|5.1.1|5.2.1|5.3.1|5.4.0|5.4.2|5.4.2|6.9.0|6.10.0|6.30.0|6.41.0
Max. supported Java Version | |  |  |  |  | 1.7 | 1.8 | 1.8 | 11 | | 15|17
Min. SonarQube Version |  |  |  |  |  | 4.5.4 | 4.5.4 | 6.6 | | | 6.7|7.x

A majority of the PMD rules have been rewritten in the Java plugin. Rewritten rules are marked "Deprecated" in the PMD plugin, but a [concise summary of replaced rules](http://dist.sonarsource.com/reports/coverage/pmd.html) is available.

## Rules on test
PMD tool provides some rules that can check the code of JUnit tests. Please note that these rules (and only these rules) will be applied only on the test files of your project.

## License
Sonar-PMD is licensed under the [GNU Lesser General Public License, Version 3.0](https://github.com/jborgers/sonar-pmd/blob/master/LICENSE.md).

Parts of the rule descriptions displayed in SonarQube have been extracted from [PMD](https://pmd.github.io/) and are licensed under a [BSD-style license](https://github.com/pmd/pmd/blob/master/LICENSE).  

