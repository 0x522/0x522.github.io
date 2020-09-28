# 正则表达式


# 正则表达式

## 为什么要学习正则表达式

- 可以在支持正则表达式的文本编辑器中提高工作效率
- 用很少的代码完成复杂的文本提取工作

## 什么是正则表达式

**正则表达式用于描述文本或字符串的一组规则。**

## 正则表达式的作用

- 处理文本
- 提取信息

## Java 中的转义字符

- 为什么需要转义字符<br>
  因为字符不可见,无法用一个键来表示。
- 常用的转义字符
  - \n 换行:使光标到行首
  - \r 回车:使光标从当前位置下移一行
  - \t 制表符:一般是四个空格
  - \\
  - \” \’

## 正则表达式中的字符

**用到以下字符原来的意思的时候需要转义**

### **常用元字符**

| 符号 | 含义                                                                                       |
| ---- | ------------------------------------------------------------------------------------------ |
| ^    | 起始位置                                                                                   |
| \$   | 结束位置                                                                                   |
| .    | 单个任意字符(不一定包含换行符)                                                             |
| \w   | 单个字符(字母数字下划线)                                                                   |
| \s   | 单个空白字符(\n\r\t)                                                                       |
| \d   | 单个数字字符                                                                               |
| \b   | 单词的开始或结束：表示所在位置的一侧为单词字符，另一侧为非单词字符、字符串的开始或结束位置 |

### 重复

| 符号  | 含义       |
| ----- | ---------- |
| \*    | 0 次或多次 |
| +     | 一次或多次 |
| ?     | 0 次或多次 |
| {n}   | n 次       |
| {n,}  | >=n 次     |
| {n,m} | n 到 m 次  |

### 选择

| 符号                | 含义                                          |
| ------------------- | --------------------------------------------- |
| [aeiou]             | 单个 aeiou 中的字符之一：从五个字母里选择一个 |
| [0-9]               | 单个数字字符                                  |
| [A-Z]               | 单个大写字母                                  |
| [A-Z0-9]            | 大写字母或者数字或者下划线                    |
| Hi\|hi 等价于 [Hh]i | Hi 或者 hi                                    |

### 反义

| 符号     | 含义                                   |
| -------- | -------------------------------------- |
| [^aeiou] | 单个除 aeiou 之外的字符                |
| [^a]     | 单个除了 A 之外的字符                  |
| \W       | 单个非\w(字母数字下划线汉字)之外的字符 |
| \S       | 单个非\s(空白)                         |
| \D       | 单个非\d(数字字符)                     |
| \B       | 非开头和结束位置                       |

## 一些实例

- ^\d{5,12}\$ 5 到 12 位的数字
- ^(0|[1-9][0-9]\*)\$ 0 或者⾮零开头的数字
- ^(-?\d+)(\.\d+)?\$ ⼩数
- \d{3}-\d{8}|\d{4}-\d{7,8} 国内的电话号码
- ^\d{4}-\d{1,2}-\d{1,2} yyyy-MM-dd ⽇期格式
- \n\s\*\r 空⽩⾏

## Java 中的正则表达式

- String
  - `split()`
  - `replaceAll()`/`replaceFirst()`
  - `matches()`
    - 尽量少用或者少编译因为效率低下。
    - Java 中正则表达式是非常昂贵的,正则表达式编译的源码非常复杂。
      - 正则表达式需要解析。
      - 匹配过程是靠"回溯"来匹配的，所以效率低下。

### 实战

**电话号码匹配**

```java
public class PhoneNumberMatcher {
    // 请编写一个函数，判断一个字符串是不是合法的固定电话号码
    // 合法的固定电话号码为：区号-号码
    // 123-45678901 区号必须以0开头
    // 021-1234567 三位区号后面只能跟八位电话号码
      public static boolean isPhoneNumber(String str) {
        Pattern telNumberPattern = Pattern.compile("[0]\\d{2}-[^0]\\d{7}|[0]\\d{3}-[^0]\\d{6,7}");
        return telNumberPattern.matcher(str).find();
    }
}
```

## 分组与捕获

