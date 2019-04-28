---
layout: post
title:  "Sorting list of dictionaries by multiple keys and order"
date:   2019-04-28 22:33:00 +0900
categories: python
---
```python
from functools import cmp_to_key
from collections import OrderedDict

def multi_key_sort(items: list, key_order: list):
    """
    Sort items.
    :param items: dict list
    :param key_order: [(KEY, ORDER), ...]
                     KEY is comparison key and ORDER is sort order.
                     If ORDER is 1, sort ascending order.Otherwise descending order.
    :return: sorted dict list
    """

    key_order = OrderedDict(key_order)

    comparer_iter = []
    for key, order in key_order.items():
        comparer_iter.append((key, order))

    def compare(item_a, item_b):
        if item_a > item_b:
            return 1
        elif item_a == item_b:
            return 0
        else:
            return -1

    def comparer(item_a, item_b):
        for sort_key, sort_order in comparer_iter:
            result = compare(item_a[sort_key], item_b[sort_key])
            if result:
                return sort_order * result
        else:
            return 0

    return sorted(items, key=cmp_to_key(comparer))

# Usage
# dict_list = [{'a': 1, 'b': 5}, {'a': 6, 'b': 3}, {'a': 3, 'b': 4}]
# multi_key_sort(dict_list, [('a', 1), ('b', -1)]

```
