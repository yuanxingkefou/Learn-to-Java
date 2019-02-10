#Queue

Queue的操作方法：

* queue的增加元素方法add和offer的区别在于，add方法在队列满的情况下将选择抛异常的方法来表示队列已经满了，
  而offer方法通过返回false表示队列已经满了；在有限队列的情况，使用offer方法优于add方法；
* remove方法和poll方法都是删除队列的头元素，remove方法在队列为空的情况下将抛异常，而poll方法将返回null；
* element和peek方法都是返回队列的头元素，但是不删除头元素，区别在与element方法在队列为空的情况下，将抛异常，而peek方法将返回null.


Deque:双向队列

是一种具有队列和栈的性质的数据结构，双向队列中的元素可以从两端弹出，其限定插入和删除在表的两端进行

操作  |头元素|头元素    |尾元素|尾元素
------|------|-------|---------|---------
特殊情况  |抛出异常|特殊值|抛出异常|特殊值
插入|addFirst(e)/push(e)|offerFirst(e)|addLast(e)|offerLast(e)
移除|removeFirst() /remove()/pop()|pollFirst()/poll()|removeLast()|pollLast()
获取|getFirst()/element()|peekFirst()/peek()|getLast()|peekLast()
