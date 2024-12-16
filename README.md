```
// module aliases
var Engine = Matter.Engine,
	Render = Matter.Render,
	Runner = Matter.Runner,
	Events = Matter.Events,
	Composite = Matter.Composite,
	Composites = Matter.Composites,
	Common = Matter.Common,
	MouseConstraint = Matter.MouseConstraint,
	Mouse = Matter.Mouse,
	Composite = Matter.Composite,
	Vector = Matter.Vector,
	Bounds = Matter.Bounds,
	Body = Matter.Body,
	Bodies = Matter.Bodies;


// sprites
let player1chosen = false
let player1;
let player1x = 75
let player1y;
let player2chosen = false
let player2;
let player2x = 150
let player2y;
let player3chosen = false
let player3;
let player3x = 225
let player3y;
let circleAradius = 25;
let blueguy;

//boolean mechanics
let start = false;
let pause = false;
let tutorialstage = false;
let stage1 = false;
let stage2 = false;
let stage3 = false;

// Borders
let rightWall;
let leftWall;
let ground;
let ceiling;

// Tutorial stage platforms
let tutorialplatform1;
let tutorialplatform2;
let tutorialplatform3;
let tutorialplatform4;
let tutorialplatform5;


function preload() {
	blueguy = loadImage('picoparkblue.png');
}

function setup() {
	World()
}

function World() {
	// create a renderer
	// create an engine

	engine = Engine.create();
	render = Render.create({
		element: document.body,
		engine: engine,
		options: {
			width: innerWidth,
			height: innerHeight,
			wireframes: false, // <-- important
			background: 'rgb(136,136,136)',
		}
	});

	// This lets use draw on the canvas using processing's (non-physics) drawing tools
	createCanvas(windowWidth, windowHeight, render.canvas)


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
	// Composite.add(engine.world, mouseConstraint)


	// creates the players
	player1 = Bodies.circle(75, windowHeight - 100, circleAradius, {
		density: 0.025,
		friction: 1,
		restitution: null,
		render: {
			sprite: {
				texture: "redball4.png",
				xScale: 0.14,
				yScale: 0.14
			}
		}
	});

	player2 = Bodies.circle(150, windowHeight - 100, circleAradius, {
		density: 0.025,
		friction: 1,
		restitution: null,
		render: {
			sprite: {
				texture: "blueball.png",
				xScale: 0.175,
				yScale: 0.175
			}
		}
	});
	
	player3 = Bodies.circle(225, windowHeight - 100, circleAradius, {
		density: 0.025,
		friction: 1,
		restitution: null,
		render: {
			sprite: {
				texture: "goldball.png",
				xScale: 0.375,
				yScale: 0.375
			}
		}
	});

	// WALLS
	leftWall = Bodies.rectangle(0, windowHeight / 2, 60, 2000, {
		isStatic: true,
		density: 1
	});
	rightWall = Bodies.rectangle(windowWidth, windowHeight / 2, 60, 2000, {
		isStatic: true,
		density: 1,
		restitution: 2
	});
	ground = Bodies.rectangle(windowWidth / 2, windowHeight, 10000, 60, {
		isStatic: true,
		density: 1
	});
	ceiling = Bodies.rectangle(windowWidth / 2, 0, 10000, 60, {
		isStatic: true,
		density: 1
	});

	// // Creates buttons to choose how many players to play with
	// var oneplayer = Bodies.circle(300, windowHeight / 2, 100, {
	// 	isStatic: true,
	// 	render: {
	// 		fillStyle: 'white'
	// 	}
	// });
	// var twoplayer = Bodies.circle(windowWidth / 2, windowHeight / 2, 100, {
	// 	isStatic: true,
	// 	render: {
	// 		fillStyle: 'white'
	// 	}
	// });
	// var threeplayer = Bodies.circle(windowWidth - 300, windowHeight / 2, 100, {
	// 	isStatic: true,
	// 	render: {
	// 		fillStyle: 'white'
	// 	}
	// });

	// add the boundaries for the tutorial level
	Composite.add(engine.world, [ground, ceiling, leftWall, rightWall]);

	// run the renderer
	Render.run(render);

	// create runner
	var runner = Runner.create();

	// run the engine
	Runner.run(runner, engine);

}

function keyPressed() {

	// 	// Checks if the game hasn't started yet
	// 	if (start == false) {

	// 		// When "ENTER" is pressed, the game starts.
	// 		if (keyCode == ENTER) {
	// 			start = true

	// 			// adds the player 
	// 			Composite.add(engine.world, [player1, player2]);
	// 		}
	// 	}



	// checks if the game has started.

	if (start == true) {

		// if the 'P' key is pressed then game is paused.
		if (keyIsDown(80) == true) {
			pause = true
		}

		// if the 'O' key is pressed then game is resumed.
		if (keyIsDown(79) == true) {
			pause = false
		}
		// checks if the game is paused and stops the players from moving
		if (pause === false) {

			// If the 'W' key is pressed, player 1 jumps.
			if (keyCode == 87) {
				var p1pos = player1.position
				var up = createVector(0, -2)
				Body.applyForce(player1, p1pos, up)
			}

			// If the 'A' key is pressed, player 1 moves to the left.
			if (keyCode == 65) {
				var p1pos = player1.position
				var left = createVector(-2, 0)
				Body.applyForce(player1, p1pos, left)
			}

			// if the 'D' key is pressed, the player 1 moves to the right.
			if (keyCode == 68) {
				var p1pos = player1.position
				var right = createVector(2, 0)
				Body.applyForce(player1, p1pos, right)
			}
			// If the 'Y' key is pressed, player 2 jumps.
			if (keyCode == 89) {
				var p2pos = player2.position
				var up = createVector(0, -2)
				Body.applyForce(player2, p2pos, up)
			}

			// If the 'G' key is pressed, player 2 moves to the left.
			if (keyCode == 71) {
				var p2pos = player2.position
				var left = createVector(-2, 0)
				Body.applyForce(player2, p2pos, left)
			}

			// if the 'J' key is pressed, the player 2 moves to the right.
			if (keyCode == 74) {
				var p2pos = player2.position
				var right = createVector(2, 0)
				Body.applyForce(player2, p2pos, right)
			}
			// If the up arrow key is pressed, player 3 jumps.
			if (keyCode == UP_ARROW) {
				var p3pos = player3.position
				var up = createVector(0, -2)
				Body.applyForce(player3, p3pos, up)
			}

			// If the left arrow key is pressed, player 3 moves to the left.
			if (keyCode == LEFT_ARROW) {
				var p3pos = player3.position
				var left = createVector(-2, 0)
				Body.applyForce(player3, p3pos, left)
			}

			// if the right arrow key is pressed, the player 3 moves to the right.
			if (keyCode == RIGHT_ARROW) {
				var p3pos = player3.position
				var right = createVector(2, 0)
				Body.applyForce(player3, p3pos, right)
			}
		}
	}
}

function draw() {
	if (start == false) {
		fill('white')
		circle(300, windowHeight / 2, 200)
		circle(windowWidth / 2, windowHeight / 2, 200)
		circle(windowWidth - 300, windowHeight / 2, 200)
		textAlign(CENTER)
		textSize(25)
		fill('black')
		text("1 Player", 300, windowHeight / 2)
		text("2 Players", windowWidth / 2, windowHeight / 2)
		text("3 Players", windowWidth - 300, windowHeight / 2)
	}

	textAlign(LEFT)
	
	if (start == true && player1chosen == true) {
		textSize(15)
		text("player 1", player1.position.x, player1.position.y - 35)
		textSize(25)
		text("player 1 controls:" + "  'A' to move left, 'D' to move right, 'W' to jump", 100, 100)
	}
	if (start == true && player2chosen == true) {
		textSize(15)
		text("player 2", player2.position.x, player2.position.y - 35)
		textSize(25)
		text("player 2 controls:" + "  'G' to move left, 'J' to move right, 'Y' to jump", 100, 200)
	}
	if (start == true && player3chosen == true) {
		textSize(15)
		text("player 3", player3.position.x, player3.position.y - 35)
		textSize(25)
		text("player 3 controls:" + "  'left arrow' to move left, 'right arrow' to move right, 'up arrow' to jump", 100, 300)
	}
}

function mouseClicked() {
	// This is the code for the # of players options at the beginning
	if (start == false && player1chosen == false && player2chosen == false && player3chosen == false) {
		if (mouseX > 200 && mouseX < 400 && mouseY > windowHeight / 2 - 100 && mouseY < windowHeight / 2 + 100) {
			start = true
			player1chosen = true
			player2chosen = false
			player3chosen = false
			clear()
			Composite.add(engine.world, [player1]);
		}
		if (mouseX > (windowWidth / 2) - 100 && mouseX < (windowWidth / 2) + 100 && mouseY > windowHeight / 2 - 100 && mouseY < windowHeight / 2 + 100) {
			start = true
			player1chosen = true
			player2chosen = true
			player3chosen = false
			clear()
			Composite.add(engine.world, [player1, player2]);
		}
		if (mouseX > windowWidth - 400 && mouseX < windowWidth - 200 && mouseY > windowHeight / 2 - 100 && mouseY < windowHeight / 2 + 100) {
			start = true
			player1chosen = true
			player2chosen = true
			player3chosen = true
			clear()
			Composite.add(engine.world, [player1, player2, player3]);
		}
	}
}
