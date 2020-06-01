---
title: TestNG常见注解
date: 2020-05-20 11:30:33
tags: TestNG
---

TestNG是一个开源的自动化测试框架, NG表示Next Generation。TestNG类似于JUnit（尤其是JUnit4），但它不是JUnit的扩展。它的灵感来自JUnit但是设计优于JUnit，尤其是在测试集成类时。TestNG有强大的注解功能，下面将利用小demo来展示常用的注解及用法。
<!--more-->

**@Test：** 最基本的注解，用来把方法标记为测试的一部分

**@BeforeSuite：** 对于套件测试，在此套件中的所有测试执行之前运行，仅运行一次
**@AfterSuite：** 对于套件测试，在此套件中的所有测试执行之后运行，仅运行一次
例子:  
{% codeblock lang:command %}
public class AnnotationSuite {
    @Test
    public void test1() {System.out.print("test1运行");}

    @Test
    public void test2() {System.out.print("test2运行");}

    @Test
    public void test3(){System.out.print("test3运行");}

    @BeforeSuite
    public void beforeSuite() {System.out.println("BeforeSuite运行");}

    @AfterSuite
    public void afterSuite(){System.out.println("AfterSuite运行");}
}  

{% endcodeblock %}
对应的XML文件如下: 
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testSuite">
    <test name="annotation1">
        <classes>
            <class name="org.example.annotation.AnnotationSuite">
                <methods>
                </methods>
            </class>
        </classes>
    </test>
</suite>
{% endcodeblock %}  

结果为：
{% codeblock lang:command %}
BeforeSuite运行
test1运行
test2运行
test3运行
AfterSuite运行
{% endcodeblock %}

**@BeforeTest：** 对于套件测试，在属于<test>标签内的所有类的测试方法执行之前运行
**@AfterTest：** 对于套件测试，在属于<test>标签内的所有类的测试方法都已执行完之后运行  
例子：   
{% codeblock lang:command %}
public class AnnotationTest {

    @Test
    public void test1() {System.out.print("test1运行");}

    @Test
    public void test2() {System.out.print("test2运行");}

    @Test
    public void test3(){System.out.print("test3运行");}

    @BeforeTest
    public void beforeTest(){System.out.println("BeforeTest运行");}

