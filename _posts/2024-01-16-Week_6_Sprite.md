---
toc: true
comments: false
layout: post
title: Sprite Animation 
description: starting out sprite animations
type: tangibles
courses: { compsci: {week: 6} }
---

<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="spidermanSprite" src="{{site.baseurl}}/images/spriteman-removebg-preview.png">  // change sprite here
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="run forward" checked>
            <label for="run forward">run forward</label><br>
            <input type="radio" name="animation" id="run left">
            <label for="run left">run left</label><br>
            <input type="radio" name="animation" id="run right">
            <label for="run right">run right</label><br>
            <input type="radio" name="animation" id="runaway" checked>
            <label for="runaway">runaway</label><br>
        </div>
    </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = 102;  // matches sprite pixel width
        const SPRITE_HEIGHT = 100; // matches sprite pixel height
        const FRAME_LIMIT = 5;  // matches number of frames per sprite row, this code assume each row is same

        const SCALE_FACTOR = 1.5;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Spiderman {
            constructor() {
                this.image = document.getElementById("spidermanSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = FRAME_LIMIT;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw dog object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // dog object
        const spiderman = new Spiderman();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'run forward':
                        spiderman.frameY = 0;
                        break;
                    case 'run left':
                        spiderman.frameY = 1;
                        break;
                    case 'run right':
                        spiderman.frameY = 2;
                        break;
                    case 'runaway':
                        spiderman.frameY = 3;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            spiderman.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            spiderman.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            //requestAnimationFrame(animate);
        setTimeout(function() {
        requestAnimationFrame(animate);
        }, 220);
        }

        // run 1st animate
        animate();
    });
</script>