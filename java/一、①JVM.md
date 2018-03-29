### 一、JVM

> JVM内存结构

- 堆、栈、方法区、直接内存、堆和栈区别

> Java内存模型

- 内存可见性、重排序、顺序一致性、volatile、锁、final

> 垃圾回收

- 内存分配策略、垃圾收集器（G1）、GC算法、GC参数、对象存活的判定

> JVM参数及调优



> Java对象模型

- oop-klass、对象头

> HotSpot

- 即时编译器、编译优化

> 类加载机制

- classLoader、类加载过程、双亲委派（破坏双亲委派）、模块化（jboss modules、osgl、jigsaw）

> 虚拟机性能监控与故障处理工具

- jps, jstack, jmap、jstat, jconsole, jinfo, jhat, javap, btrace、TProfiler
  - jps：虚拟机进程状况工具
  - jstat：虚拟机统计信息监视工具
  - jinfo：java配置信息工具
  - jmap：java内存映像工具
  - jhat：虚拟机堆转储快照分析工具
  - jstack：java堆栈跟踪工具
  - hsdis：jit生成代码反汇编
  - jconsole：java监视与管理控制台
  - visualvm：多合一故障处理工具