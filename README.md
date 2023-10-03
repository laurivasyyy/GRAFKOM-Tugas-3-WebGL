# GRAFKOM TUGAS 3 - WEBGL [2D Square and 3D Cube]

| Nama | NRP |
|-----------------------------|------------|
|Laurivasya Gadhing Syahafidh | 5025211136 |


Drawing a 2-D square by combining two triangles [ Square Preview](https://laurivasyyy.github.io/GRAFKOM-Tugas-3-WebGL/2D-Square.html "square")

Drawing a 3-D rotation cube [Cube Preview](https://laurivasyyy.github.io/GRAFKOM-Tugas-3-WebGL/3D-Cube.html "cube")

## 2D-SQUARE
| Triangle-1 | Triangle-2 | Square |
|-------------------|-------------------|--------------------|
|![triangle1](https://github.com/laurivasyyy/GRAFKOM-Tugas-3-WebGL/blob/65a6158978b0994abcf86dca69a586d1b327b018/assets/triangle-1.png)|![triangle2](https://github.com/laurivasyyy/GRAFKOM-Tugas-3-WebGL/blob/65a6158978b0994abcf86dca69a586d1b327b018/assets/triangle-2.png)|![square](https://github.com/laurivasyyy/GRAFKOM-Tugas-3-WebGL/blob/65a6158978b0994abcf86dca69a586d1b327b018/assets/square.png)|

The square was created by positioning two separate 
triangles at specific coordinates in a way that they 
combine to form a unified square shape. Below is a code 
snippet illustrating how this drawing and positioning were 
accomplished :

``` javascript
function draw() { 
    
        gl.clearColor(0,0,0,1);  
        gl.clear(gl.COLOR_BUFFER_BIT);  
    
        /* FIRST TRIANGLE */
        let  coords1 = new Float32Array( [ -0.5,-0.5, 0.5,0.5, -0.5,0.5 ] );
       
        gl.bindBuffer(gl.ARRAY_BUFFER, bufferCoords);
        gl.bufferData(gl.ARRAY_BUFFER, coords1, gl.STREAM_DRAW);
        gl.vertexAttribPointer(attributeCoords, 2, gl.FLOAT, false, 0, 0);
        gl.enableVertexAttribArray(attributeCoords); 
       
        // Set up values for the "color" attribute 
       
        let  color1 = new Float32Array( [ 0,1,0, 1,0,0, 0,0,1 ] );
    
        gl.bindBuffer(gl.ARRAY_BUFFER, bufferColor);
        gl.bufferData(gl.ARRAY_BUFFER, color1, gl.STREAM_DRAW);
        gl.vertexAttribPointer(attributeColor, 3, gl.FLOAT, false, 0, 0);
        gl.enableVertexAttribArray(attributeColor); 
        
        // Draw the triangle.
       
        gl.drawArrays(gl.TRIANGLES, 0, 3);
        
         /* SECOND TRIANGLE */
         let  coords2 = new Float32Array( [ -0.5,-0.5, 0.5,0.5, 0.5, -0.5 ] );
       
         gl.bindBuffer(gl.ARRAY_BUFFER, bufferCoords);
         gl.bufferData(gl.ARRAY_BUFFER, coords2, gl.STREAM_DRAW);
         gl.vertexAttribPointer(attributeCoords, 2, gl.FLOAT, false, 0, 0);
         gl.enableVertexAttribArray(attributeCoords); 
        
         // Set up values for the "color" attribute 
        
         let  color2 = new Float32Array( [ 0,1,0, 1,0,0, 1,1,1 ] );
     
         gl.bindBuffer(gl.ARRAY_BUFFER, bufferColor);
         gl.bufferData(gl.ARRAY_BUFFER, color2, gl.STREAM_DRAW);
         gl.vertexAttribPointer(attributeColor, 3, gl.FLOAT, false, 0, 0);
         gl.enableVertexAttribArray(attributeColor); 
         
         // Draw the triangle.
        
         gl.drawArrays(gl.TRIANGLES, 0, 3);
    }
```

## 3D-CUBE
![cube](https://github.com/laurivasyyy/GRAFKOM-Tugas-3-WebGL/blob/65a6158978b0994abcf86dca69a586d1b327b018/assets/cube-rotation.gif)

The cube was made  by modifying the initial sample code 
and then animated to visualize each angle by dragging on 
your mouse. Here are snippets of the code that depict the 
drawing and arrangement process :

### Drawing

``` javascript
function draw() { 
    gl.clearColor(0,0,0,1);
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    
    /* Set the value of projection to represent the projection transformation */
    
    mat4.perspective(projection, Math.PI/8, 1, 8, 12);
    
    /* Get the view matrix from the SimpleRotator object.
       There is no modeling transformation in this program. */
    
    let modelview = rotator.getViewMatrix();
        
    /* Multiply the projection matrix times the modelview matrix to give the
       combined transformation matrix, and send that to the shader program. */
       
    mat4.multiply( modelviewProjection, projection, modelview );
    gl.uniformMatrix4fv(u_modelviewProjection, false, modelviewProjection );
    
    /* Draw the six faces of a cube, with different colors. */
    
    // Front (Red)
    drawPrimitive( gl.TRIANGLE_FAN, [1,0,0,1], [ -1,-1,1, 1,-1,1, 1,1,1, -1,1,1 ]);
    // Top (Green)
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,0,1], [ -1,-1,-1, -1,1,-1, 1,1,-1, 1,-1,-1 ]);
    // Right (Blue)
    drawPrimitive( gl.TRIANGLE_FAN, [0,0,1,1], [ -1,1,-1, -1,1,1, 1,1,1, 1,1,-1 ]);
    // Bottom (Yellow)
    drawPrimitive( gl.TRIANGLE_FAN, [1,1,0,1], [ -1,-1,-1, 1,-1,-1, 1,-1,1, -1,-1,1 ]);
    // Left (Magenta)
    drawPrimitive( gl.TRIANGLE_FAN, [1,0,1,1], [ 1,-1,-1, 1,1,-1, 1,1,1, 1,-1,1 ]);
    // Back (Cyan)
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,1,1], [ -1,-1,-1, -1,-1,1, -1,1,1, -1,1,-1 ]);
    
    /* Draw coordinate axes as thick colored lines that extend through the cube. */
    
    gl.lineWidth(4);
    drawPrimitive( gl.LINES, [1,0,0,1], [ -2,0,0, 2,0,0] );
    drawPrimitive( gl.LINES, [0,1,0,1], [ 0,-2,0, 0,2,0] );
    drawPrimitive( gl.LINES, [0,0,1,1], [ 0,0,-2, 0,0,2] );
    gl.lineWidth(1);
}
```

### Javascript rotation by mouse function
``` javascript
function doMouseDrag(evt) {
        if (!dragging) {
            return; 
        }
        var r = canvas.getBoundingClientRect();
        var x = evt.clientX - r.left;
        var y = evt.clientY - r.top;
        var newRotX = rotateX + degreesPerPixelX * (y - prevY);
        var newRotY = rotateY + degreesPerPixelY * (x - prevX);
        newRotX = Math.max(-xLimit, Math.min(xLimit,newRotX));
        prevX = x;
        prevY = y;
        if (newRotX != rotateX || newRotY != rotateY) {
            rotateX = newRotX;
            rotateY = newRotY;
            if (callback) {
                callback();
            }
        }
    }```


