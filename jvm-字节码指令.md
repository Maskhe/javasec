### 加载和存储指令

-   将一个局部变量加载到操纵栈的指令包括：iload、iload_、lload…
    
-   将一个数值从操作数栈存储到局部变量表的指令包括：istore、istore_、lstore…

```
    public static int add(int a,int b){
        int c=0;
        c=a+b;
        return c;
    }
```

对应字节码：

```
0: iconst_0        //常量0压入操作数栈
1: istore_2        //弹出操作数栈栈顶元素，保存到局部变量表第2个位置
2: iload_0         //第0个变量压入操作数栈
3: iload_1         //第1个变量压入操作数栈
4: iadd            //操作数栈中的前两个int相加，并将结果压入操作数栈顶
5: istore_2        //弹出操作数栈栈顶元素，保存到局部变量表第2个位置
6: iload_2         //加载局部变量表的第2个变量到操作数栈顶
7: ireturn         //返回
```


### 运算指令

·加法指令：iadd、ladd、fadd、dadd ·减法指令：isub、lsub、fsub、dsub

### 类型转化指令

处理窄化类型转换（Narrowing Numeric Conversion）时，就必须显式地使用转换指 令来完成，这些转换指令包括i2b、i2c、i2s、l2i、f2i、f2l、d2i、d2l和d2f。窄化类型转换可能会导致 转换结果产生不同的正负号、不同的数量级的情况，转换过程很可能会导致数值的精度丢失

### 对象创建与访问指令

创建类实例的指令：new ·

创建数组的指令：newarray、anewarray、multianewarray ·

访问类字段（static字段，或者称为类变量）和实例字段（非static字段，或者称为实例变量）的 指令：getfield、putfield、getstatic、putstatic ·

把一个数组元素加载到操作数栈的指令：baload、caload、saload、iaload、laload、faload、 daload、aaload 

·将一个操作数栈的值储存到数组元素中的指令：bastore、castore、sastore、iastore、fastore、 dastore、aastore 

·取数组长度的指令：arraylength

·检查类实例类型的指令：instanceof、checkcast


### 操作数栈管理指令

将操作数栈的栈顶一个或两个元素出栈：pop、pop2 

·复制栈顶一个或两个数值并将复制值或双份的复制值重新压入栈顶：dup、dup2、dup_x1、 dup2_x1、dup_x2、dup2_x2 

·将栈最顶端的两个数值互换：swap


### 控制转移指令

·条件分支：ifeq、iflt、ifle、ifne、ifgt、ifge、ifnull、ifnonnull、if_icmpeq、if_icmpne、if_icmplt、 if_icmpgt、if_icmple、if_icmpge、if_acmpeq和if_acmpne

·复合条件分支：tableswitch、lookupswitch

·无条件分支：goto、goto_w、jsr、jsr_w、ret


### 方法调用和返回指令

·invokevirtual指令：用于调用对象的实例方法，根据对象的实际类型进行分派（虚方法分派）， 这也是Java语言中最常见的方法分派方式。 

·invokeinterface指令：用于调用接口方法，它会在运行时搜索一个实现了这个接口方法的对象，找 出适合的方法进行调用。 

·invokespecial指令：用于调用一些需要特殊处理的实例方法，包括实例初始化方法、私有方法和 父类方法。 

·invokestatic指令：用于调用类静态方法（static方法）。

·invokedynamic指令：用于在运行时动态解析出调用点限定符所引用的方法。并执行该方法。前面 四条调用指令的分派逻辑都固化在Java虚拟

方法调用指令与数据类型无关，而方法返回指令是根据返回值的类型区分的，包括ireturn（当返 回值是boolean、byte、char、short和int类型时使用）、lreturn、freturn、dreturn和areturn，另外还有一 条return指令供声明为void的方法、实例初始化方法、类和接口的类初始化方法使用。