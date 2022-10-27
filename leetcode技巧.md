



## **摩尔投票法**

**剑指 Offer 39. 数组中出现次数超过一半的数字**

时间O(n)，空间O(1)

```java
        int count =0,card=0;

        for(int num:nums){
            if(count==0) card=num;
            count+= card == num?1:-1;
        }
        return card;
```



## **List<Integer>转Int[]**

`list.stream().mapToInt(Integer::valueOf).toArray()`

