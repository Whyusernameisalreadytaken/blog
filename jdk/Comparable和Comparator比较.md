# Comparable和Comparator比较

1. Comparable定义自身的比较器,实现此接口的类的实例对象本身是可比较的
2. Comparator定义外部比较器,当某个类未实现Comparable接口且需要对其进行排序时,可以使用该接口定义一个外部比较器,手动对该类的实例进行排序而又不需要修改源代码
3. 在jdk1.8中,定义Comparable为函数接口,可以使用lambda表达式
4. ​