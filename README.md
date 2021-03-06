# 在多人协同开发中，保持代码风格一样是一件十分重要的事情，而checkstyle 可以帮助我们做到这一点


# 一. Checkstyle 可以做什么


1.注解检查
2.代码块检查
3.类检查
4.代码检查
5.文件头检查
6.导包检查
7.度量检查
8.命名规范检查

------

# 二. Checkstyle 配置规则

 * 注解检查

```
        <!-- 检查注解存在的位置
             allowSamelineMultipleAnnotations 是否允许同行存在多个注解 default false
             allowSamelineSingleParameterlessAnnotation 是否允许同行存在无参数的注解  default true
             allowSamelineParameterizedAnnotation 是否允许同行存在带参数的注解  default false
        -->
        <module name="AnnotationLocation">
            <property name="allowSamelineMultipleAnnotations" value="false" />
            <property name="allowSamelineSingleParameterlessAnnotation" value="false" />
            <property name="allowSamelineParameterizedAnnotation" value="false" />
        </module>

        <!-- 检查注解风格
            注解元素风格 简洁  default compact_no_array
            注解元素数组末尾是否允许逗号  default never
            注解末尾是否允许空的括号 default always
        -->
        <module name="AnnotationUseStyle">
            <property name="elementStyle" value="compact" />
            <property name="trailingArrayComma" value="never" />
            <property name="closingParens" value="never" />
        </module>

        <!--检查@deprecated 注解与  @deprecated javaDoc是否同时存在
            skipNoJavadoc 没有javadoc跳过  defalult  false
        -->
        <module name="MissingDeprecated">
            <property name="skipNoJavadoc" value="false" />
        </module>

        <!--检查{@inheritDoc} javadoc存在时 @Override 注解是否存在
            javaFiveCompatibility 是否只检查 类，接口  default false
         -->
        <module name="MissingOverride">
            <property name="javaFiveCompatibility" value="true" />
        </module>

        <!--确保包注解只存在 package-info.java 中-->
        <module name="PackageAnnotation" />

        <!--检查不能忽略的警告注释
            format 注解元素的格式
            tokens 适用范围
        -->
        <module name="SuppressWarnings">
            <property name="format" value="^unchecked$|^unused$" />
            <property name="tokens" value="
          CLASS_DEF,INTERFACE_DEF,ENUM_DEF,
          ANNOTATION_DEF,ANNOTATION_FIELD_DEF,
          ENUM_CONSTANT_DEF,METHOD_DEF,CTOR_DEF
          " />
        </module>

```
 *  代码块检查

```
        <!--避免嵌套的代码块，如

                     public void guessTheOutput(){
                         int whichIsWhich = 0;
                         {
                              int whichIsWhich = 2;
                         }
                         System.out.println("value = " + whichIsWhich);
                     }

            allowInSwitchCase  是允许switch-case中存在嵌套代码块，如

                    switch (a){
                        case 0:
                            // Never OK, break outside block
                            {
                                x = 1;
                            }
                            break;
                        case 1:
                            // Never OK, statement outside block
                            System.out.println("Hello");
                            {
                                x = 2;
                                break;
                            }
                        case 1:
                            // OK if allowInSwitchCase is true
                            {
                                System.out.println("Hello");
                                x = 2;
                                break;
                            }
                    }
        -->
        <module name="AvoidNestedBlocks">
            <property name="allowInSwitchCase" value="false" />
        </module>

        <!--检查空白块
            option  选项 text，stmt     default stmt
        -->
        <module name="EmptyBlock">
            <property name="option" value="stmt" />
        </module>

        <!--检查空的 catch块-->
        <module name="EmptyCatchBlock" />

        <!--检查 { 存在的位置
           option 选项 same,nl,nlow    defaule eol
        -->
        <module name="LeftCurly">
            <property name="option" value="nlow" />
        </module>

        <!--检查 } 存在的位置
          option 选项 eol,alone,alone_or_singleline    defaule same
       -->
        <module name="RightCurly" />

        <!--检查代码块是否需要 {} 如 if(){},while(){}
        -->
        <module name="NeedBraces" />

```

 * 类检查