- 想要将所有符合正则表达式的⽂本抓出来处理。
  - 使用括号的来指定一个被捕获的分组。
  - 分组的编号从 1 开始，group(0)表示的是完整匹配正则表达式的字符串,group(1)表示的是匹配正则的第一个分组。
  - 分组的编号只看左括号。
  - (?:)不捕获和分配编号，括号只⽤于分组或标记优先级。

### 在 Java 中处理捕获

- Pattern
  - `matcher()`
- Matcher
  - `find()`
  - `group()`

### 使用正则表达式处理 GClog

```java
public class GCLogAnalyzer {
    // 在本项目的根目录下有一个gc.log文件，是JVM的GC日志
    // 请从中提取GC活动的信息，每行提取出一个GCActivity对象
    // 2019-08-21T07:48:17.401+0200: 2.924: [GC (Allocation Failure) [PSYoungGen:
    // 393216K->6384K(458752K)] 416282K->29459K(1507328K), 0.0051622 secs] [Times: user=0.02
    // sys=0.00, real=0.01 secs]
    // 例如，对于上面这行GC日志，
    // [PSYoungGen: 393216K->6384K(458752K)] 代表JVM的年轻代总内存为458752，经过GC后已用内存从393216下降到了6384
    // 416282K->29459K(1507328K) 代表JVM总堆内存1507328，经过GC后已用内存从416282下降到了29459
    // user=0.02 sys=0.00, real=0.01 分别代表用户态消耗的时间、系统调用消耗的时间和物理世界真实流逝的时间
    // 请将这些信息解析成一个GCActivity类的实例
    // 如果某行中不包含这些数据，请直接忽略该行
    public static List<GCActivity> parse(File gcLog) throws IOException {
        List<GCActivity> list = new ArrayList<>();
        String youngGenRegex = "\\[PSYoungGen:[ ](\\d+)K->(\\d+)K\\((\\d+)K\\)\\]";
        String jvmRegex = "[ ](\\d+)K->(\\d+)K\\((\\d+)K\\),";
        String timeRegex = "\\[Times:[ ]user=(-?\\d+\\.\\d+?)[ ]sys=(-?\\d+\\.\\d+?),[ ]real=(-?\\d+\\.\\d+?)[ ]secs\\]";
        Pattern youngGenGC = Pattern.compile(youngGenRegex);
        Pattern jvmGC = Pattern.compile(jvmRegex);
        Pattern times = Pattern.compile(timeRegex);
        BufferedReader bufferedReader = new BufferedReader(new FileReader(gcLog));
        String str = null;
        while ((str = bufferedReader.readLine()) != null) {
            Matcher matcherYoungGen = youngGenGC.matcher(str);
            Matcher matcherJvm = jvmGC.matcher(str);
            Matcher matcherTimes = times.matcher(str);
            while (matcherYoungGen.find() && matcherJvm.find() && matcherTimes.find()) {
                list.add(new GCActivity(
                        Integer.parseInt(matcherYoungGen.group(1)),
                        Integer.parseInt(matcherYoungGen.group(2)),
                        Integer.parseInt(matcherYoungGen.group(3)),
                        Integer.parseInt(matcherJvm.group(1)),
                        Integer.parseInt(matcherJvm.group(2)),
                        Integer.parseInt(matcherJvm.group(3)),
                        Double.parseDouble(matcherTimes.group(1)),
                        Double.parseDouble(matcherTimes.group(2)),
                        Double.parseDouble(matcherTimes.group(3))
                ));
            }
        }
        return list;
    }

    public static void main(String[] args) throws IOException {
        List<GCActivity> activities = parse(new File("gc.log"));
        activities.forEach(System.out::println);
    }
        public static class GCActivity {
        // 年轻代GC前内存占用，单位K
        int youngGenBefore;
        // 年轻代GC后内存占用，单位K
        int youngGenAfter;
        // 年轻代总内存，单位K
        int youngGenTotal;
        // JVM堆GC前内存占用，单位K
        int heapBefore;
        // JVM堆GC后内存占用，单位K
        int heapAfter;
        // JVM堆总内存，单位K
        int heapTotal;
        // 用户态时间
        double user;
        // 系统调用消耗时间
        double sys;
        // 物理世界流逝的时间
        double real;

    public GCActivity(
                int youngGenBefore,
                int youngGenAfter,
                int youngGenTotal,
                int heapBefore,
                int heapAfter,
                int heapTotal,
                double user,
                double sys,
                double real) {
            this.youngGenBefore = youngGenBefore;
            this.youngGenAfter = youngGenAfter;
            this.youngGenTotal = youngGenTotal;
            this.heapBefore = heapBefore;
            this.heapAfter = heapAfter;
            this.heapTotal = heapTotal;
            this.user = user;
            this.sys = sys;
            this.real = real;
        }

        @Override
        public String toString() {
            return "GCActivity{"
                    + "youngGenBefore="
                    + youngGenBefore
                    + ", youngGenAfter="
                    + youngGenAfter
                    + ", youngGenTotal="
                    + youngGenTotal
                    + ", heapBefore="
                    + heapBefore
                    + ", heapAfter="
                    + heapAfter
                    + ", heapTotal="
                    + heapTotal
                    + ", user="
                    + user
                    + ", sys="
                    + sys
                    + ", real="
                    + real
                    + '}';
        }

        public int getYoungGenBefore() {
            return youngGenBefore;
        }

        public int getYoungGenAfter() {
            return youngGenAfter;
        }

        public int getYoungGenTotal() {
            return youngGenTotal;
        }

        public int getHeapBefore() {
            return heapBefore;
        }

        public int getHeapAfter() {
            return heapAfter;
        }

        public int getHeapTotal() {
            return heapTotal;
        }

        public double getUser() {
            return user;
        }

        public double getSys() {
            return sys;
        }

        public double getReal() {
            return real;
        }
    }
}
```

