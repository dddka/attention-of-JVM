# attention-of-JVM
# JVM
字节码通过类加载过程加载到,JVM后执行的三种模式：
1.第一,解释执行.第二,JIT 编译执行.第三,JIT编译与解释混合执行(主流 JVM默认执行模式)。
2.混合执行模式的优势在于解释器在启动时先解释执行，省去编译时间。随着时间推进 JVM 通过热点代码统计分析,识别高频的方法调用、循环体、公共模块等，基于强大的JlT动态编译技术，将热点代码转换成机器码，直接交给CPU
执行。JIT 作用是将Java字节码动态地编译成可以直接发送给处理器指令执行的机器码
# heaq的理解
堆分成两大块，新生代和老年代。对象产生之初在新生代 步入暮年时进入老年代，但是老年代也接纳在新生代无法容纳的超大对象。
新生代＝ Eden 区＋ Survivor 区。绝大部分对象在Eden 区生成 Eden 区装填满的时候, 会触发 Young Garbage Collection YGC 。垃圾回收的时候 Eden 区实现清除策略 没有被引 用的对象则直接回收。依然存活的对象会被移送到Survivor 这个区真是名副其实的存在。
Survivor 区分为 so Sl 两块内存空间 送到哪块空间呢？每次 YGC 的时候，将存活的对象复制到未使用的那块空间，然后将当前正在使用的空间完全清除 交换两块空间的使用状态。
如果YGC要移送的对象大于Survivor区容量的上限，则直接移交给老年代。假如一些没有进取心的对象以为可以一直在新生代的Survivor区交换来交换去，那就错了。每个对象都有 个计数器，每次 YGC 都会加1，XX:MaxTenuringThreshold 参数能配置计数器的值到达某个阐值的时候 对象从新生晋升至老年代。
如果该参数配置为1，那么从新生代的 Eden 区直接移至老年代。默认值 是15 可以在Survivor区交换14次之后晋升至老年代。
# 垃圾回收
堆的内存空间既可以固定大小，也可以在运行时动态地调整，通过如下参数设定初始值和最大，比如 Xms256M -Xmxl024M，其中表示它是JVM运行参数，在通常情况下，服务器在运行过程中，堆空间不断地扩容与回缩，势必形成不必要的系统压力 所以在线上生产环境中 JVM ms mx 设置成同样大小，避免在 GC 后调整堆大小时带来的额外压力。
