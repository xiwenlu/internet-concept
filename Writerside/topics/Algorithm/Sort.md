# 排序

## 冒泡排序
1. 两两比较 向后交换
```java 
for(int i=0;i<arrayInt.length-1;i++){
    for(int j=0;j<arrayInt.length-1-I;i++){
        if(arrayInt[j]>arrayInt[j+1]){
            int temp = arrayInt[j];
            arrayInt[j]=arrayInt[j+1]；
            arrayInt[j+1] =  arrayInt[j];
        }
    }
}
```
2. 两两比较向前交换
```java    
for(int i=0;i<arrayInt.length;i++){
    for(int j=i+1;j<arrayInt.length;j++){
        if(arrayInt[i]>arrayInt[j]){
            int temp = arrayInt[i];
            arrayInt[i]= arrayInt[j]；
            arrayInt[j]= arrayInt[i]；
        }
    }
}
```
3. 王梦璇版
```java  
for(int a=0; a<arr.length-1;a++){
    for(int b=0; b<arr.length-1-a;b++){
        if(arr[b]>arr[b+1]){
            int temp =arr[b+1];
            arr[b+1] =arr[b];
            arr[b] =temp;
        }
    }
}
```

