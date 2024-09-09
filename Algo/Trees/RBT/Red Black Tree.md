---
создал заметку: 2024-09-04
---
Red - black tree: a Binary Search Tree where each node also has a colour (which can be red or black)

Approximatly balanced(Приблизительно сбалансированный): Three height is O-(log n)


### Red-Black Tree Properties

Five key properties of Red-Black Trees (CLRS)

RBTs are Bianry Search Trees with: 

1. Каждый узел либо красный, либо чёрный.
2. Корень дерева всегда чёрный.
3. Все листья (NIL-узлы) чёрные.
4. Красный узел не может иметь красного родителя (свойство "красный-красный" запрещено).
5. Любой путь от корня до листа или пустого дочернего узла содержит одинаковое количество чёрных узлов.