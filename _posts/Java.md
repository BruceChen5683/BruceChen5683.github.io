- 大小端 ByteOrder定义了写入buffer时字节的顺序 **java默认是big-endian**
```
2个内置的ByteOrder ByteOrder.BIG_ENDIAN和ByteOrder.LITTLE_ENDIAN
ByteOrder.nativeOrder() 返回本地jvm运行的硬件的字节顺序.使用和硬件一致的字节顺序可能使buffer更加有效.
ByteOrder.toString() 返回ByteOrder的名字,BIG_ENDIAN或LITTLE_ENDIAN
```

- big little endian,e.g.
```
/**
 * Feb 25, 2011 by dzh
 */
package buffer.endian;

import java.nio.ByteBuffer;
import java.nio.ByteOrder;

/**
 * @author dzh
 *
 */
public class ByteOrderTest {
	public static void main(String[] args) {
		ByteBuffer buf =ByteBuffer.allocate(4);
		System.out.println("Default java endian: "+buf.order().toString()); 
		
		buf.putShort((short) 1);
		buf.order(ByteOrder.LITTLE_ENDIAN);
		System.out.println("Now: "+buf.order().toString());
		buf.putShort((short) 2);
		
		buf.flip();
		for(int i=0;i<buf.limit();i++)
			System.out.println(buf.get()&0xFF); 
		
		System.out.println("My PC: "+ByteOrder.nativeOrder().toString());
	}
}

//结果
Default java endian: BIG_ENDIAN
Now: LITTLE_ENDIAN
0
1
2
0
My PC: LITTLE_ENDIAN
```

- ByteBuffer  **allocate(int capacity)以及allocateDirect(int capacity)**  ![avatar](http://dl.iteye.com/upload/attachment/536185/9df7e3c7-8126-3c5f-8a25-0ff0a12ae39a.png)
```
为什么要提供两种方式呢？这与Java的内存使用机制有关。第一种分配方式产生的内存开销是在JVM中的，而另外一种的分配方式产生的开销在JVM之外，以就是系统级的内存分配。当Java程序接收到外部传来的数据时，首先是被系统内存所获取，然后在由系统内存复制复制到JVM内存中供Java程序使用。所以在另外一种分配方式中，能够省去复制这一步操作，效率上会有所提高。可是系统级内存的分配比起JVM内存的分配要耗时得多，所以并非不论什么时候allocateDirect的操作效率都是最高的。以下是一个不同容量情况下两种分配方式的操作时间对照： 
```

- 最好将线程组看成事一次不成功的尝试，你只要忽略它就好了  承诺升级理论