    @AfterTest
    public void afterTest(){ System.out.println("AfterTest运行");}
{% endcodeblock %}   

对应的XML文件为  

{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testTest">
    <test name="annotationTest1">
        <classes>
            <class name="org.example.annotation.AnnotationTest">
                <methods>
                    <include name="test1"/>
                    <include name="test3"/>
                </methods>
            </class>
        </classes>
    </test>

        <test name="annotationTest2">
            <classes>
                <class name="org.example.annotation.AnnotationTest">
                    <methods>
                        <include name="test2"/>
                    </methods>
                </class>
            </classes>
        </test>
</suite>

{% endcodeblock %}   

运行结果为:  
{% codeblock lang:command %}
BeforeTest运行
test1运行
test3运行
AfterTest运行

BeforeTest运行
test2运行
AfterTest运行
{% endcodeblock %}   

注意：从运行结果可以看到@BeforeTest和@AfterTest针对的是xml文件中的test标签来决定是否运行，而不是测试用例上面的@test。因此如果有多个测试用例，但在xml文件中只有一个test标签，那么@BeforeTest和@AfterTest也只分别运行一次。

**@BeforeClass：** 在调用当前类之前运行  
**@AfterClass：** 在调用当前类之后运行  
例子：  
{% codeblock lang:command %}
ublic class AnnotationClass {

    @Test
    public void test1() {System.out.print("test1运行");}

    @Test
    public void test2() {System.out.print("test2运行");}

    @Test
    public void test3(){System.out.print("test3运行");}

    @BeforeClass
    public void beforeClass(){System.out.println("BeforeClass运行");}

    @AfterClass
    public void afterClass(){System.out.println("AfterClass运行");}
}
{% endcodeblock %}  

对应的XML文件为：
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testClass">
    <test name="annotationClass1">
        <classes>
            <class name="org.example.annotation.AnnotationClass">
                <methods>
                </methods>
            </class>
        </classes>
    </test>
</suite>  

{% endcodeblock %}  

运行结果：  
{% codeblock lang:command %}
BeforeClass运行
test1运行
test2运行
test3运行
AfterClass运行  
{% endcodeblock %} 

**@BeforeMethod：** 在每个测试方法执行之前都会运行  
**@AfterMethod：** 在每个测试方法执行之后都会运行      
例子：  
{% codeblock lang:command %}
public class AnnotationMethod {

    @Test
    public void test1() { System.out.print("test1运行");}

    @Test
    public void test2() {System.out.print("test2运行");}

    @Test
    public void test3(){System.out.print("test3运行");}

    @BeforeMethod
    public void beforeMethod(){System.out.println("BeforeMethod运行");}

    @AfterMethod
    public void afterMethod(){System.out.println("AfterMethod运行");}
}

{% endcodeblock %}
对应的XML文件为：
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testMethod">
    <test name="annotationTest1">
        <classes>
            <class name="org.example.annotation.AnnotationMethod">
                <methods>
                </methods>
            </class>
        </classes>
    </test>
</suite>
{% endcodeblock %}
运行结果：   
{% codeblock lang:command %}
BeforeMethod运行
test1运行
AfterMethod运行

BeforeMethod运行
test2运行
AfterMethod运行

BeforeMethod运行
test3运行
AfterMethod运行
{% endcodeblock %}


**@BeforeGroups：** 在调用属于该组的第一个测试方法之前运行  
**@AfterGroups：** 在调用属于该组的最后一个测试方法执行之后运行  
例子：   
{% codeblock lang:command %}
public class AnnotationGroups {

    @Test(groups = { "group1", "group2" })
    public void test1() {System.out.println("test1");
    }

    @Test(groups = {"group1", "group2"} )
    public void test2() {System.out.println("test2");
    }

    @Test(groups = { "group1" })
    public void test3() {System.out.println("test3");
    }

    @BeforeGroups("group1")
    public void beforeGroups(){System.out.println("BeforeGroups");}

    @AfterGroups("group2")
    public void afterGroups(){System.out.println("AfterGroups");}
}
{% endcodeblock %}  

对应的XML文件为：  
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testGroup">
    <test name="group">
        <groups>
            <run>
                <include name="group1" />
                <include name="group2" />
            </run>
        </groups>
        <classes>
            <class name="org.example.annotation.AnnotationGroups">
            </class>
        </classes>
    </test>
</suite>

{% endcodeblock %}  
运行结果：  
{% codeblock lang:command %}
BeforeGroups
test1
test2
AfterGroups

test3
{% endcodeblock %}


**@Parameters：** 描述如何将参数传递给@Test方法。  
例子：   
{% codeblock lang:command %}
public class testParameter {
    @Test
    @Parameters(value = "str1")
    public void test1(String str){System.out.println(str);}
}
{% endcodeblock %}
对应的XML文件为： 
{% codeblock lang:command %}
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="testParameter">
    <test name="login">
        <parameter name="str1" value="通过@test下面的@Paremeter传参数"></parameter>
        <classes>
            <class name="org.example.parameter.testParameter">
            </class>
        </classes>
    </test>
</suite>
{% endcodeblock %}
运行结果：  
{% codeblock lang:command %}
通过@test下面的@Paremeter传参数
{% endcodeblock %}

当参数类型较为复杂时，上述的传参方法无法满足需要，此时可以利用@DataProvider传参。该方法会返回一个Object二维数组或一个Iterator<Object[]>来提供复杂的参数对象。Object[][]这个二维数组的每一个一维数组代表一个对象。这个一维数组中的元素类型，个数必须和使用这个provider的函数的参数匹配。使用这个Provider的Method会遍历Provider传递过来的每一个对象，每个对象都执行一次方法体内的代码。所以Object[][]第一个参数指定了@Test标识的test method被执行的次数。  

**@DataProvider：** 标记一种方法来提供测试方法的数据。 注释方法必须返回一个Object [] []，其中每个Object []可以被分配给测试方法的参数列表。  
例子：  
{% codeblock lang:command %}
public class testDataProvider {
    @DataProvider(name = "inDp")
    public Object[][] createData1() {
        return new Object[][] {
                { "通过内部DataProvider传参1", new Integer(36) },
                { "通过内部DataProvider传参2", new Integer(37)},
        };
    }
    @Test(dataProvider = "inDp")
    public void test3(String n1, Integer n2) {
        System.out.println(n1 + " " + n2);
    }
}
{% endcodeblock %}   

运行结果：  
{% codeblock lang:command %}
通过内部DataProvider传参1 36
通过内部DataProvider传参2 37
{% endcodeblock %}
 
如果测试用例和DataProvider不在同一个类中，那么需要将返回参数值的函数标记为static。    
例子： 
{% codeblock lang:command %}
public class StaticProvider {
    @DataProvider(name = "outDp")
    public static Object[][] createData() {
        return new Object[][] {
                { "通过外部DataProvider传参", new Integer("1") },
                {"通过外部DataProvider传参", new Integer("2")},
        };
    }
}

调用过程为：
@Test(dataProvider = "outDp", dataProviderClass = StaticProvider.class)
    public void test4(String str1,Integer i)
    {
        System.out.println(str1 + " " + i);
    }
{% endcodeblock %}  
  
运行结果： 
{% codeblock lang:command %}
通过外部DataProvider传参 1
通过外部DataProvider传参 2
{% endcodeblock %} 
除此之外还可以使用迭代器进行传参数，例子如下：  
{% codeblock lang:command %}
 @DataProvider(name = "iteratorDataProvider")
    public Iterator<Object[]> iteratorDataProvider(){
        List<Object[]> objList = new ArrayList<Object[]>();
        for(int i = 1; i < 5; i++){
            objList.add(new Object[]{"第" + i  + "名得分", new Integer(i) ,"哈哈"});
        }
        return objList.iterator();
    }
////
    @Test(dataProvider = "iteratorDataProvider")
    public void testDataProvider(String str, Integer i, String str2){
        System.out.println(str+i+str2);
    }
{% endcodeblock %}
运行结果：  
{% codeblock lang:command %}
第1名得分1哈哈
第2名得分2哈哈
第3名得分3哈哈
第4名得分4哈哈
{% endcodeblock %}  
因此使用DataProvider传递参数时，返回的参数个数和类型应与调用这个provider的函数所需要的参数个数和类型完全匹配。

以上便是TestNGINX常用的一些注解，后面若学习到新的注解会继续补充~