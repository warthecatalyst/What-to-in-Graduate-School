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


## 算法题
We can solve this problem through a greedy algorithm.

There are n stations, where A and B representing the capacity and current passenger number of each station. Therefore, we can get an array C, where $C_i = A_i - B_i, 1≤i≤n$, which represents the remaining space of each station. The time complexity of the process should be O(n) where n stations are calculated once each.

Through array X and Y, we can get a counter array defined as D for each station, where $D_i$ = the number of train(s) that stucked close to station $i$. For the example given in the problem, $D = [1,2,1]$, which means station 1 has one train stucked close to it and station 2 has two trains stucked close to it and station 3 has one train stucked close to it. The time complexity of this process should be $O(m)$, where m trains are calculated once each.

Thus, we can arrange n stations in ascending order of array D and descending order of array C. We name it as O. For the example of this problem, it should be $O = [2,0,1]$, which each represents station 3,0 and 1(minus 1 each). The time complexity of this process should be $O(nlogn)$.

And we can get the trains that are close to station as a two-dimensional array, which defined as T.  For the example of this problem, it should be $T = [[0],[1,0],[1]]$, and T[i] can also be sorted in the ascending order of the sum of remaining space of its neightbour stations.($So T[1]=[1,0]$). The time complexity of this process should be $O(n*mlogm)$.

With all given above , we can solve this through the Java code given below.

```Java
class Solution{
    public int getMinNumberToRescue(int[] C,int[] O,int[][] T,int[] P){
        for(int station:O){
            int[] trains = T[station];
            for(int train:trains){
                if(P[train]>0){
                    int minNum = Math.min(P[train],C[station]);
                    P[train]-=Math.min(P[train])
                    C[station]-=Math.min(C[station]);
                }
            }
        }
        int ans = 0;
        for(int remain:P){
            ans += remain;
        }
        return ans;
    }
} 
```
The Whole Complexity will be $O(mlogm*n)$;

10(a) $A[0]+A[1]>n$

10(b) 
We can solve this through a greedy algorithm. For i =  k to 1, to calculate the step that $ghost_i$ needs to go to n, which is $n-A[i]$. And when $totalSteps ≥ n$, break to calculate.

This process can be described as the Java code below:
```Java
class Solution{
    public int getMaxNumberOfGhostsToSave(int[] A,int k,int n){
        int totalSteps = 0;
        int i = k;
        for(;i>=1;i--){
            totalSteps += n-A[i];
            if(totalSteps>=n){
                break;
            }
        }
        return k-i;
    }
}
```

9(a) We can solve the problem with a binary search algorithm.
Fisrtly, we add d to the end of A and $A = [90,160,200,250,300]$.
And we define left = 0, right = n, and while $left <= right$, we define $mid = (left+right)/2$

if $A[mid] == b$,we can just return mid+1. Or else, if $A[mid] < b, left = mid+1$,else $right = mid$.

And finally, we check if $left==0$. If so, return 1, else we check whether A[left-1] and A[left] is closer to b.

9(b) Similarly, we are just finding abs(min(A[i]+C[i]-n));

So similar to the previous question, to define if $A[mid]+C[mid] == d$, return mid+1. Or else, if $A[mid]+C[mid] < d, left = mid+1$,else $right = mid$.

And finally, we check if $left==0$. If so, return 1, else we check whether A[left-1]+C[left-1] and A[left]+C[left] is closer to d.