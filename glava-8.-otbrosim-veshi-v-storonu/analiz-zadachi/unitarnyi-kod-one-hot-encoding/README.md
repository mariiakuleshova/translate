# Унитарный код \(One hot encoding\)

Концепция довольно проста. Вместо того, чтобы заменять категорию перечислением, мы добавляем один столбец с нашими данными для каждого значения и устанавливаем его равным 1 или 0, основываясь на этом значении. Название происходит от того, что только один столбец в наборе является "горячим" или выбранным.

Мы можем применить этот принцип к нашему примеру. Мы можем заменить один столбец «Материал», с четырьмя столбцами для каждого типа материала в нашей базе данных. Итак, наша колонка «Материал», становится 4 колонками «керамика», «мех», «металл», «пластик» и «дерево»:

| Материал | Керамика | Мех | Металл | Пластик | Дерево |
| :--- | :--- | :--- | :--- | :--- | :--- |
| металл | 0 | 0 | 1 | 0 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| мех | 0 | 1 | 0 | 0 | 0 |
| мех | 0 | 1 | 0 | 0 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| дерево | 0 | 0 | 0 | 0 | 1 |
| дерево | 0 | 0 | 0 | 0 | 1 |
| дерево | 0 | 0 | 0 | 0 | 1 |
| керамика | 1 | 0 | 0 | 0 | 0 |
| пластик | 0 | 0 | 0 | 1 | 0 |
| пластик | 0 | 0 | 0 | 1 | 0 |
| металл | 0 | 0 | 1 | 0 | 0 |
| дерево | 0 | 0 | 0 | 0 | 1 |
| пластик | 0 | 0 | 0 | 1 | 0 |
| дерево | 0 | 0 | 0 | 0 | 1 |


Это вызывает некоторые структурные осложнения для нашей программы. Мы должны вставить столбцы для каждого из наших типов, которые заменяют три столбца 14 новыми столбцами.

Я нашел две функции, которые мы можем использовать для преобразования текстовых категорий в унитарный код нескольких столбцов. Одним из них является OneHotEncoder, который является частью Scikit-learn. Используется как LabelEncoder - фактически вы должны использовать обе функции одновременно. Вам придется преобразовать строковые данные в числовую форму с помощью LabelEncoder, а затем применить OneHotEncoder, чтобы преобразовать это в форму «один бит на значение», которую мы хотим.

Более простой способ - использовать функцию Pandas get\_dummies \(\). Имя функции такое видимо потому что мы создаем фиктивные значения, чтобы заменить строку числами. Это выполняет та же функция. Шаги являются немного более простыми, чем при использовании OneHotEncoder, так что это мы рассмотрим это на нашем примере.

Верхний раздел заголовка такой же, как и раньше - у нас тот же импорт:

```text
# decision tree classifier
# with One Hot Encoding and Gini criteria
#
# Author: Francis X Govers III
#
# Example from book "Artificial Intelligece for Robotics"
#
from sklearn import tree
import numpy as np
import pandas as pd
import sklearn.preprocessing as preproc
import graphviz
```

Мы начнем с чтения данных нашей таблицы, как и раньше. Я добавил дополнительный столбец для себя под названием название игрушки, чтобы я мог отслеживать, где какая игрушка. Нам не нужен этот столбец для дерева решений, поэтому мы можем вынести его с помощью функции Pandas del, указав имя столбца для удаления:

```text
toyData = pd.read_csv("toy_classifier_tree.csv")
del toyData["Toy Name"] # we don't need this for now
```


Теперь мы собираемся создать список столбцов, которые мы собираемся удалить и заменить из Pandas dataTable. Это столбцы «Цвет», «Жесткость» и «Материал». Я использовал термин «жесткость» для обозначения игрушек, которые были мягкими и легко сдавливались, потому что это отдельный критерий использования руки нашего робота. Мы генерируем «фиктивные» значения и заменяем три столбца 18 новыми столбцами. Pandas автоматически называет столбцы комбинациями старого имени столбца и значения. Например, один столбец Color заменяет на Color\_white, Color\_blue, Color\_green и так далее:

```text
textCols = ['Color','Soft','Material']
toyData = pd.get_dummies(toyData,columns=textCols)
```


Я поместил здесь печать данных, чтобы убедиться, что все собрано правильно. Это необязательно. Я действительно впечатлён Pandas для работы с таблицами данных - там много возможностей для создания функций типа базы данных и анализа данных:

```text
print toyData
```


Теперь мы готовы создать наше дерево решений. Мы создаем экземпляр объекта и называем его dTree, устанавливаем критерии классификации для Джини. Затем мы извлекаем значения данных из нашего toyData dataframe и поместим значения класса в первом \(0-м\) столбце в переменную classValues, используя операторы разбиения массива:

```text
dTree = tree.DecisionTreeClassifier(criterion ="gini")
dataValues=toyData.values[:,1:]
classValues = toyData.values[:,0]
```


Нам все еще нужно преобразовать имена классов в тип перечислений, используя LabelEncoder, как мы это делали в предыдущих двух примерах. Нам не нужен унитарный код. Каждый класс представляет конечное состояние для нашего примера классификации - листья в нашем дереве решений. Если бы мы делали классификатор нейронной сети, это были бы наши выходные нейроны. Одна большая разница - при использовании дерева решений компьютер сообщает вам, какими критериями он пользуется для классификации и разделения предметов. С нейронной сетью, он тоже будет делать классификацию, но у вас не будет возможности узнать, какие критерии были использованы:

```text
lencoder = preproc.LabelEncoder()
lencoder.fit(classValues)
classes = lencoder.transform(classValues)
```


Как мы уже говорили, чтобы использовать имена значений классов в конечном выводе, мы должны исключить любые дубликаты имен и отсортировать их по алфавиту. Следующая пара вложенных функций нужна именно  для этого:

```text
classValues = list(sorted(set(classValues)))
```


Это конец нашей программы. На самом деле создание дерева решений занимает теперь всего одну строчку кода, когда мы настроили все данные. Мы используем те же шаги, что и раньше, и затем создайте графику с помощью графика и сохраните изображение в формате PDF. Это все совсем несложно настроить – особенно теперь, после всей нашей практики:

```text
print ""
dTree = dTree.fit(dataValues,classes)
c_data=tree.export_graphviz(dTree,out_file=None,feature_names=toyData.colum
ns,
class_names=classValues, filled = True,
rounded=True,special_characters=True)
graph = graphviz.Source(c_data)
graph.render("toy_decision_tree_graph_oneHot_gini ")
```

![](../../../.gitbook/assets/image%20%286%29.png)