```
        <!--final 类检查，当类的构造方法为 private 时-->
        <module name="FinalClass" />

        <!--工具类检查，确保没有 public 构造方法-->
        <module name="HideUtilityClassConstructor" />

        <!--内部类检查，确保内部类在外部类的底部-->
        <module name="InnerTypeLast" />

        <!--内部接口检查，确保接口不是空的-->
        <module name="InterfaceIsType" />

        <!--异常类检查，确保异常类的只含有 final 成员-->
        <module name="MutableException"/>

        <!--检查每个java文件是否只含有一个顶层类-->
        <module name="OneTopLevelClass" />

        <!--检查每个类抛出的异常个数，默认最多4个-->
        <module name="ThrowsCount" />

        <!--类成员访问控制检查-->
        <module name="VisibilityModifier">
            <property name="packageAllowed" value="true" />
            <property name="publicMemberPattern" value="^$" />
            <property name="allowPublicImmutableFields" value="true" />
            <property name="ignoreAnnotationCanonicalNames" value="Inject,Bind" />
        </module>

```

 * 代码检查

```
        <!--数组初始化末尾逗号检查-->
        <module name="ArrayTrailingComma" />

        <!--三元表达式检查-->
        <module name="AvoidInlineConditionals"/>

        <!--共变 equals 检查，重载 equals() 方法时 也必须重写 它-->
        <module name="CovariantEquals" />

        <!--声明顺序检查 ，顺序
            1，类变量，实例变量：  public ——> protected ——>default ——>private
            2,类方法：  构造方法 ——>其他方法
        -->
        <module name="DeclarationOrder" />

        <!--switch 中default的位置检查，在所有case下面-->
        <module name="DefaultComesLast" />

        <!--空行检查  一行 ; -->
        <module name="EmptyStatement" />

        <!--使用 equals 时 避免出现null 如  str.equls("xx") 正确使用 "xx".equls(str) -->
        <module name="EqualsAvoidNull" />

        <!--检查 重写equals方法时是否有重写hashCode方法-->
        <module name="EqualsHashCode" />

        <!--明确初始化检查-->
        <module name="ExplicitInitialization" />

        <!--检查 switch 语句 中 case 代码块是否包含 break, return, throw or continue-->
        <module name="FallThrough" />

        <!--检查 未使用或未改变的局部变量或参数 是否定义为 final-->
        <module name="FinalLocalVariable" />

        <!--检查局部变量，参数 是否跟全局变量同名
            ignoreConstructorParameter 忽略构造方法的参数 default false
            ignoreSetter 忽略setter default false
            setterCanReturnItsClass 忽略返回 this 类型的setter default false
        -->
        <module name="HiddenField">
            <property name="ignoreConstructorParameter" value="true" />
            <property name="ignoreSetter" value="true" />
            <property name="setterCanReturnItsClass" value="true" />
        </module>

        <!--检查是否捕获了不应该捕获的异常-->
        <module name="IllegalCatch" />

        <!--检查不合法的实例 某些类有工厂方法来生成实例，比如 Boolean ，就不要 new -->
        <module name="IllegalInstantiation">
            <property name="classes" value="java.lang.Boolean" />
        </module>

        <!--检查非法的抛出声明，比如 Error，RuntimeException-->
        <module name="IllegalThrows" />

        <!--非法标签检查-->
        <module name="IllegalToken" />

        <!--非法标记文本检查
            tokens 类型
            format 匹配正则
        -->
        <module name="IllegalTokenText">
        <property name="tokens" value="NUM_INT,NUM_LONG"/>
        <property name="format" value="^0[^lx]"/>
        <property name="ignoreCase" value="true"/>
        </module>

        <!--非法类型检查 是否有不应声明的类型被使用-->
        <module name="IllegalType" />

        <!--检查繁重内部逻辑，比如 while ((line = bufferedReader.readLine()) != null) ，难以阅读的代码-->
        <module name="InnerAssignment" />

        <!--魔法数检查-->
        <module name="MagicNumber"/>

        <!--检查是否有构造器-->
        <module name="MissingCtor" />

        <!--检查switch 是否包含 default-->
        <module name="MissingSwitchDefault" />

        <!--循环控制变量不应在代码块中改变  如
             for (int i = 0; i < 1; i++) {
                     i++; //violation
                   }
           skipEnhancedForLoopVariable  是否检查foreach
        -->
        <module name="ModifiedControlVariable">
            <property name="skipEnhancedForLoopVariable" value="true" />
        </module>

        <!--检查同个文件中是否出现多个同样的字面量-->
        <module name="MultipleStringLiterals" />

        <!--检查是否同时声明了多个变量  如 Button b1,b2,b3; -->
        <module name="MultipleVariableDeclarations" />

        <!--for 语句嵌套检查  默认嵌套1层-->
        <module name="NestedForDepth" />

        <!--if 语句嵌套检查  默认嵌套1层-->
        <module name="NestedIfDepth" />

        <!--try 语句嵌套检查  默认嵌套1层-->
        <module name="NestedTryDepth" />

        <!--clone 方法重写检查-->
        <module name="NoClone" />

        <!--finalize 方法重写检查-->
        <module name="NoFinalizer" />

        <!--一行只允许一条语句，即末尾一个；-->
        <module name="OneStatementPerLine" />

        <!--重载方法是否在同个地方-->
        <module name="OverloadMethodsDeclarationOrder" />

        <!--包声明检查-->
        <module name="PackageDeclaration" />

        <!--参数中不允许有语句-->
        <module name="ParameterAssignment" />

        <!--this 检查 局部变量和全局变量同名时，全局变量的this.不能少-->
        <module name="RequireThis" />

        <!--return 次数检查 ，默认最多2次-->
        <module name="ReturnCount"/>

        <!--boolean 检查， 如 if (b == true), b || true, !false 糟糕代码-->
        <module name="SimplifyBooleanExpression" />

        <!--boolean 返回值检查  如
            if (valid())
               return false;
            else
               return true;
        -->
        <module name="SimplifyBooleanReturn" />

        <!--字符串比较检查，字符串比较用equals-->
        <module name="StringLiteralEquality" />

        <!--检查 重写的 clone 方法是否调用了 super.clone()-->
        <module name="SuperClone" />

        <!--检查 重写的 finalize 方法是否调用了 super.finalize()-->
        <module name="SuperFinalize" />

        <!--检查无语的圆括号-->
        <module name="UnnecessaryParentheses" />

        <!--变量定义与使用之间的距离  默认3行代码-->
        <module name="VariableDeclarationUsageDistance"/>

        <!-- 每行字符数 -->
        <module name="LineLength">
            <property name="max" value="200" />
        </module>

        <!-- 方法长度 max default 150行 -->
        <module name="MethodLength">
            <property name="tokens" value="METHOD_DEF" />
        </module>
        <!-- 构造方法长度 max default 150行 -->
        <module name="MethodLength">
            <property name="tokens" value="CTOR_DEF" />
            <property name="max" value="60" />
        </module>

        <!-- ModifierOrder 检查修饰符的顺序，默认是 public,protected,private,abstract,static,final,transient,volatile,synchronized,native -->
        <module name="ModifierOrder" />

        <!-- 检查是否有多余的修饰符，例如：接口中的方法不必使用public、abstract修饰  -->
        <module name="RedundantModifier" />

        <!-- Checks the number of parameters of a method or constructor. max default 7个. -->
        <module name="ParameterNumber">
            <property name="max" value="19" />
        </module>

```

 * 文件头检查

