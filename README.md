int numObjects = 2;
PVector[] positions;
PVector[] velocities;
PVector[] rotations;

int cameraMode = 0;

void setup() {
  size(800, 800, P3D);
  positions = new PVector[numObjects];
  velocities = new PVector[numObjects];
  rotations = new PVector[numObjects];
  
  
  for (int i = 0; i < numObjects; i++) {
    positions[i] = new PVector(random(-200, 200), random(-200, 200), random(-200, 200));
    velocities[i] = new PVector(random(-2, 2), random(-2, 2), random(-2, 2));
    rotations[i] = new PVector(random(-0.05, 0.05), random(-0.05, 0.05), random(-0.05, 0.05));
  }
}

void draw() {
  background(30);
  lights(); 
  
  
  if (cameraMode == 0) {
    
    camera(0, -500, 500, 0, 0, 0, 0, 1, 0);
  } else if (cameraMode == 1) {
    
    camera(positions[0].x, positions[0].y, positions[0].z + 100, 
           positions[0].x, positions[0].y, positions[0].z, 
           0, 1, 0);
  }
  
 
  for (int i = 0; i < numObjects; i++) {
    pushMatrix();
    translate(positions[i].x, positions[i].y, positions[i].z);
    
  
    rotateX(frameCount * rotations[i].x);
    rotateY(frameCount * rotations[i].y);
    rotateZ(frameCount * rotations[i].z);
    

    ambient(100 + i * 30, 100, 200);
    specular(255);
    shininess(10);
    

    if (i % 2 == 0) {
      sphere(30); 
    } else {
      box(50, 50, 50); 
    }
    
    popMatrix();
    
    
    positions[i].add(velocities[i]);
    
    
    if (abs(positions[i].x) > 300 || abs(positions[i].y) > 300 || abs(positions[i].z) > 300) {
      velocities[i].mult(-1);
    }
  }
}

void keyPressed() {
  
  if (key == '1') cameraMode = 0;
  if (key == '2') cameraMode = 1;
}
