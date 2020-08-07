# AlgorithmTemplate
记录做题用的模板,免得每次都现写

## 快速排序
日期:2020年08月07日10:36:47
acwing 785
```c++
//复杂度O(nlogn)
void quick_sort(int q[],int l,int r){
    if(l>=r) return;
    int i=l-1,j=r+1;
    int x=q[l+(r-l)/2];
    while(i<j){
        do i++;while(q[i]<x);
        do j--;while(q[j]>x);
        if(i<j) swap(q[i],q[j]);
    }
    quick_sort(q,l,j);
    quick_sort(q,j+1,r);
}
```