```
        <!--检查文件的头部
            headerFile  指向存放头的文件
            ignoreLines  忽略行
            fileExtensions  扩展名
            header  直接头类容而不是headerFile
        -->
        <module name="Header">
        <property name="headerFile" value="config/java.header"/>
        <property name="ignoreLines" value="2, 3, 4"/>
        <property name="fileExtensions" value="java"/>
        </module>

        <!--用正则来匹配的文件头-->
        <module name="RegexpHeader">
        <property name="headerFile" value="config/java.header"/>
        <property name="multiLines" value="10, 13"/>
        </module>

```

 * 导包检查

```
        <!-- 必须导入类的完整路径，即不能使用*导入所需的类 -->
        <module name="AvoidStarImport" />

        <!--避免静态导入 如 static import xxx-->
        <module name="AvoidStaticImport" />

        <!--导包顺序检查-->
        <module name="ImportOrder">
        <property name="groups" value="/^java\./,javax,org,com"/>
        <property name="ordered" value="true"/>
        <property name="separated" value="true"/>
        <property name="option" value="above"/>
        <property name="sortStaticImportsAlphabetically" value="true"/>
        </module>

        <!--检查包的导入顺序是否跟我们定义的一样-->
        <module name="CustomImportOrder">
        <property name="customImportOrderRules"
        value="STATIC###STANDARD_JAVA_PACKAGE###SPECIAL_IMPORTS###THIRD_PARTY_PACKAGE"/>
        <property name="specialImportsRegExp" value="^org\."/>
        <property name="thirdPartyPackageRegExp" value="^com\."/>
        <property name="sortImportsInGroupAlphabetically" value="true"/>
        <property name="separateLineBetweenGroups" value="true"/>
        </module>

        <!-- 检查是否从非法的包中导入了类 illegalPkgs: 定义非法的包名称-->
        <module name="IllegalImport" />

        <!--导包控制检查-->
        <module name="ImportControl">
        <property name="file" value="config/import-control.xml"/>
        </module>

        <!-- 检查是否导入了不必显示导入的类-->
        <module name="RedundantImport" />

        <!-- 检查是否导入的包没有使用-->
        <module name="UnusedImports" />

        <!--**************       javadoc注释检查        ××******************** -->

        <!--@语句的顺序-->
        <module name="AtclauseOrder">
            <property name="tagOrder" value="@author, @version, @param,
                           @return, @throws, @exception, @see, @since, @serial,
                           @serialField, @serialData, @deprecated" />
            <property name="target" value="CLASS_DEF, INTERFACE_DEF, ENUM_DEF,
                           METHOD_DEF, CTOR_DEF, VARIABLE_DEF" />
        </module>

        <!-- 检查方法的javadoc的注释
                    scope: 可以检查的方法的范围，例如：public只能检查public修饰的方法，private可以检查所有的方法
                    allowMissingParamTags: 是否忽略对参数注释的检查
                    allowMissingThrowsTags: 是否忽略对throws注释的检查
                    allowMissingReturnTag: 是否忽略对return注释的检查 -->
        <module name="JavadocMethod">
            <property name="scope" value="private" />
            <property name="allowMissingParamTags" value="false" />
            <property name="allowMissingThrowsTags" value="false" />
            <property name="allowMissingReturnTag" value="false" />
            <property name="tokens" value="METHOD_DEF" />
            <property name="allowUndeclaredRTE" value="true" />
            <property name="allowThrowsTagsForSubclasses" value="true" />
            <!--允许get set 方法没有注释-->
            <property name="allowMissingPropertyJavadoc" value="true" />
        </module>


        <!--javadoc 段落检查-->
        <module name="JavadocParagraph" />

        <!--javadoc 风格-->
        <module name="JavadocStyle" />

        <!--javadoc 标签缩进  默认4空格-->
        <module name="JavadocTagContinuationIndentation" />

        <!-- 检查类和接口的javadoc 默认不检查author 和version tags
                 authorFormat: 检查author标签的格式
                       versionFormat: 检查version标签的格式
                       scope: 可以检查的类的范围，例如：public只能检查public修饰的类，private可以检查所有的类
                       excludeScope: 不能检查的类的范围，例如：public，public的类将不被检查，但访问权限小于public的类仍然会检查，其他的权限以此类推
                       tokens: 该属性适用的类型，例如：CLASS_DEF,INTERFACE_DEF -->
        <module name="JavadocType">
            <property name="authorFormat" value="\S" />
            <property name="scope" value="protected" />
            <property name="tokens" value="CLASS_DEF,INTERFACE_DEF" />
        </module>

        <!-- 检查类变量的注释
               scope: 检查变量的范围，例如：public只能检查public修饰的变量，private可以检查所有的变量 -->
        <module name="JavadocVariable">
            <property name="scope" value="private" />
        </module>

        <!--@ 后面有对 @ 的描述-->
        <module name="NonEmptyAtclauseDescription" />

        <!--含@ 的javadoc 必须多行，不含则可一行显示-->
        <module name="SingleLineJavadoc" />

        <!--检查是否包含不允许使用的词汇-->
        <module name="SummaryJavadocCheck" />

        <!--自定义 javadoc 标签的检查-->
        <module name="WriteTag">
        <property name="tag" value="@incomplete"/>
        <property name="tagFormat" value="\S"/>
        <property name="severity" value="ignore"/>
        <property name="tagSeverity" value="warning"/>
        </module>


```

 * 度量检查

