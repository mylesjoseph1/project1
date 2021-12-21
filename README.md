<!DOCTYPE html>
<html>
<head>
    <script id="vertex-shader" type="x-shader/x-vertex">
        attribute vec4 vPosition;
        attribute vec4 vColor;
        varying vec4 fColor;
        uniform vec3 theta;
        
        uniform mat4 transMatrix;
        uniform mat4 modelViewMatrix;
        uniform mat4 projectionMatrix;

        void main()
        {
            // Compute the sines and cosines of theta for each of
            //   the three axes in one computation.
            vec3 angles = radians( theta );
            vec3 c = cos( angles );
            vec3 s = sin( angles );

            // Remeber: thse matrices are column-major
            mat4 rx = mat4( 1.0,  0.0,  0.0, 0.0,
            0.0,  c.x,  s.x, 0.0,
            0.0, -s.x,  c.x, 0.0,
            0.0,  0.0,  0.0, 1.0 );

            mat4 ry = mat4( c.y, 0.0, -s.y, 0.0,
            0.0, 1.0,  0.0, 0.0,
            s.y, 0.0,  c.y, 0.0,
            0.0, 0.0,  0.0, 1.0 );

            mat4 rz = mat4( c.z, s.z, 0.0, 0.0,
            -s.z,  c.z, 0.0, 0.0,
            0.0,  0.0, 1.0, 0.0,
            0.0,  0.0, 0.0, 1.0 );

            fColor = vColor;

            gl_Position = projectionMatrix * modelViewMatrix * transMatrix * rz * ry * rx * vPosition;

            gl_Position.z = -gl_Position.z;
        }
    </script>

    <script id="fragment-shader" type="x-shader/x-fragment">
        precision mediump float;

        varying vec4 fColor;
        uniform bool useBlack;


        void main()
            {
                if (useBlack)
            {
                gl_FragColor = vec4( 0.0, 0.0, 0.0, 1.0 );
            }
                else
            {
            gl_FragColor = fColor;
            }
        }
    </script>

    <script type="text/javascript" src="Common/webgl-utils.js"></script>
    <script type="text/javascript" src="Common/initShaders.js"></script>
    <script type="text/javascript" src="Common/MV.js"></script>
    <script type="text/javascript" src="CubeShade.js"></script>
</head>

<body>
    <canvas id="gl-canvas" width="512" height="512">
        Oops ... your browser doesn't support the HTML5 canvas element
    </canvas>

    <br />
    
    <button id="xRotate">Rotate X</button>
    <button id="yRotate">Rotate Y</button>
    <button id="zRotate">Rotate Z</button>
</body>
</html>
