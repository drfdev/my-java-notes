### Сортировка по умолчанию

The implementation was adapted from Tim Peters's list sort for Python ( TimSort ).
It uses techniques from Peter McIlroy's "Optimistic Sorting and Information Theoretic Complexity",
in Proceedings of the Fourth Annual ACM-SIAM Symposium on Discrete Algorithms, pp 467-474, January 1993.

https://svn.python.org/projects/python/trunk/Objects/listsort.txt

---

Сортировка по умолчанию `TimSort`  
Страница на википедии: https://en.wikipedia.org/wiki/Timsort

Гибридный стабильный алгоритм, объединение merge sort (сортировка слиянием) и insertion sort (сортировка вставками)  
Лучший результат: `O(n)`  
Худший результат: `O(n log(n))`  

