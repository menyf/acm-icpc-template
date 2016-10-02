# STL

## Vector
以`vector<int>v;`为例

1. `v.push_back(a);` 
2. `v.clear();`无法真正释放空间，可能会导致MLE。推荐使用`vector<int>().swap(v);`
3. `v.erase();` 
	* `v.erase(it);`或`v.erase(v.begin()+2);`
	*  `b.erase(b.begin(), b.begin()+3) ;        //将(b.begin(), b.begin()+3)之间的元素删除`
4. `v.size();`返回`v`的元素个数
5. `v.back();`返回最后一个元素的引用
6. `v.front();`返回第一个元素的引用
6.  `vector<int> myvector (8,10);        // myvector: 10 10 10 10 10 10 10 10`
7. `fill_n (myvector.begin(),4,20);     // myvector: 20 20 20 20 10 10 10 10`
8. 存储方式，下标范围`[0, v.size()-1]`
9. `v.empty();`
10. `v.insert();`



## List

## Map

## Bitset

## Stack

## Queue

## Priority_queue

## String

## Algorithm