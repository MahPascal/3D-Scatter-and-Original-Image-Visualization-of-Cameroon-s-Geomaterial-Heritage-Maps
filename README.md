# 3D-Scatter-and-Original-Image-Visualization-of-Cameroon-s-Geomaterial-Heritage-Maps
3D Scatter and Original Image Visualization of Cameroon’s Geomaterial Heritage Maps



------------------------------------------------------------

!pip install opencv-python scikit-image matplotlib

---------------------------------------------------------------
!pip install opencv-python matplotlib numpy
-----------------------------------------------------------
!pip install opencv-python numpy matplotlib scikit-image
-----------------------------------------------------
import cv2
import numpy as np
import matplotlib.pyplot as plt
import os
from mpl_toolkits.mplot3d import Axes3D

def resize_images_to_smallest(images):
    """ Resize all images to the smallest height and width among them """
    heights = [img.shape[0] for img in images]
    widths = [img.shape[1] for img in images]
    new_height = min(heights)
    new_width = min(widths)
    resized_images = [cv2.resize(img, (new_width, new_height)) for img in images]
    return resized_images

def visualize_3_geomaps(image_paths, image_titles):
    """ Visualize three images in 3D scatter plots with original image previews """

    # Load and resize images
    images = [cv2.imread(path) for path in image_paths]
    images_resized = resize_images_to_smallest(images)

    # Convert images to grayscale for 3D plotting
    grays = [cv2.cvtColor(img, cv2.COLOR_BGR2GRAY) for img in images_resized]
    coords = [np.meshgrid(np.arange(g.shape[1]), np.arange(g.shape[0])) for g in grays]
    z_vals = [g.flatten() for g in grays]

    # Plotting
    fig = plt.figure(figsize=(18, 12))

    for i in range(3):
        # 3D Scatter
        ax_3d = fig.add_subplot(3, 2, 2 * i + 1, projection='3d')
        x_flat, y_flat = coords[i][0].flatten(), coords[i][1].flatten()
        ax_3d.scatter(x_flat, y_flat, z_vals[i], c=z_vals[i], cmap='viridis', marker='o')
        ax_3d.set_title(f"{image_titles[i]}")
        ax_3d.set_xlabel('X')
        ax_3d.set_ylabel('Y')
        ax_3d.set_zlabel('Pixel Intensity')

        # Original Image
        ax_img = fig.add_subplot(3, 2, 2 * i + 2)
        ax_img.imshow(cv2.cvtColor(images_resized[i], cv2.COLOR_BGR2RGB))
        ax_img.set_title(f"Original: {image_titles[i]}")
        ax_img.axis('off')

    plt.tight_layout()
    plt.show()

# Example usage (update paths as needed)
image_paths = [
    "/content/Geomaterials map of Cameroon (PRECASEM, 2021) A.jpg",
    "/content/Geomaterials map of Cameroon (PRECASEM, 2021) B.jpg",
    "/content/Geomaterials map of Cameroon (PRECASEM, 2021) C.jpg"
]

image_titles = [
    "Geomaterials map of Cameroon (PRECASEM, 2021) A",
    "Geomaterials map of Cameroon (PRECASEM, 2021) B",
    "Geomaterials map of Cameroon (PRECASEM, 2021) C"
]

visualize_3_geomaps(image_paths, image_titles)

------------------------------------------------------




