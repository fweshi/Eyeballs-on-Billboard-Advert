# Eyeballs-on-Billboard-Advert
The "Eyeballs on Billboard Advert" project aims to explore the effectiveness and impact of billboard advertisements through the collective observations and feedback of multiple participants, metaphorically referred to as "eyeballs." This project aims to contribute to the improvement and innovation of outdoor advertising strategies.

## Getting Started
To contribute to this project, follow these steps:

1. Clone the repository: `git clone https://github.com/fweshi/eyeballs-on-billboard.git`
2. Navigate to the project directory: `cd eyeballs-on-billboard`
3. Observe the billboard advertisement.
4. Provide your feedback by following the instructions in the repository.
5. Submit your feedback or changes via a pull request.

## Contributors
- []

## License
This project is licensed under the [MIT] License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments


import cv2

# Load the image containing the billboard
billboard_image = cv2.imread('billboard_image.jpg')

# Load the image containing faces (assuming it's captured from a camera)
faces_image = cv2.imread('faces_image.jpg')

# Initialize the Haar cascade classifier for face detection
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

# Convert images to grayscale (for face detection)
gray_billboard = cv2.cvtColor(billboard_image, cv2.COLOR_BGR2GRAY)
gray_faces = cv2.cvtColor(faces_image, cv2.COLOR_BGR2GRAY)

# Detect faces in the faces image
faces = face_cascade.detectMultiScale(gray_faces, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

# Iterate over detected faces and check if they overlap with the billboard
for (x, y, w, h) in faces:
    # Calculate the coordinates of the face bounding box
    face_top_left = (x, y)
    face_bottom_right = (x + w, y + h)
    
    # Check if the face bounding box overlaps with the billboard
    if face_top_left[0] < billboard_image.shape[1] and face_top_left[1] < billboard_image.shape[0]:
        # Calculate the area of overlap
        intersection_area = max(0, min(face_bottom_right[0], billboard_image.shape[1]) - max(face_top_left[0], 0)) * \
                            max(0, min(face_bottom_right[1], billboard_image.shape[0]) - max(face_top_left[1], 0))
        
        # Calculate the percentage of the billboard area overlapped by the face
        billboard_area = billboard_image.shape[0] * billboard_image.shape[1]
        overlap_percentage = (intersection_area / billboard_area) * 100
        
        print(f"Face detected with {overlap_percentage:.2f}% overlap on the billboard.")

# Display the images with detected faces (for visualization purposes)
for (x, y, w, h) in faces:
    cv2.rectangle(faces_image, (x, y), (x+w, y+h), (0, 255, 0), 2)

cv2.imshow('Detected Faces', faces_image)
cv2.waitKey(0)
cv2.destroyAllWindows()

