# Javascript日常积累
    1. void(0)与undefined区别：void(0)永远返回undefined。在void(0)中的计算中存在getvalue的方法。void(0）计算 \sigma 然后返回 undefined。然而，有些算符，比如 [] 和点，计算的结果是一个 Reference，它只记录了基底和属性，并没有进行取值，而取值这个过程有可能有副作用（因为调用了 Getter），所以要多一个 GetValue 布骤。