```
        <!--检查一个表达式中  &&, ||, &, | ， ^ 的最大允许个数-->
        <module name="BooleanExpressionComplexity">
            <property name="max" value="7" />
        </module>

        <!--类数据抽象耦合检查，即一个类成员变量类型包含多少种其他类类型，
            不包括继承来的，不包括基本类型，字符串类型  默认最多7 -->
        <module name="ClassDataAbstractionCoupling" />


        <!--类的分散复杂度检查，类依赖其他类的数量 default 20-->
        <module name="ClassFanOutComplexity" />

        <!--圈复杂度检查，即执行到最深层代码需要判断的条件数，多见于多层嵌套的if-else ，default 10-->
        <module name="CyclomaticComplexity" />

        <!--方法，类，文件的行数限制检查
            methodMaximum 方法最大行数  default 50
            classMaximum 类最大行数  default 1500
            fileMaximum 文件最大行数  default 2000
        -->
          <module name="JavaNCSS">
                    <property name="methodMaximum" value="50" />
                </module>

                <!--方法分之复杂度，一个方法可能的执行路径数 default 200-->
                <module name="NPathComplexity" />

```

 * 命名规范检查

```
        <!--名字简写检查，不能出现连续的大写字母个数 default 3
            不包括 static ，final，重写方法-->
        <module name="AbbreviationAsWordInName" />

        <!--抽象类名-->
        <module name="AbstractClassName"/>

        <!--catch 参数名称-->
        <module name="CatchParameterName"/>

        <!--类类型参数名-->
        <module name="ClassTypeParameterName"/>

        <!-- local, final variables, including catch parameters -->
        <module name="LocalFinalVariableName" />

        <!-- local, non-final variables, including catch parameters-->
        <module name="LocalVariableName" />

        <!-- 静态变量命名  不能有小写字母，长度(0,29) -->
        <module name="StaticVariableName">
            <property name="format" value="(^[A-Z0-9_]{0,29}$)" />
        </module>

        <!-- 包命名  小写字母开头 -->
        <module name="PackageName">
            <property name="format" value="^[a-z]+(\.[a-z][a-z0-9]*)*$" />
        </module>

        <!-- 类，接口命名  大写字母开头，长度(0,29) -->
        <module name="TypeName">
            <property name="format" value="(^[A-Z][a-zA-Z0-9]{0,29}$)" />
        </module>

        <!-- 方法命名  小写字母开头，长度(0,29) -->
        <module name="MethodName">
            <property name="format" value="(^[a-z][a-zA-Z0-9]{0,29}$)" />
        </module>

        <!-- 成员变量命名  小写字母开头，长度(0,29) -->
        <module name="MemberName">
            <property name="format" value="(^[a-z][a-zA-Z0-9]{0,29}$)" />
        </module>

        <!-- 参数命名  小写字母开头，长度(0,29) -->
        <module name="ParameterName">
            <property name="format" value="(^[a-z][a-zA-Z0-9_]{0,29}$)" />
        </module>

        <!-- 常量命名  不能有小写字母，长度(0,29) -->
        <module name="ConstantName">
            <property name="format" value="(^[A-Z0-9_]{0,29}$)" />
        </module>

        <!-- 代码缩进   -->
        <module name="Indentation" />


```

 * 其他检查...
```

```
------

# 三. Checkstyle的具体使用

* 安装CheckStyle-IDEA 插件

  ![这里写图片描述](http://img.blog.csdn.net/20170606110829508?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* 添加CheckStyle 配置文件

  ![这里写图片描述](http://img.blog.csdn.net/20170606111013593?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
* 代码检查中勾选CheckStyle

  ![这里写图片描述](http://img.blog.csdn.net/20170606111102011?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
* 进行代码检查

  * 通过checkstyle功能区

    ![这里写图片描述](http://img.blog.csdn.net/20170606111147090?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  * 通过右键检查当前文件

    ![这里写图片描述](http://img.blog.csdn.net/20170606111221029?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  * 通过gradle任务

    ![这里写图片描述](http://img.blog.csdn.net/20170606111242970?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbmFpdm9y/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

------

以上总结可能还有很多不到位的地方，欢迎大家指正和交流。

去Checkstyle网站查看 [更多类容 ](http://checkstyle.sourceforge.net/index.html)