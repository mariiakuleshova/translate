# 8.3.5.А\* алгоритм

  
Честно говоря, невозможно написать книгу о робототехнике, не упоминая алгоритм А \*. A \* берет свое начало с робота Шеки в Стэнфордском университете еще в 1968 году. Это была одна из первых роботизированных навигационных карт. Нильс Нильссон и его команда пыталась найти способ перемещаться для Шеки по коридорам в Стэнфорде, и начала пробовать разные алгоритмы. Первый назывался «А1» и так далее. После нескольких итераций команда решила, что сочетание техник сработало лучше всего. В информатике A \* означает буква А сопровождается чем-то еще, и, таким образом, к названию алгоритма добавили звезду.

Концепция процесса A \* очень похожа на то, что мы уже делали с нашими другими планированиями пути. Как и в планирование волнового фронта, мы начинаем с рассмотрения соседей вокруг нашего стартового местоположения. Мы рассчитаем эвристику или оценку для каждого квадрата основанную на двух факторах: расстояние от стартового местоположения и расстояние по прямой линии к цели. Мы будем использовать эти факторы, чтобы найти путь с самой низкой совокупной стоимостью. Мы рассчитаем эту стоимость путем сложения эвристического значения для каждой сетки квадратов, которые является частью пути. 

Формула:

$$
F(n) = g(n) + h(n)
$$

  
где: F = вклад этого квадрата в стоимость пути, g \(n\) = расстояние от этого квадрата до начальная позиции вдоль выбранного пути \(то есть сумма стоимости пути\), а h \(n\) – это расстояние по прямой от этого квадрата до цели.

Это значение представляет собой стоимость или вклад квадрата, если он был частью окончательного пути. Мы выберем квадрат, который будет частью пути с наименьшей совокупной стоимостью. Как и в планировании волнового фронта, мы отслеживаем квадрат предшественник или квадрат, который был пройден до этого, чтобы восстановить наш путь:

![](../../.gitbook/assets/image%20%288%29.png)

  
Предыдущая диаграмма иллюстрирует алгоритм A \* \(A Star\). Каждый квадрат оценивается на основе суммы расстояния вдоль пути от конца к началу \(G\) и оценки оставшегося расстояние до цели \(H\). Желтые квадраты представляют собой выбранный путь.

Вот код  на языке Python, иллюстрирующий работу алгоритма A \*.

Мы сохраняем набор всех квадратов сетки на карте, для которых мы рассчитали значения. Мы назовем его exploredMap.

Наша квадратная сетка карты выглядит так:



```text
# globals
mapLength = 1280
mapWidth = 1200
mapSize = mapLength*mapWidth
map = []
for ii in range(0, mapWidth):
for jj in range(0,mapLength):
mapSq = mapGridSquare()
mapSq.position = [ii,jj]
mapSq.sType =EMPTY
# create obstacles
obstacles = [[1,1],[1,2],[1,3],[45,18],[32,15] …..[1000,233]]
# iterate through obstacles and mark on the map
for pos in obstacles:
map[pos]. sType = OBSTACLE
pathGrid = []
START = [322, 128]
GOAL = [938,523]
def mapGridSquare():
def __init__(self):
self.F_value = 0.0 #total of G and H
self.G_value = 0.0 # distance to start
self.H_value = 0.0 # distance to goal
self.position=[0,0] # grid location x and
y
self. predecessor =None # pointer to
previous square
self.sType = PATH
def compute(self, goal, start):
self.G_value = sum(pathGrid.distance)
self.H_value =
distance(goal.position,self.position
self.F_value = self.G_value + self.H_value
return self.F_value
def reconstructPath(current):
totalPath=[current]
done=False
while not done:
a_square = current.predecessor
if a_square == None: # at start position?
done = True
totalPath.append(a_square)
current = a_square
return totalPath
def A_Star_navigation(start, goal, exploredMap, map):
while len(exploredMap>0):
current = findMin(exploredMap) # the findMin function
returns the grid block data with
the lowest “F” score
if current.position == goal.position:
# we are done – we are at the goal
return reconstructPath(current)
neighbors = getNeighbors(current)
# returns all the neighbors of the current square that are not marked as
obstacles
for a_square in neighbors:
if a_square.predecessor == None:
# square has not been
previously evaluated
old_score =
a_square.F_value
score = a_square.compute(GOAL, START)
exploredMap.append(a_square) # add this to the map
# we are looking for a square that is closer to the goal than our current
position
if a_square.G_value < current.G_value:
a_square.predecessor = current
current = a_square
```



