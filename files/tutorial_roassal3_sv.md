# Tutorial - Roassal3 - Software Visualization
In this tutorial, you will learn how to use Roassal3 to build software visualizations. Roassal3 is already included in Pharo 9. However, if you prefer to use Pharo 8, you can install Roassal3 by executing the following code snippet in a Playground.

```Smalltalk
Metacello new
    baseline: 'Roassal3';
    repository: 'github://ObjectProfile/Roassal3:v0.9.5';
    load.
```

1. In our first task, we have to create circles to represent the elements in the dataset. Then, we add the circles to the canvas, set a layout, and define simple interactions.

```Smalltalk
"Each class is represented as a small circle"
dataset := RSObject withAllSubclasses collect: [ :c | RSEllipse new size: 5; color: Color gray ].

"c is a canvas. This is where we can draw"
c := RSCanvas new.
c addAll: dataset.

"All classes are displayed using a grid layout"
RSGridLayout on: dataset.

"The canvas can be zoomed in / out using keys I and O"
"It can also be navigated using scrollbars"
c @ RSCanvasController.
```

This will produce the following output.
![alt text](../images/tutorial1.png)

2. Then, we can use a normalizer to map software metrics to the size and color of the circles. We also add interactions to select elements and obtain contextual information. 
```Smalltalk
"Each class is represented as a small circle"
dataset := RSObject withAllSubclasses collect: [ :c | RSEllipse new model: c ] as: RSGroup.

"c is a canvas. This is where we can draw"
c := RSCanvas new.
c addAll: dataset.

"We normalize the size of the circles with the number of methods of classes"
RSNormalizer size
	shapes: dataset;
	normalize: #numberOfMethods.

"We normalize the size of the circles with the number of lines of code of classes"
RSNormalizer color
	shapes: dataset;
	from: Color veryVeryLightGray;
	to: Color black;
	normalize: #numberOfLinesOfCode.

"All classes are displayed using a grid layout"
RSGridLayout on: dataset.

"Make each element have a popup text and allow it to be dragged"
dataset @ RSPopup @ RSDraggable.

"The canvas can be zoomed in / out using keys I and O"
"It can also be navigated using scrollbars"
c @ RSCanvasController.
```