**gc.log(省略版)**

```vim
Java HotSpot(TM) 64-Bit Server VM (25.181-b13) for linux-amd64 JRE
(1.8.0_181-b13), built on Jul 7 2018 00:56:38 by "java_re" with gcc 4.3.0
20080428 (Red Hat 4.3.0-8) Memory: 4k page, physical 32707224k(7763808k free),
swap 16777212k(16772860k free) CommandLine flags: -XX:InitialHeapSize=1610612736
-XX:MaxHeapSize=1610612736 -XX:+PrintGC -XX:+PrintGCDateStamps
-XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+UseCompressedClassPointers
-XX:+UseCompressedOops -XX:+UseParallelGC 2019-08-21T07:48:15.609+0200: 1.133:
[GC (Metadata GC Threshold) [PSYoungGen: 283264K->20213K(458752K)]
283264K->20229K(1507328K), 0.0220106 secs] [Times: user=0.07 sys=0.01, real=0.02
secs] 2019-08-21T07:48:15.631+0200: 1.155: [Full GC (Metadata GC Threshold)
[PSYoungGen: 20213K->0K(458752K)] [ParOldGen: 16K->19450K(1048576K)]
20229K->19450K(1507328K), [Metaspace: 20743K->20743K(1067008K)], 0.0461462 secs]
[Times: user=0.16 sys=0.01, real=0.05 secs] 2019-08-21T07:48:16.604+0200: 2.127:
[GC (Metadata GC Threshold) [PSYoungGen: 163389K->16486K(458752K)]
182840K->35944K(1507328K), 0.0129310 secs] [Times: user=0.03 sys=0.01, real=0.01
secs] 2019-08-21T07:48:16.617+0200: 2.140: [Full GC (Metadata GC Threshold)
[PSYoungGen: 16486K->0K(458752K)] [ParOldGen: 19458K->23066K(1048576K)]
35944K->23066K(1507328K), [Metaspace: 34726K->34723K(1079296K)], 0.0324194 secs]
[Times: user=0.09 sys=0.00, real=0.03 secs] 2019-08-21T07:48:17.401+0200: 2.924:
[GC (Allocation Failure) [PSYoungGen: 393216K->6384K(458752K)]
416282K->29459K(1507328K), 0.0051622 secs] [Times: user=0.02 sys=0.00, real=0.01
secs]
```

