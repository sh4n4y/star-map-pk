<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Star Map</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/globe.gl"></script>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
    </style>
</head>
<body>
    <div id="globe-container"></div>
    <script>
        const globe = Globe()(document.getElementById('globe-container'))
            .globeImageUrl('//unpkg.com/three-globe/example/img/earth-night.jpg')
            .backgroundImageUrl('//unpkg.com/three-globe/example/img/night-sky.png')
            .hexPolygonsData([]);
        
        async function fetchPlanets() {
            const response = await fetch('https://api.le-systeme-solaire.net/rest/bodies/');
            const data = await response.json();
            const planets = data.bodies.filter(body => body.isPlanet);
            
            planets.forEach(planet => {
                console.log(`Planet: ${planet.englishName}, Distance: ${planet.semimajorAxis}, Inclination: ${planet.inclination}`);
            });
            
            // Example to visualize planets (mock coordinates for now)
            const planetData = planets.map(planet => ({
                lat: (Math.random() * 180) - 90, 
                lng: (Math.random() * 360) - 180, 
                size: 1.5,
                name: planet.englishName
            }));
            
            globe.pointsData(planetData)
                .pointColor(() => 'red')
                .pointAltitude(d => 0.1)
                .pointLabel(d => d.name);
        }
        
        fetchPlanets();
    </script>
</body>
</html>
