# ZhePong

Commit 2 changes
$ git diff
diff --git a/js/scripts.js b/js/scripts.js
index d6afbc0..92c0bb6 100644
--- a/js/scripts.js
+++ b/js/scripts.js
@@ -9,7 +9,10 @@ player = {
        width:20,
        height: 100,

-       update: function() {},
+       update: function() {
+               if (keystate[UpArrow]) this.y -= 7;
+               if (keystate[DownArrow]) this.y += 7;
+       },
        draw: function() {
                ctx.fillRect(this.x, this.y, this.width, this.height);
        }
@@ -28,9 +31,36 @@ ai = {
 ball = {
        x: null,
        y: null,
+       vel: null,
        side: 20,
-
:...skipping...
diff --git a/js/scripts.js b/js/scripts.js
index d6afbc0..92c0bb6 100644
--- a/js/scripts.js
+++ b/js/scripts.js
@@ -9,7 +9,10 @@ player = {
        width:20,
        height: 100,

-       update: function() {},
+       update: function() {
+               if (keystate[UpArrow]) this.y -= 7;
+               if (keystate[DownArrow]) this.y += 7;
+       },
        draw: function() {
                ctx.fillRect(this.x, this.y, this.width, this.height);
        }
@@ -28,9 +31,36 @@ ai = {
 ball = {
        x: null,
        y: null,
+       vel: null,
        side: 20,
-
-       update: function() {},
+       speed: 5,
+
+       update: function() {
+               this.x += this.vel.x;
+               this.y += this.vel.y;
+
+               if (0 > this.y || this.y + this.side > HEIGHT) {
+                       var offset = this.vel.y < 0 ? 0 - this.y : HEIGHT - (this.y + this.side);
+                       this.y += 2*offset; // Duvar carpma gorsellerini gelistirdi.
+
+                       this.vel.y *= -1;
+               }
+
+               var AABBIntersect = function (ax, ay, aw, ah, bx, by, bw, bh) {
+                       return ax < bx+bw && ay < by+bh && bx < ax+aw && by < ay+ah;
+               };
+
+               var pdle = this.vel.x < 0 ? player : ai;                        //chooses which side will be bounced
+               if (AABBIntersect(pdle.x, pdle.y, pdle.width, pdle.height, this.x, this.y, this.side, this. side)
+               ) {
+                       console.log(pi);
+                       this.x = (pdle === player) ? player.x + player.width : ai.x - this.side;
+                       var n = (this.y + this.side - pdle.y) / (pdle.height + this.side);
+                       var phi = 0.25*pi*(2*n-1);                      //pi/4 = 45 derece
+                       this.vel.x = (pdle === player ? 1 : -1) * this.speed*Math.cos(phi);             // ball bounces from players.
+                       this.vel.y = this.speed*Math.sin(phi);
+               }
+       },
        draw: function() {
                ctx.fillRect(this.x, this.y, this.side, this.side);
:
