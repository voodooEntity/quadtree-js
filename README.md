

<p align="center">
    <a href='LICENCE'><img src="https://img.shields.io/github/license/CorentinTh/quadtree-js.svg" alt="Licence Badge" /></a>
</p>



Modified version of<a href="https://github.com/CorentinTh/quadtree-js" target="_blank"> quadtree-js</a>. 

Readme outdated - update needed. Fork for specific use case.


Import :

```javascript
// EMAScript import
import {QuadTree, Box, Point, Circle} from 'js-quadtree';
// Or Common JS:
const {QuadTree, Box, Point, Circle} = require('js-quadtree');

const quadtree = new QuadTree(new Box(0, 0, 1000, 1000));

quadtree.insert(new Point(100, 200, {custom: 'data'}));

const results = quadtree.query(new Circle(150, 150, 100));
```
### Browser

You can use the CDN:
```html
<script src="https://unpkg.com/js-quadtree"></script>
```
And everything is globally accessible and **prefixed with `QT`**:
```javascript
const quadtree = new QT.QuadTree(new QT.Box(0, 0, 1000, 1000));

quadtree.insert(new QT.Point(100, 200, {custom: 'data'}));

const results = quadtree.query(new QT.Circle(150, 150, 100));
```
 
## Usage
### Creation
```javascript
// Create the bounding area of the quadtree
const boundingArea = new Box(0, 0, 1000, 1000);

// Instantiate  the new quadtree
const quadtree = new QuadTree(boundingArea);
```

You can also specify the following optional parameters:
```javascript
// Create the bounding area of the quadtree
const boundingArea = new Box(0, 0, 1000, 1000);

const config = {
    capacity: 10,            // Specify the maximum amount of point per node (default: 4)
    removeEmptyNodes: true,  // Specify if the quadtree has to remove subnodes if they are empty (default: false).
    maximumDepth: 5,         // Specify the maximum depth of the quadtree. -1 for no limit (default: -1).
    // Specify a custom method to compare point for removal (default: (point1, point2) => point1.x === point2.x && point1.y === point2.y).
    arePointsEqual: (point1, point2) => point1.data.foo === point2.data.foo      
};

// An array of point to insert directly (same as quadtree.insert(points) )
const points = [new Point(10, 10), new Point(52, 64)];

const quadtree = new QuadTree(boundingArea, config, points);
```

### Insert

You can insert a **Point** element, an array of **Point** element, your **own element** as long as it has an **x** and a **y** property or an array of custom element.

```javascript
const point = new Point(10, 25);

const pointArray = [
    new Point(45, 22),
    new Point(30, 60),
    new Point(14, 12)
];

const customPoint = {
    x: 94,
    y: 23,
    customField:{}
};

quadtree.insert(point);
quadtree.insert(pointArray);
quadtree.insert(customPoint);
```

You can add your data in a **Point** element:
```javascript
const myData = {
    foo: 'bar'
};

const point = new Point(50, 50, myData);

console.log(point.data.foo); // 'bar'
```

### Remove

As the **insert** method, you can remove a **Point** element, an array of **Point** element, your **own element** as long as it has an **x** and a **y** property or an array of custom element.

By default, points having the same **x** and **y** values will be removed. To override this behavior, add a method under `arePointsEqual` in the config of the quadtree that takes two points in parameters and return a boolean if the points are equal.
Example: `const quadtree = new QuadTree(boundingArea, {arePointsEqual: (point1, point2) => point1.data.foo === point2.data.foo});` 

```javascript
const point = new Point(10, 25);

const pointArray = [
    new Point(45, 22),
    new Point(30, 60),
    new Point(14, 12)
];

const customPoint = {
    x: 94,
    y: 23
};

quadtree.remove(point);
quadtree.remove(pointArray);
quadtree.remove(customPoint);
```

**Note**: it doesn't have to be the same object, the test is done with the coordinates.

### Query

Use the **query** method to get all the point within a range.


```javascript
// This will return all the points in the given Box
const points = quadtree.query(new Box(10, 10, 100, 100));
```

```javascript
// This will return all the points in the given Circle
const points = quadtree.query(new Circle(10, 10, 100));
```

You can use a **Box** or a **Circle** as a range or even your own range element as long as it has the following methods:
* **contains**: return true if a point is within this range, false otherwise.
* **intersects**: return true if a **Box** intersects with this range, false otherwise.
See [the Box definition](src/Box.js) for a good example.

### Get all the point

If want to retrieve all the point, you can use this method:

```javascript
const points = quadtree.getAllPoints();
```

**Note**: you may want to store your points in a side array since, it have to look trough all the child nodes.

### Get Tree

You can get the amount of points by nodes with the `getTree()` method.

```javascript
const qt = new QuadTree(new Box(0, 0, 10, 10));

qt.insert(new Point(5, 5));
qt.insert(new Point(6, 5));
qt.insert(new Point(4, 5));
qt.insert(new Point(3, 5));
qt.insert(new Point(2, 5));
qt.insert(new Point(1, 5));

console.log(qt.getTree());

// {
//     ne: 2, 
//     nw: {
//         ne: 0, 
//         nw: 0, 
//         se: 3, 
//         sw: 2
//     }, 
//     se: 2,
//     sw: 3
// }
```

### Clear

Use this method to clear the quadtree. It remove all the points and sub-nodes.

```javascript
quadtree.clear();
```

## License

This project is under the [MIT license](LICENSE).

