# 算法与数据结构(Java语言设计)
## 引论
牢记递归的四条基本法则：
1、基准情形：必须要存在某些基准情形不用递归就能求解出
2、不断推进：对于那些需要递归求解的场景，每一次递归调用都必须要使情况朝向某一种基准情形推进
3、设计法则：假设所有的递归调用都能运行
4、合成效益法则(compound interest rule)：在求解一个问题的同一实例时，切勿在不同的递归调用中做重复的工作

Java中数组是协变的，但集合类型不是，但可以使用通配符来弥补该不足
```Java
public static <AnyType extends Comparable<? super AnyType>>
AnyType findMax(AnyType[] arr){
    int maxIndex = 0;
    for(int i=1;i<arr.length;i++){
        if(arr[i].compareTo(arr[maxIndex])>0){
            maxIndex = i;
        }
    }
    return arr[maxIndex];
}

//Generic findMax, with a function Object.
//Precondition: a.size()>0
class CompareObjects
{
    public static <AnyType> AnyType findMax(AnyType[] arr,Comparator<? super AnyType> cmp){
        int maxIndex = 0;
        for(int i = 1;i<arr.length;i++){
            if(cmp.compare(arr[i],arr[maxIndex])>0){
                maxIndex = i;
            }
        }
        return arr[maxIndex];
    }
}

class CaseInsensitiveCompare implements Comparator<String>
{
    public int compare(String lhs,String rhs){
        return lhs.compareToIgnoreCase(rhs);
    }
}

class TestProgram
{
    public static void main(String[] args){
        String[] arr = {"ZEBRA","alligator","crocodile"};
        System.out.println(findMax(arr,new CaseInsensitiveCompare()));
    }
}
```


## 雪花UUID算法
