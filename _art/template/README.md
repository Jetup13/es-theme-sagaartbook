Python Script to cut 483x900 png into the shape of the template.png.

Requires pythong & pillow installed

Place images in the same directory as the script and template.png > Run script > images will be inside a output folder

```

from PIL import Image
import os

def shape_images(template_path, input_folder, output_folder):
    try:
        # Open template image and convert to RGBA
        template = Image.open(template_path).convert("RGBA")

        # Iterate over images in the input folder
        for filename in os.listdir(input_folder):
            if filename.endswith(".png") or filename.endswith(".jpg"):
                # Open the input image
                input_image_path = os.path.join(input_folder, filename)
                input_image = Image.open(input_image_path).convert("RGBA")

                # Resize input image to match the template's size
                input_image = input_image.resize(template.size)

                # Create a mask using the template's alpha channel
                mask = template.split()[3]

                # Apply the mask to the input image
                shaped_image = Image.composite(input_image, Image.new("RGBA", template.size, (0, 0, 0, 0)), mask)

                # Save the result to the output folder
                output_image_path = os.path.join(output_folder, filename)
                shaped_image.save(output_image_path)

        print("Batch processing completed successfully.")
    except Exception as e:
        print(f"Error: {str(e)}")

if __name__ == "__main__":
    # Specify the path to your template image (assumed to be in the same directory)
    template_path = "template.png"

    # Specify the input folder (same as the script's directory)
    input_folder = os.path.dirname(os.path.realpath(__file__))

    # Specify the output folder (in the same directory as the script)
    output_folder = os.path.join(os.path.dirname(os.path.realpath(__file__)), "output")

    # Create the output folder if it doesn't exist
    os.makedirs(output_folder, exist_ok=True)

    # Call the function to shape images
    shape_images(template_path, input_folder, output_folder)

    # Add a prompt to press Enter to exit
    input("Press Enter to exit.")

```