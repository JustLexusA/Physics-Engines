# Physics-Engines
REAL Physics game (2d)

```
// module aliases
var Engine = Matter.Engine,
	Render = Matter.Render,
	Runner = Matter.Runner,
	Bodies = Matter.Bodies,
	Body = Matter.Body,
	Composite = Matter.Composite,
	Events = Matter.Events,
	Mouse = Matter.Mouse,
	MouseConstraint = Matter.MouseConstraint;

let circleA;

function setup() {
	// create an engine
	var engine = Engine.create();

	// create a renderer
	var render = Render.create({
		element: document.body,
		engine: engine
	});

	// create two boxes and a ground
	var boxA = Bodies.rectangle(350, 200, 80, 80);
	var boxB = Bodies.rectangle(425, 100, 180, 20);
	circleA = Bodies.circle(375, 50, 25);
	var pentagon = Bodies.polygon(200, 150, 5, 100);
	var leftWall = Bodies.rectangle(0, 305, 60, 810, {
		isStatic: true
	});
	var rightWall = Bodies.rectangle(775, 305, 60, 810, {
		isStatic: true
	});
	var ground = Bodies.rectangle(400, 610, 810, 60, {
		isStatic: true
	});
	var ceiling = Bodies.rectangle(400, 0, 810, 60, {
		isStatic: true
	});


	// creates a mouseconstraint
	var mouse = Mouse.create(render.canvas),
		mouseConstraint = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				stiffness: 0.2,
				render: {
					visible: false
				}
			}
		});
	// adds the mouseconstraint
	Composite.add(engine.world, mouseConstraint)

	// add all of the bodies to the world
	Composite.add(engine.world, [boxA, boxB, circleA, pentagon, ground, ceiling, leftWall, rightWall]);

	// run the renderer
	Render.run(render);

	// create runner
	var runner = Runner.create();

	// run the engine
	Runner.run(runner, engine);

}
// PUT THIS IN A DIFFERENT TAB IN OPENPROCESSING!!
function keyPressed() {
	if (keyCode == UP_ARROW) {
		var pos = circleA.position
		var upforce = createVector(0, -0.04)
		Body.applyForce(circleA, pos, upforce)
	}
}

